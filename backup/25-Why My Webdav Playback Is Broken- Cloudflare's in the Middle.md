The title basically tells everything, which I finally figured it out after an unintentional file uploading.

I run a frp server on the VPS to expose the Webdav hosted on my local laptop. As usual I put a Cloudflare proxy on DNS. That's the background.

It worked actually very fine and all functions seemed to be good. But the video playback was so laggy that most of the time I even could not start playing. I thought the problem was the poor network between my laptop and the VPS in Tokyo.

Recently I tried to upload a 140MB file through Webdav to my laptop, but it always failed after an around 100MB transaction and got a **413 Request Entity Too Large** HTTP error. This error was given by [Cloudflare](https://developers.cloudflare.com/support/troubleshooting/http-status-codes/4xx-client-error/#413-payload-too-large-rfc7231), and till then I knew about the 100MB limitation for the first time.

After I switched my DNS setting to no proxy, the file uploading issue was gone, so as the video playback one.