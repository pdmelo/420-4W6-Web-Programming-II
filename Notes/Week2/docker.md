# Docker

In this course, we’re going to be using Docker to run our web server and database. Let’s find out what Docker is!

[![IMAGE ALT TEXT](http://img.youtube.com/vi/_dfLOzuIg2o/0.jpg)](https://www.youtube.com/watch?v=_dfLOzuIg2o "What is docker")

https://www.youtube.com/watch?v=J0NuOlA2xDc


> Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker’s methodologies for shipping, testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in production.

The way that Docker implements their platform is through a process called **containerization**.

## 📦 Containerization

To help contextualize what a **container** actually is, it is useful to understand what **virtual machine** is first. This is because they are two different techniques to solve the same problem: Creating an environment to develop and run code. Once this environment is setup, it can be shared with developers to ensure that everyone’s code works on the same “machine”.

A **virtual machine** runs a full-blown “guest” operating system with virtual access to host resources through a hypervisor. In general, VMs incur a lot of overhead beyond what is being consumed by your application logic. A **container** runs natively on, and shares the kernel of, the host machine. It runs a discrete process, taking no more memory than any other executable, making it more lightweight than a VM.

![VM](https://vikramsinghmtl.github.io/420-4W6-Web-Programming-II/_astro/1.3.1-Container-VM.ERQA5Nqr_ZQuhX6.webp)

**TL;DR**: VMs create an entire new instance on top of the host OS to spin up an environment whereas containers leverage the host OS to spin up an environment.

## 🧱 Key Terms 

### 🐳**Container**

A small, lightweight box that has:

- Your app
- Everything your app needs to run

    #### Example:

    A Python app container will include:

    - Python
    - Needed libraries
    - The app code

### 🖼️**Image**

A Docker image is to a container like what a recipe is to food; it is the instructions from which a specific container is created. Many containers can be created based on the same image

 You run an image → you get a container.

### 📝**Dockerfile**

A written recipe describing how to build an image.



## ✅ Advantages

Dockers enables developers to:

- Write code locally and share their work with their colleagues using Docker containers.
- Push their applications into a test environment and execute automated and manual tests.
- Find and fix bugs in the development environment and redeploy them to the test environment for testing and validation.
- Deploying the fix to the customer by pushing the updated image to the production environment.



## 👍 Simple One‑Line Definition

**Docker lets you package apps with everything they need so they run exactly the same everywhere.**



## ✨ Some more about docker

**Docker lets you package apps with everything they need so they run exactly the same everywhere.**

[Here’s a nice description](https://dev.to/npentrel/docker-jargon-from-dockerfile-to-container-942) from [dev.to user Naomi Pentrel](https://dev.to/npentrel):

> You can look at the benefits of Docker from the perspective of two groups of people: DevOps folks, and developers. If you’re a developer, the benefit of Docker is that you can set up one environment which you can use for both testing and for production. Iterating on, testing, and deploying changes should be very fast. On top of that, containers solve the problem of ‘it works for me’-bugs because everything will be exactly the same regardless where you deploy. If you do DevOps, containers allow you to focus more on container management while removing the responsibility of configuring machines.

[The above notes steal heavily from the official Docker developer docs.](https://docs.docker.com/engine/docker-overview/) For a more in depth look into how Docker works, I’d recommend checking out that link. Since this is not an OS course, the only thing you really need to know is how to use basic Docker for managing your environment.