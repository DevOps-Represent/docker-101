## The Basics

Note: Before proceeding, make sure that you've gone through the prerequisites! Quite important, yanno?

### Basic Concepts

Docker is in use now by a lot of companies when deplooying applications to the cloud. However, to explain why Docker is important, we may need to go back a couple of years to revisit how we used to deploy applications.

In the past, we used to deploy applications against operating systems installed on hardware that we bought. Much like how you'd install software on your Windows operating system, which is installed on your laptop.

Over time, we started having [Hypervisors](https://en.wikipedia.org/wiki/Hypervisor) - software that would allow you to emulate hardware, and install *another* operating system on top of it. Conceptually, it all stacks up like this:

![Virtualization](/images/1-vms.png)

You would install the Hypervisor, and then "*fake*" hardware that you can install an operating system in. 

During the last couple of years, people have started using Docker. Compared to Virtual Machines, it all stacks up like this:

![Containerization](/images/2-containers.png)

The operating system ( or to be more correct, the [kernel](https://en.wikipedia.org/wiki/Kernel_(operating_system)) ) began to be reused, and all you had to do was pack up *just what you need* - the libraries, or anything "uncommon" that doesn't exist in the kernel by default.

Dockerizing applications gave us a couple of things:

 - **Portability**. A Dockerized application will now run the *exact same way* regardless of what the host operating system is, as long as it's a compatible Docker Host.

 - **Isolation**. With everything that you need encapsulated inside the image/container, this means that you no longer have to worry about dependencies being missing on your destination host.

![Concerns](/images/3-concerns.png)

### Docker basics

The Docker ecosystem has three primary parts to it - the **Client**, the **Host**, and the **Registry**.

![Ecosystem](/images/4-docker.png)

The **Client** is the primary way of interacting with the Docker host. In very much the same way that client applications talk to an API, the Docker command line interacts with the Docker server.

The **Docker Host** is the service that runs the API and the daemon that manages your images and containers. Note that we have this annotated as `DOCKER_HOST`, because while this is usually installed in the same system as the **Client**, your Docker Host can be in another machine.

The **Registry** is where your images are stored. We'll talk about all of these in more detail later, but for now, all you need to know is they roughly wire up like this:

![Ecosystem](/images/5-docker2.png)

Don't worry about absorbing this all - we'll slowly unpack how everything wires together.


### Containers and Images

A Docker image is a file, composed of multiple layers, used to execute code. When this code is in execution and is able to read/write is when we consider it a **container**. In short, an **image** is an inert **container**, and a **container** is an image that is running.

### Docker Run

Before we start with anything, let's run the following command in the command line interface (CLI):

```
docker run hello-world
```

This runs a pre-existing image called `hello-world` on your Docker host, which runs and outputs some cute text.

### Docker Pull

Running a `docker pull` means that your Docker Host downloads an image from the registry (but does not run it). So try running the following command:

```
docker pull busybox
```

This will pull the `busybox` image from the default Docker registry: https://hub.docker.com/explore/. You can also try pulling other images from the registry if you want!

Now that you have the `busybox` image, you can try running it:

```
docker run busybox
```

Notice that nothing happened. *This is on purpose* - we didn't specify a command, so all Docker did was to load up `busybox`, ran an empty command, then exited. So what does it look like when we want to run a command? Try:

```
docker run busybox echo "Hallo Avocado!"
```

This will load up the `busybox` image, then run a command which outputs `Hallo Avocado!`. Look at you, you command-line *slayer*.


### Docker Images

Oh homes, now you have two images downloaded. You can check the Docker images you have by running `images`:

```
docker images
```

This should list something similar to:

```
REPOSITORY       TAG             IMAGE ID            CREATED             SIZE
hello-world      latest          fce289e99eb9        2 weeks ago         1.84kB
busybox          latest          3a093384ac30        2 weeks ago         1.2MB
```

### Docker Run A Service

Okay. Now let's try running a website - say, an Nginx application that just serves the default page. For this, all we have to do is run:

```
docker run nginx:latest
```

Notice that we didn't have to do a `docker pull`. What essentially happens is shown in the output of the command:

```
$ docker run nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
177e7ef0df69: Pull complete 
ea57c53235df: Pull complete 
bbdb1fbd4a86: Pull complete 
Digest: sha256:b543f6d0983fbc25b9874e22f4fe257a567111da96fd1d8f1b44315f1236398c
Status: Downloaded newer image for nginx:latest
```

As per the above, Docker tries to see if the image you're trying to run exists locally - then pulls from somewhere else if it does not exist. The *somewhere else* is the Docker Hub we saw earlier - it just grabs the matching version off that.

But there's no website, right? How do we access it? It turns out that we need to *publish ports* for Docker to be able to serve out anything.

But wait, it seems to be stuck? Not really. Docker is just running on attached mode, so we also need to run Docker in a *detached mode* it looks like. Just type *Ctrl-C* to cancel the running container, and rerun it with as the following:

```
docker run -d -P --name banana-smith nginx
```

`-d` runs Docker in *detached mode*, and `-P` publishes all exposed ports so we can access it. `banana-smith` is what we're naming the container - feel free to change it to any name you want. We can then find out how to access the site by running:

```
docker port banana-smith
```

Which will give us something like:

```
80/tcp -> 0.0.0.0:32768
```

This basically means that the service is accessible from port `32768` of your machine. You should be able to open up your browser and go to:

```
http://localhost:32768
```

Neat, right?


### Docker Run Shell Within Nginx

Note that the Docker image you're running also contains other things. It has a small slice of the operating system, and this slice is actually available to you. You can access the command line inside the `nginx` container we were running earlier by running:

```
docker run -it nginx:latest /bin/bash
```

The `-it` flag attaches you to an interactive "console" within the container. `/bin/bash` is a shell that exists within the container. To see where the page that you saw before was, try running the following while inside the console:

```
cat /usr/share/nginx/html/index.html
```

And there you go! You can pull down and run containers.


### Exercise

Now, what you're going to find is that there are lots of Docker containers out in the wild. Now that you know how to pull down and run images, I've got a challenge for you: why not see if you can find and run the `wordpress` image from https://hub.docker.com/_/wordpress ?
