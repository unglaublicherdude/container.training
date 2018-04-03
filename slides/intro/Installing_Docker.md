class: title

# Installing Docker

![install](images/title-installing-docker.jpg)

---

## Objectives

At the end of this lesson, you will know:

* How to install Docker.

* When to use `sudo` when running Docker commands.

---

## Installing Docker

There are many ways to install Docker.

We can arbitrarily distinguish:

* Installing Docker on an existing Linux machine (physical or VM)

* Installing Docker on macOS or Windows

* Installing Docker on a fleet of cloud VMs

---

## Installing Docker on Linux

* The recommended method is to install the packages supplied by Docker Inc. at https://store.docker.com

* The general method is:

  - add Docker Inc.'s package repositories to your system configuration

  - install the Docker Engine

* Detailed installation instructions (distro by distro) are available on:

  https://docs.docker.com/engine/installation/

* You can also install from binaries (if your distro is not supported):

  https://docs.docker.com/engine/installation/linux/docker-ce/binaries/

---

## Installing Docker on macOS and Windows

* On macOS, the recommended method is to use *Docker for Mac*:

  https://docs.docker.com/docker-for-mac/install/

* On Windows 10 Pro, Enterprise, and Eduction, you can use Docker for Windows:

  https://docs.docker.com/docker-for-windows/install/

* On older versions of Windows, you can use the Docker Toolbox:

  https://docs.docker.com/toolbox/toolbox_install_windows/

* On Windows Server 2016+, you can install the native engine or Docker for Windows

  https://docs.docker.com/install/windows/docker-ee/

---

## Running Docker on macOS and Windows

* Special Docker Editions that have Settings GUI and use Host OS prefered virtualization

* They are installed like normal applications on host

* They run a tiny special VM that is mostly transparent to your daily use 
  <br/>(e.g. http://localhost)

* They access network resources like normal applications
  <br/>(and therefore, play well with enterprise VPNs and firewalls)

* They support filesystem sharing through volumes (we'll talk about this later)

* They are a superset of the features in Docker Toolbox

* They only support running one Docker VM at a time ...

  ... so if you want to run a full cluster locally, use docker-machine (rarely needed)

---

## Running Docker on macOS and Windows, cont.

When you execute `docker version` from the terminal:

* the CLI connects to the Docker Engine over a standard socket,
* the Docker Engine is, in fact, running in a VM,
* ... but the CLI doesn't know or care about that,
* the CLI sends a request using the REST API,
* the Docker Engine in the VM processes the request,
* the CLI gets the response and displays it to you.

All communication with the Docker Engine happens over the API.

This will also allow to use remote Engines exactly as if they were local.

---

## Important PSA about security

* If you have access to the Docker control socket, you can take over the machine

  (Because you can run containers that will access the machine's resources)

* Therefore, on Linux machines, the `docker` user is equivalent to `root`

* You should restrict access to it like you would protect `root`

* By default, the Docker control socket belongs to the `docker` group

* You can add trusted users to the `docker` group

* Otherwise, you will have to prefix every `docker` command with `sudo`, e.g.:

  ```bash
  sudo docker version
  ```
