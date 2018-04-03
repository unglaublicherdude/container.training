
class: title

# Naming and inspecting containers

![Markings on container door](images/title-naming-and-inspecting-containers.jpg)

---

## Objectives

In this lesson, we will learn about an important
Docker concept: container *naming*.

Naming allows us to:

* Reference easily a container.

* Ensure unicity of a specific container.

We will also see the `inspect` command, which gives a lot of details about a container.

---

## Naming our containers

So far, we have referenced containers with their ID.

We have copy-pasted the ID, or used a shortened prefix.

But each container can also be referenced by its name.

If a container is named `thumbnail-worker`, I can do:

```bash
$ docker logs thumbnail-worker
$ docker stop thumbnail-worker
etc.
```

---

## Default names

When we create a container, if we don't give a specific
name, Docker will pick one for us.

It will be the concatenation of:

* A mood (furious, goofy, suspicious, boring...)

* The name of a famous inventor (tesla, darwin, wozniak...)

Examples: `happy_curie`, `clever_hopper`, `jovial_lovelace` ...

---

## Specifying a name

You can set the name of the container when you create it.

```bash
$ docker run --name ticktock jpetazzo/clock
```

If you specify a name that already exists, Docker will refuse
to create the container.

This lets us enforce unicity of a given resource.

---

## Inspecting a container

The `docker inspect` command will output a very detailed JSON map.

```bash
$ docker inspect <containerID>
[{
...
(many pages of JSON here)
...
```

There are multiple ways to consume that information.

