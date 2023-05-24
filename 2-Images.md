## Making your own Docker images

As neat as it is to quickly run preexisting images, we probably want to explore how we can create our own images. In this section, we'll be taking a look at `Dockerfile`.

A `Dockerfile` is the traditional naming of a file which contains instructions on how to build an image.

![Dockerfile](/images/8-docker-build.png)

#### Docker Images

Before we start making Docker images, it's best if we familiarize ourselves with how images are stored in your Docker host (effectively, your laptop). Run the following command to get a list:

```
docker images
```

This will produce a list of images on your laptop that look like this:

```
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
hello-world                latest              fce289e99eb9        3 weeks ago         1.84kB
busybox                    latest              3a093384ac30        3 weeks ago         1.2MB
nginx                      latest              7042885a156a        4 weeks ago         109MB
```

This effectively lists the images you have, the tags they are associated with, and the image ID. Note that you can also use image IDs as a reference when running Docker commands. Like so:

```
docker run -it 7042885a156a /bin/bash
```

#### Dockerfiles

Okay! Now let's make us some images. In the `docker-101` directory, you'll find a file called `Dockerfile`. Inside this file, you'll see:

```
FROM nginx:mainline-alpine
```

This basically means that we're starting from the `nginx:mainline-alpine` image at the starting layer. This layer contains similar things to the `nginx` container you were running earlier.

To build the image, we can then run the following from inside the `docker-101` directory:

```
docker build -t banana-smith-image .
```

This takes our `Dockerfile` file, reads the instructions, and creates the basic image on your Docker host called `banana-smith-image`. Again, change the name if you want. You can verify that the image is built by listing the images you now have:

```
docker images
```

You should see the image you just created. Finally, run the image by running:

```
docker run -d -P --name banana-smith-container banana-smith-image
```

To see that it's actually running, check the port by:

```
docker port banana-smith-container
```
If you get an error, something like :

```
docker: Error response from daemon: Conflict. The container name "/banana-smith-container" is already in use by container "dd22b733a855cafc3a9235efbe892828ca82a9ee02cd30b9c7120ba891825414". You have to remove (or rename) that container to be able to reuse that name.
See 'docker run --help'.
```
this might be because you already had a banana-smith-container and a banana-smith-image. You can remove the container by running

```
docker rm banana-smith-container
```

⚠️ Did you get the following error? ⚠️

```
Error response from daemon: You cannot remove a running container b854db1f962007c030a3609683903644511337cde67da69d45d5702240a8cb9a. Stop the container before attempting removal or force remove
```
We need to stop the container first by running

```
docker stop banana-smith-container
```
Let's try that again:

```
docker rm banana-smith-container
```

and remove the image by running

```
docker image rm banana-smith-image
```
Then try building the image again.

Then open up your browser, and access the same page - e.g., `http://localhost:32768`. Once done, cleanup the running container by running:

```
docker stop banana-smith-container
```

#### Dockerfile instructions breakdown

Now, in the `docker-101` directory, you'll find a file called `index.html`. Unfortunately, I have no eye for design - so feel free to modify the page to make it look as good as possible.

For the next section, we're going to be modifying our Dockerfile so that 1.) We replace the configuration, and 2.) We insert our `index.html` file. We'll do this by changing the `Dockerfile` so it looks like this:

```
FROM nginx:mainline-alpine
RUN rm /etc/nginx/conf.d/*
ADD wassup.conf /etc/nginx/conf.d/
ADD index.html /usr/share/nginx/html/
```

The `RUN` directive runs the command specified (in this case, `rm /etc/nginx/conf.d/*` - which cleans out the configuration directory for Nginx). The `ADD` directive adds the files from your current directory into the image's directories. What we're doing here is we're basically:

1.) Referencing the `nginx:mainline-alpine` image to start with

2.) Removing existing configuration files

3.) Adding a new configuration file (`wassup.conf`)

4.) And adding a new index.html file.

To build the image, run the same command as before:

```
docker build -t banana-smith-image .
```

Now, if you list out your images with `docker images`, you'll see something like:

```
REPOSITORY                     TAG                 IMAGE ID
banana-smith-image             latest              98be87baf87e
hello-world                    latest              fce289e99eb9
busybox                        latest              3a093384ac30
nginx                          latest              7042885a156a
```

What you might have noticed is that the IMAGE ID is now different. Run it the way you've run any other image:

```
docker run -d -P --name banana-smith-container banana-smith-image
```

Then check the port it's running under:

```
docker port banana-smith-container
```

Then open up your browser, and access the same page - e.g., `http://localhost:32768`. What you should see is your new, fresh image!


#### Exercise

Why not try adding CSS to the index.html page?
