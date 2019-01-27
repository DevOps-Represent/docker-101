As neat as it is to quickly run preexisting images, we probably want to explore how we can create our own images. In this section, we'll be taking a look at `Dockerfile`.

A `Dockerfile` is the traditional naming of a file which contains instructions on how to build an image.

## Docker Images

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

## Dockerfiles

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

Then open up your browser, and access the same page.

## Docker build


## Dockerfile instructions breakdown


## Creating a new docker image


## Exercise
