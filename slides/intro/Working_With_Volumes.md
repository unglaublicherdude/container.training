
class: title

# Working with volumes

![volume](images/title-working-with-volumes.jpg)

---

## Objectives

At the end of this section, you will be able to:

* Create containers holding volumes.

* Share a host directory with one or many containers.

---

## Working with volumes

Docker volumes can be used to achieve many things, including:

* Bypassing the copy-on-write system to obtain native disk I/O performance.

* Bypassing copy-on-write to leave some files out of `docker commit`.

* Sharing a directory between multiple containers.

* Sharing a directory between the host and a container.

* Sharing a *single file* between the host and a container.

---

## Volumes are special directories in a container

Volumes can be declared in two different ways.

* Within a `Dockerfile`, with a `VOLUME` instruction.

```dockerfile
VOLUME /uploads
```

* On the command-line, with the `-v` flag for `docker run`.

```bash
$ docker run -d -v /uploads myapp
```

In both cases, `/uploads` (inside the container) will be a volume.

---

## Volumes exist independently of containers

If a container is stopped, its volumes still exist and are available.

Volumes can be listed and manipulated with `docker volume` subcommands:

```bash
$ docker volume ls
DRIVER              VOLUME NAME
local               5b0b65e4316da67c2d471086640e6005ca2264f3...
local               pgdata-prod
local               pgdata-dev
local               13b59c9936d78d109d094693446e174e5480d973...
```

Some of those volume names were explicit (pgdata-prod, pgdata-dev).

The others (the hex IDs) were generated automatically by Docker.

---

## Naming volumes

* Volumes can be created without a container, then used in multiple containers.

Let's create a couple of volumes directly.

```bash
$ docker volume create webapps
webapps
```

```bash
$ docker volume create logs
logs
```

Volumes are not anchored to a specific path.

---

## Using our named volumes

* Volumes are used with the `-v` option.

* When a host path does not contain a /, it is considered to be a volume name.

Let's start a web server using the two previous volumes.

```bash
$ docker run -d -p 1234:8080 \
         -v logs:/usr/local/tomcat/logs \
         -v webapps:/usr/local/tomcat/webapps \
         tomcat
```

Check that it's running correctly:

```bash
$ curl localhost:1234
... (Tomcat tells us how happy it is to be up and running) ...
```

---

## Managing volumes explicitly with "bind-mounts"

In some cases, you want a specific directory on the host to be mapped
inside the container:

* You want to manage storage and snapshots yourself.

    (With LVM, or a SAN, or ZFS, or anything else!)

* You have a separate disk with better performance (SSD) or resiliency (EBS)
  than the system disk, and you want to put important data on that disk.

* You want to share your source directory between your host (where the
  source gets edited) and the container (where it is compiled or executed).

Wait, we already met the last use-case in our example development workflow!
Nice.

```bash
$ docker run -d -v /path/on/the/host:/path/in/container image ...
```

---

## Volume plugins

You can install plugins to manage volumes backed by particular storage systems,
or providing extra features. Find many at https://store.docker.com under plugins. For instance:

* [REX-Ray](https://github.com/emccode/rexray) - create and manage volumes backed by an shared storage (e.g. SAN or NAS), or by cloud block stores (e.g. EBS, EFS, S3);
* [Portworx](http://portworx.com/) - provide distributed block store for containers;
* and much more!

---

## Volumes vs. Mounts

* Since Docker 17.06, a new options is available: `--mount`.

* It offers a new, richer syntax to manipulate data in containers.

* It makes an explicit difference between:

  - volumes (identified with a unique name, managed by a storage plugin),

  - bind mounts (identified with a host path, not managed).

* The former `-v` / `--volume` option is still usable.

---

## `--mount` syntax

Binding a host path to a container path:

```bash
$ docker run \
  --mount type=bind,source=/path/on/host,target=/path/in/container alpine
```

Mounting a volume to a container path:

```bash
$ docker run \
  --mount source=myvolume,target=/path/in/container alpine
```

Mounting a tmpfs (in-memory, for temporary files):

```bash
$ docker run \
  --mount type=tmpfs,destination=/path/in/container,tmpfs-size=1000000 alpine
```

---

## Section summary

We've learned how to:

* Create and manage volumes.

* Share volumes across containers.

* Share a host directory with one or many containers.
