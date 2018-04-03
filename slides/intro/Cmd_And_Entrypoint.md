
class: title

# Adding `CMD`

![Container entry doors](images/entrypoint.jpg)

---

## Objectives

In this lesson, we will learn about the next important Dockerfile command:

`CMD`

This allows us to set the default command
to run in a container.


---

## Defining a default command

When people run our container, we want to greet them with a nice hello message, and using a custom font.

For that, we will execute:

```bash
figlet -f script hello
```

* `-f script` tells figlet to use a fancy font.

* `hello` is the message that we want it to display.

---

## Adding `CMD` to our Dockerfile

Our new Dockerfile will look like this:

```dockerfile
FROM ubuntu
RUN apt-get update && apt-get -y install figlet
CMD figlet -f script hello
```

* `CMD` defines a default command to run when none is given.

* It can appear at any point in the file.

* Each `CMD` will replace and override the previous one.

* As a result, while you can have multiple `CMD` lines, it is useless.

---

## Build and test our image

Let's build it:

```bash
$ docker build -t figlet .
...
Successfully built 042dff3b4a8d
```

And run it:

```bash
$ docker run figlet
 _          _   _       
| |        | | | |      
| |     _  | | | |  __  
|/ \   |/  |/  |/  /  \_
|   |_/|__/|__/|__/\__/ 
```

---

## Overriding `CMD`

If we want to get a shell into our container (instead of running
`figlet`), we just have to specify a different program to run:

```bash
$ docker run -it figlet bash
root@7ac86a641116:/# 
```

* We specified `bash`.

* It replaced the value of `CMD`.



