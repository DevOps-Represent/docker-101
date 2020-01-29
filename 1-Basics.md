## Docker Basics

In this section, we'll be talking about what it means to do a `docker run` and a `docker pull`:

![Docker run-pull](/images/9-pull-run.png)


## Running Docker

Before we start with anything, let's run the following command in the command line interface (CLI):

```
docker run hello-world
```

This runs a pre-existing image called `hello-world` on your Docker host, which runs and outputs some cute text.

#### Pulling images

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
docker run busybox echo "Hallo Avocado"
```

This will load up the `busybox` image, then run a command which outputs `Hallo Avocado!`. Look at you, you command-line *slayer*.


#### Seeing your Docker Images

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

#### Let's run something in Docker 

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
docker run -d -P --name banana-smith-container nginx
```

`-d` runs Docker in *detached mode*, and `-P` publishes all exposed ports so we can access it. `banana-smith` is what we're naming the container - feel free to change it to any name you want. We can then find out how to access the site by running:

```
docker port banana-smith-container
```

Which will give us something like:

```
80/tcp -> 0.0.0.0:32768
```

This basically means that the service is accessible from port `32768` of your machine. You should be able to open up your browser and go to:

```
http://localhost:32768
```

Neat, right? Finally, stop the container by running:

```
docker stop banana-smith-container
```


#### Docker Run Shell Within Nginx

Note that the Docker image you're running also contains other things. It has a small slice of the operating system, and this slice is actually available to you. You can access the command line inside the `nginx` container we were running earlier by running:

```
docker run -it nginx:latest /bin/bash
```

The `-it` flag attaches you to an interactive "console" within the container. `/bin/bash` is a shell that exists within the container. To see where the page that you saw before was, try running the following while inside the console:

```
cat /usr/share/nginx/html/index.html
```

And there you go! You can pull down and run containers.


#### Exercise

Now, what you're going to find is that there are lots of Docker containers out in the wild. Now that you know how to pull down and run images, I've got a challenge for you: why not see if you can find and run the `wordpress` image from https://hub.docker.com/_/wordpress ?
