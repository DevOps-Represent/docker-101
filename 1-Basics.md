## The Basics

Note: Before proceeding, make sure that you've gone through the prerequisites! Quite important, yanno?

### Basic Concepts

Docker is in use now by a lot of companies when deplooying applications to the cloud. However, to explain why Docker is important, we may need to go back a couple of years to revisit how we used to deploy applications.

In the past, we used to deploy applications against operating systems installed on hardware that we bought. Much like how you'd install software on your Windows operating system, which is installed on your laptop.

Over time, we started having [Hypervisors](https://en.wikipedia.org/wiki/Hypervisor) - software that would allow you to emulate hardware, and install *another* operating system on top of it. Conceptually, it all stacks up like this:

![Virtualization](/images/1-vms.jpg)

You would install the Hypervisor, and then "*fake*" hardware that you can install an operating system in. While this meant that anything on top of the Hypervisor is now portable, it was subject to *add more stuff here*

During the last couple of years, people have started using Docker. Compared to Virtual Machines, it all stacks up like this:

![Containerization](/images/2-containers.jpg)

The operating system ( or to be more correct, the [kernel](https://en.wikipedia.org/wiki/Kernel_(operating_system)) ) began to be reused, and all you had to do was pack up *just what you need* - the libraries, or anything "uncommon" that doesn't exist in the kernel by default.

Dockerizing applications gave us a couple of things:

 - **Portability**. A Dockerized application will now run the *exact same way* regardless of what the host operating system is, as long as it's a compatible Docker Host.

 - **Isolation**. With everything that you need encapsulated inside the image/container, this means that you no longer have to worry about dependencies being missing on your destination host.

![Concerns](/images/concerns.jpg)

### Docker basics

The Docker ecosystem has three primary parts to it - the **Client**, the **Host**, and the **Registry**.


