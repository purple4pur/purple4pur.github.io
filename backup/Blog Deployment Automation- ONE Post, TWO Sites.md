### Background

Before this post, I was hosting this blog on github.io only, which is quite enough for internet access. But GitHub and github.io are facing connection issues in mainland China these days. What I'm trying to achieve is to deploy the blog to my server as well, which provides relatively easy access for viewers in China, and a customized URL.

This post shows the full view of my completely automated workflow of deploying this blog to two sites, along with detailed per step descriptions.

### Workflow: the full view

```
                 GitHub
+---------------------------------------+
|      +--------------------------+     |
|      | [A] write posts in Issue |     |
|      +-------------|------------+     |
|                    |                  |
|         +----------v----------+       |
| +-------- [B] generate & push |       |
| |       |  (GitHub Actions)   |       |
| |       +----------|----------+       |
| |                  |                  |
| |     +------------v------------+     |
| |     | [C] deploy to github.io <------- internet access
| |     |    (GitHub Actions)     |     |
| |     +------------|------------+     |
| |                  |                  |                    My Server
| | +----------------v----------------+ |             +---------------------+
| | | [D] request server side pulling | |   REST API  | +-----------------+ |
| | |        (GitHub Actions)         ------------------>                 | |
| | +---------------------------------+ |             | | [E] task runner | |
| |   git push                          |   git pull  | |   (OliveTin)    | |
| +------------>[blog repo]<-----------------------------                 | |
|                    |                  |             | +--------|--------+ |
|                    |                  | new commits |          v          |
|                    +------------------------------------->[blog repo]     |
+  -------------------------------------+             |          |          |
                                                      | +--------v-------+  |
                                                      | | [F] web server <---- internet access
                                                      | |    (nginx)     |  |
                                                      | +----------------+  |
                                                      +---------------------+
```

### Step [A], [B] and [C]

These steps are all in help with or provided by [Gmeek](https://github.com/Meekdai/Gmeek) , the GitHub-based blog tool (Shout-out to this really amazing tool!).

When I open or edit an issue, Gmeek will check if it's a valid issue for a blog post, then generate the corresponding static web files, push changes to current repository, finally deploy to GitHub page (github.io).

After that, <https://purple4pur.github.io/> is ready for internet access. Here is where the previous flow ends.

Check [`Gmeek.yml`](https://github.com/purple4pur/purple4pur.github.io/blob/main/.github/workflows/Gmeek.yml) for details.

### Step [D]

To automatically update the blog in remote, I add a new GitHub Actions workflow right after when [C] completes. In this workflow a POST request will be sent to the task runner hosted on my server, telling it to do a git pull on the server side, then check the command result returned from the server.

Check [`pull_from_server.yaml`](https://github.com/purple4pur/purple4pur.github.io/blob/main/.github/workflows/pull_from_server.yaml) for details.

### Step [E]

Here is a task runner ([OliveTin](https://github.com/OliveTin/OliveTin)) that can run any predefined commands, with REST API provided. I have prepared a git work directory and a git pull command for it. So when this task is triggered, the local blog repository will fetch all changes from GitHub.

### Step [F]

A nginx server is hosting the local blog repo to provide internet access. What I didn't mention is the OliveTin instance and the nginx instance are all running in docker. So the tricky part here is they share a common volume to update or access the local repository.

Check [docker compose file](https://github.com/purple4pur/docker_compose/tree/master/blog) for details.

### Conclusion

This a my first time to try to connect all these services to do sequential jobs in such a workflow, and the results please me so well. Docker and the router Traefik actually make them much simpler and portable. I'm happy to take all-in-docker and continually having more fun with it.