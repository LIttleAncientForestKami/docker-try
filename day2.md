# Let's start a container

Here it's `redis`, choose your own if you wish.

`docker search redis`

Which one? The official one is safest bet.

`docker run redis[:latest]`
By default `:latest` is implied, so I'm marking it here with [], as optional.

Is it running? Well, it is, but we're stuck with it. Let's run in the background, as a kinda **d**eamonized process:
`docker run -d redis`
I skipped `:latest` since it's implied either way.

But... does it run now?
`docker ps`

And... how it's doing? Like, config... logs?
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

-d = in the background
-it = interactive
	certain images allow to override (then you may have Ubuntu image that runs shell and Ubuntu image that runs OS command...)
	docker run ubuntu ps
	docker run -it ubuntu bash

**Exercises:** 

1. start a docker container, build static HTML, serve it using Nginx. If stuck, look at `Dockerfiles/static-hello-world`.
2. have a GNU/Linux dev box with Java 8, Maven and Git installed (search DockerHub for Java 8 and you will find few interesting ones, like [Kai Winter's one](https://hub.docker.com/r/kaiwinter/docker-java8-maven/), if stuck, look at **Java8 image** below.
3. 

## Java8 image

Several ways to have one, to be honest, but they boil down to two choices.

1. Pull one from the Registry. Search the hub, choose, pull.
2. Build one (from either 1. above, or from bare-bones OS Docker image).

You can find one you want (or one that fits you): 

`docker pull stephenreed/jenkins-java8-maven-git`
`docker pull kaiwinter/docker-java8-maven`

Or you can choose that as your base and build upon it (for instance, adding Git to Kai's Docker image). Or you can choose your preferred distro and 'fit it' with Java 8 (Oracle one? OpenJDK? Other?).
