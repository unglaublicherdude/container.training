
class: title

# Understanding Docker images

![image](images/title-understanding-docker-images.png)

---

## Objectives

In this section, we will explain:

* What is an image.

* What is a layer.

* The various image namespaces.

* How to search and download images.

* Image tags and when to use them.

---

## What is an image?

* Image = files + metadata

* These files form the root filesystem of our container.

* The metadata can indicate a number of things, e.g.:

  * the author of the image
  * the command to execute in the container when starting it
  * environment variables to be set
  * etc.

* Images are made of *layers*, conceptually stacked on top of each other.

* Each layer can add, change, and remove files and/or metadata.

* Images can share layers to optimize disk usage, transfer times, and memory use.

---

## Example for a Java webapp

* CentOS base layer
* JRE
* Tomcat
* Our application's dependencies
* Our application code and assets
* Our application configuration

---

## Example in layers

<table>
  <tr>
    <td>CentOS</td>
    <td>-----></td>
    <td>Base "FROM" Layer</td>
  </tr>
  <tr>
    <td>JRE</td>
    <td>-----></td>
    <td>Layer 2</td>
  </tr>
  <tr>
    <td>Tomcat</td>
    <td>-----></td>
    <td>Layer 3</td>
  </tr>
  <tr>
    <td>App dependencies</td>
    <td>-----></td>
    <td>Layer 4</td>
  </tr>
</table>

etc.

---

class: pic

## Example in pictures

![layers](images/container-layers.jpg)

---

class: pic

## Example of many containers using same images

![layers](images/sharing-layers.jpg)

---

## Differences between containers and images

* An image is a read-only filesystem.

* A container is an encapsulated set of processes running in a
  read-write copy of that filesystem.

* To optimize container boot time, *copy-on-write* is used 
  instead of regular copy.

* `docker run` starts a container from a given image.

---

## Differences between containers and images

* An image is a read-only filesystem.

* A container is an encapsulated set of processes running in a
  read-write copy of that filesystem.

* To optimize container boot time, *copy-on-write* is used 
  instead of regular copy.

* `docker run` starts a container from a given image.

---

## How do we get images?

* We download (aka pull) them from a image "registry".

* We make them ourselves.

---

## Wait a minute...

If an image is read-only, how do we change it?

* We don't change existing images, we create new ones.

---

## Creating images

`docker build` (**used 99% of time**)

* Performs a repeatable build sequence.
* Uses a `Dockerfile` for build instructions.
* This is the preferred method!

`docker commit`

* Saves all the changes made to a container into a new layer.
* Creates a new image (effectively a copy of the container).

---

## Images namespaces

There are three namespaces:

* Official images

    e.g. `ubuntu`, `busybox` ...

* User (and organizations) images

    e.g. `jpetazzo/clock`

* Self-hosted images

    e.g. `registry.example.com:5000/my-private/image`

Let's explain each of them.

---

## Root namespace

The root namespace is for official images. They are put there by Docker Inc.,
but they are generally authored and maintained by trusted third parties.

Those images include:

* Small, "swiss-army-knife" images like `busybox`.

* Distro images to be used as bases for your builds, like `ubuntu`, `fedora`...

* Ready-to-use components and services, like `redis`, `postgresql`...

* Over 130 at this point!

---

## User namespace

The user namespace holds images for Docker Hub users and organizations.

For example: `jpetazzo/clock`

  * The Docker Hub user is: `jpetazzo`

  * The image name is: `clock`

---

## Self-Hosted namespace

This namespace holds images which are not hosted on Docker Hub, but on third
party registries.

They contain the hostname (or IP address), and optionally the port, of the
registry server.

For example:

```bash
localhost:5000/wordpress
```

* `localhost:5000` is the host and port of the registry
* `wordpress` is the name of the image

---

## How do you store and manage images?

Images can be stored:

* On your Docker host.
* In a Docker registry.
* Exported (save) to external tar archive (rare)

You can use the Docker client to download (pull) or upload (push) images.

To be more accurate: you can use the Docker client to tell a Docker Engine
to push and pull images to and from a registry.

---

## Showing current images

Let's look at what images are on our host now.

```bash
$ docker images
REPOSITORY       TAG       IMAGE ID       CREATED         SIZE
fedora           latest    ddd5c9c1d0f2   3 days ago      204.7 MB
centos           latest    d0e7f81ca65c   3 days ago      196.6 MB
ubuntu           latest    07c86167cdc4   4 days ago      188 MB
redis            latest    4f5f397d4b7c   5 days ago      177.6 MB
postgres         latest    afe2b5e1859b   5 days ago      264.5 MB
alpine           latest    70c557e50ed6   5 days ago      4.798 MB
debian           latest    f50f9524513f   6 days ago      125.1 MB
busybox          latest    3240943c9ea3   2 weeks ago     1.114 MB
training/namer   latest    902673acc741   9 months ago    289.3 MB
jpetazzo/clock   latest    12068b93616f   12 months ago   2.433 MB
```

---

## Downloading images

There are two ways to download images.

* Explicitly, with `docker pull`.

* Implicitly, when executing `docker run` and the image is not found locally.

---

## Pulling an image

```bash
$ docker pull alpine:3.7
3.7: Pulling from library/alpine
ff3a5c916c92: Pull complete
Digest: sha256:7df6db5aa61ae9480f52f0b3a06a140ab98d427f86d8d5de0bedab9b8df6b1c0
Status: Downloaded newer image for alpine:3.7
```

* As seen previously, images are made up of layers.

* Docker has downloaded all the necessary layers.

* In this example, `:3.7` indicates which exact version of Debian
  we would like.

  It is a *version tag*.

---

## Image and tags

* Images can have tags.

* Tags define image versions or variants.

* `docker pull ubuntu` will refer to `ubuntu:latest`.

* The `:latest` tag is generally updated often.

---

## When to (not) use tags

Don't specify tags:

* When doing rapid testing and prototyping.
* When experimenting.
* When you want the latest version.

Do specify tags:

* When recording a procedure into a script.
* When going to production.
* To ensure that the same version will be used everywhere.
* To ensure repeatability later.
* **ProTip: Never use `:latest` for anything beyond local machine**

---

## Section summary

We've learned how to:

* Understand images and layers.
* Understand Docker image namespacing.
* Search and download images.

