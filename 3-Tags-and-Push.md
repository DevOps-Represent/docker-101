## Tags and registries

Images are good and fine, but eventually you'll need a way to share them with people or systems so that they can be executed or deployed. Docker uses mechanisms like `docker tag` and `docker push` to make this happen.


#### The basics of tags

First off, you can see the tags that you currently have by running `docker images`. The output should look like this:

```
REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
banana-smith-image             latest              98be87baf87e        28 seconds ago      17.8MB
hello-world                    latest              fce289e99eb9        3 weeks ago         1.84kB
busybox                        latest              3a093384ac30        3 weeks ago         1.2MB
nginx                          latest              7042885a156a        4 weeks ago         109MB
```

The `latest` tag gets appended to the latest build of a particular image name. However, you can add more tags to that image ID by running:

```
docker tag 98be87baf87e banana-smith-image:tag-2
```

Where you can replace `98be87baf87e` with the IMAGE ID of your image. Now, if you run `docker images`, you should see:

```
REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
banana-smith-image             tag-2               98be87baf87e        28 seconds ago      17.8MB
banana-smith-image             latest              98be87baf87e        28 seconds ago      17.8MB
hello-world                    latest              fce289e99eb9        3 weeks ago         1.84kB
busybox                        latest              3a093384ac30        3 weeks ago         1.2MB
nginx                          latest              7042885a156a        4 weeks ago         109MB
```

Note that you can also use repositories and tags as a source image. For example:

```
docker tag banana-smith-image:latest banana-smith-image:tag-3
```

Now, when you run `docker images`, you should get:

```
REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
banana-smith-image             tag-3               98be87baf87e        28 seconds ago      17.8MB
banana-smith-image             tag-2               98be87baf87e        28 seconds ago      17.8MB
banana-smith-image             latest              98be87baf87e        28 seconds ago      17.8MB
hello-world                    latest              fce289e99eb9        3 weeks ago         1.84kB
busybox                        latest              3a093384ac30        3 weeks ago         1.2MB
nginx                          latest              7042885a156a        4 weeks ago         109MB
```

Now your image has three tags. Note that you can use tags as a way of referencing where to upload your images, but we'll tackle that later.

#### Docker repositories

Docker repositories allow you to share your images with the community. To get started, we'll create a repository by [going to Docker Hub](https://hub.docker.com/) and selecting **Create A Repository**:

![Create-Repo](/images/6-create-repo.png)

Name it however you want. In the below example, we're calling it `/my-first-repo`:

![Private Repo](/images/7-create-repo.png)

We're setting it to **Private**.

Now, remember the images we had earlier? We're going to add a tag - except the tag will be your Docker Hub username, and the repository you have created:

```
docker tag banana-smith-image:latest banana-smith/my-first-repo
```

Where `banana-smith` is your Docker Hub username. Finally, we'll login to Docker Hub with the following command:

```
docker login
```

Use your Docker Hub credentials to login. Once you're logged in, you can push your image to Docker Hub:

```
docker push banana-smith/my-first-repo
```

Now, we can check if we've uploaded our image to Docker hub. With your browser, go to your Docker Hub repository and check under ***Tags***. Do you see your image?

To verify that your image can be pulled down, you can try running:

```
docker pull banana-smith/my-first-repo
```

#### Exercise

Now that you know how to login and push images, try creating a ***Public*** repository and pushing an image to it. Ask the person next to you to pull the image down and run it.

