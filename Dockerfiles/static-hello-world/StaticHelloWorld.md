# Static Hello World

1. start a docker container, 
2. build static HTML, 
3. serve it using Nginx

## Commands

After // are my comments

```
vim Dockerfile // or use your editor
i // insert mode
FROM nginx:alpine

COPY . /usr/share/nginx/html
Esc // quits insert mode
:wq // write & quit
vim index.html
i
<h1>Hello World</h1>
Esc
:wq
// build the image
docker build -t static-hello:v1 .
docker images | grep static 
// you should see something similar to:
//static-hello      v1      ef1e4524b052        About a minute ago   16.82 MB
//notice name: static-hello and version: v1)
//Expose a port
docker run -d -p 80:80 static-hello:v1
// not :latest, we have v1! 
# Also, if your host port 80 is taken, use something else, like 8989

// take a peek
curl docker[:your-port-here-if-you-used-other-than-80]
// if this doesn't work, use curl localhost with the port
// <h1>Hello World</h1>

## Docker build command output

----
Sending build context to Docker daemon 4.608 kB
Step 1 : FROM nginx:alpine
alpine: Pulling from library/nginx
128191993b8a: Pull complete 
eb7d4e7a5a24: Pull complete 
7bf750ac70e1: Pull complete 
3313627c7838: Pull complete 
Digest: sha256:40e95eef5082a5b26c5fd9441bd7ee6bcda1bb5f9fbf7a4a1ef9b2b0f88d5c43
Status: Downloaded newer image for nginx:alpine
 ---> e9b19873bc49
Step 2 : COPY . /usr/share/nginx/html
 ---> ef1e4524b052
Removing intermediate container 0793a5ce598e
Successfully built ef1e4524b052
----
