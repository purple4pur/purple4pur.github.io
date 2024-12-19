**DISCLAIMER: This project is for study only, and does nothing with to-server requests. Use with caution and at your own risk.**

### Background

I really enjoy playing a game, which handles almost everything on client side, and its server side is for ranking, replays, online matches, etc. As always, the server has a bad network connection for players in mainland China like me. So I was thinking if I could setup a relay server on my VPS, to provide a better connection of the game.

### 1. Setup the relay server

What I was thinking is a topology as follows:

```
+----------+     https     +----------+     https     +----------+
|          ---------------->          ---------------->          |
|          |               |          |               |  Actual  |
|  Client  |               |   Relay  |               |  server  |
|          |               |          |               |          |
|          <----------------          <----------------          |
+----------+     https     +----------+     https     +----------+
```

Actually it was going quite well. I use Traefik in the frontend to route all related requests to the backend. And in the backend there is a nginx instance to do a just `proxy_pass` job.

One thing I'd like to mention is how nginx is dealing with SSL/TLS certification. Requests it received from the frontend is http w/o TLS. I had to add the marked lines to make it work with actual server side TLS certification:

```nginx
server {
  listen 8080;
  server_name localhost;        # <--
  location / {
    resolver 8.8.8.8;
    proxy_pass https://actual.server.com$request_uri;
    proxy_ssl_server_name on;   # <--
    proxy_ssl_session_reuse on; # <--
  }
}
```

Let's take a closer look of the topology now:

```
                           +----------------------------------------------+
                           |                 Relay server                 |
                           |                                              |
+----------+     https     |   +---------+       http       +---------+   |     https     +----------+
|          -------------------->         ------------------->         -------------------->          |
|          |               |   |         |                  |         |   |               |  Actual  |
|  Client  |               |   | Traefik |                  |  nginx  |   |               |  server  |
|          |               |   |         |                  |         |   |               |          |
|          <--------------------         <-------------------         <--------------------          |
+----------+     https     |   +---------+       http       +---------+   |     https     +----------+
                           |                                              |
                           +----------------------------------------------+
```

So it works... Does it? Yeah technically it works very well, for doing the relay job. But here's an issue that about half of the time I can't hold a stable network connection between my client and the relay server! Again, many thanks to THE FIREWALL. So it comes the bad news that the relay server doesn't do any help of a better connection.

But I would not stop here. Quickly I realized the whole traffic between the client and the server was going through my relay server, which meant that I could monitor anything in it, or even manipulate it as I want.

Let's insert a middleware within the relay server!

### 2. Insert the middleware

I'd like to get rid of all the SSL/TLS stuff. So the best place are there: between Traefik and nginx. I put a http proxy in Golang there (first time Golang experience!).

The http proxy instance is a modified version of [wtsi-hgi/http-proxy-logger](https://github.com/wtsi-hgi/http-proxy-logger), with which I can dump the traffic I care to try to understand the packet structure. Now the topology looks like this:

```
                           +-------------------------------------------------------------+
                           |                         Relay server                        |
                           |                                                             |
+----------+     https     |  +---------+   http   +-------------+   http   +---------+  |     https     +----------+
|          ------------------->         -----------|-----(#)-----|---------->         ------------------->          |
|          |               |  |         |          |             |          |         |  |               |  Actual  |
|  Client  |               |  | Traefik |          |  Http Proxy |          |  nginx  |  |               |  server  |
|          |               |  |         |          |             |          |         |  |               |          |
|          <-------------------         <----------|-----(#)-----|-----------         <-------------------          |
+----------+     https     |  +---------+   http   +-------------+   http   +---------+  |     https     +----------+
                           |                                                             |
                           +-------------------------------------------------------------+

                                                         (#) - dump requests/responses
```

Everything is under my control. Can I do some funny things to the traffic?

### 3. Trick the client

Thanks to other excellent open-source private server implements, I can find out what messages are sent back to the client by directly reading their source code. It caught my attention that a very bit indicates the user is a supporter, which unlocks some more options on the client side (, and client side only!).

It took me a bit longer time to exactly understand how the bytes are transferred in the response body and extract them in Golang [(code)](https://github.com/purple4pur/goup). And finally I was very surprised that the whole flow worked! Here's the topology now:

```
                           +-------------------------------------------------------------+
                           |                         Relay server                        |
                           |                                                             |
+----------+     https     |  +---------+   http   +-------------+   http   +---------+  |     https     +----------+
|          ------------------->         -----------|-------------|---------->         ------------------->          |
|          |               |  |         |          |             |          |         |  |               |  Actual  |
|  Client  |               |  | Traefik |          |  Http Proxy |          |  nginx  |  |               |  server  |
|          |               |  |         |          |             |          |         |  |               |          |
|          <-------------------         <----------|-----(*)-----|-----------         <-------------------          |
+----------+     https     |  +---------+   http   +-------------+   http   +---------+  |     https     +----------+
                           |                                                             |
                           +-------------------------------------------------------------+

                                                         (*) - trick here!
```

### What's next?

Well, when I found that special bit, I was asking more options that the client could give me. But the reality is it only gives me about 50% of my expectation. It's not a actual issue because I was giving a best guess of it.

So I'm planing some further steps from here, including bringing back a monitor-only mode, finding more interesting bytes in the traffic, and one more step: can I do some server side jobs? Now I'm so curious if it's possible to implement the friend ranking in the relay side.

Will see later (if any)!