# Let's start a container

Here it's `redis`, choose your own if you wish.

`docker search redis`

Which one? The official one is safest bet.

`docker run redis[:latest]`
By default `:latest` is impled.

Is it running? Well, it is, but we're stuck with it. Let's run in the background:
`docker run -d redis`
I skipped `:latest` since it's implied either way.

But... does it run now?
`docker ps`

And... how it's doing?
`docker inspect elated_hawking

docker logs elated_hawking`

Instead of `elated_hawking` use either your container name OR it's ID.

So, it runs. I've taken a look at logs. I've inspected config. But, how do I knock on it?
Basically, you need to EXPOSE the port. In Redis' example, the default port is 6379. But even if on a container, this port is used... on a guest, we need to map to some port. Any port!

`docker run -d --name redisHostPort -p 6379:6379 redis[:latest]`

I named that container `redisHostPort` now. And I set the port to... hmmm. Which one is guest, which is host? Docs say:

    -p <host-port>:<container-port>

But... if I want to launch 5 Redis instances... do I need to repeat the command 5 times, each time changing port? And name?
Name must be unique, otherwise, conflict error message pops out:

    conflict, container name .....YourNameHere.... used by ...ID-here... remove or rename

Port also needs to be unique. So - dynamic port!
`docker run -d --name redisDynamic -p 6379 redis`

Excellent! Now we have a random host port being mapped to 6379 port on our `redisDynamic` container! 
	Wait!
		Which port on our host should I knock on then to get to my Redis?!

`docker port redisDynamic 6379
# or
docker ps`

OK! It runs fine and dandy! However! Removing and creating container erases data. We don't want to lose data!

Enter **VOLUMES**. Directories are volumes. Containers are stateless (they don't hold state). Bindind is a two-way road:


` -v <guest-dir><container-dir>`
Mount the `guest-dir` and changes there are reflected on a container.

`docker run -d --name redisMapped -v /opt/docker/data/redis:/data redis`\
	To map current directory: `$PWD`

-d = w tle
-it = powłoka
	certain images allow to override (then you may have Ubuntu image that runs shell and Ubuntu image that runs OS command...)
	docker run ubuntu ps
	docker run -it ubuntu bash


start a docker container, build static HTML, serve it using Nginx

vim Dockerfile
i
FROM nginx:alpine

COPY . /usr/share/nginx/html
Esc
:wq
vim index.html
i
<h1>Hello World</h1>
Esc
:wq
docker buid -t webserver-image:v1 .
docker images
   (name: webserver-image, version: v1)
A port? Trza wystawić port:
 -p <host-port>:<container-port>
docker run -d -p 80:80 webserver-image:v1
docker run -d -p 80:80 webserver-image - nie ma :latest, bo mamy v1 tylko, a nie :latest!

dobrać się do?
curl docker

JAK TO DZIAŁA? JAK curl docker DZIAŁA?

