
class: title

# The Container Network Model

![A denser graph network](images/title-the-container-network-model.jpg)

---

## Objectives

We will learn about the CNM (Container Network Model).

At the end of this lesson, you will be able to:

* Create a private network for a group of containers.

* Use container naming to connect services together.

* Dynamically connect and disconnect containers to networks.

We will also explain the principle of overlay networks and network plugins.

---

## The Container Network Model

The CNM was introduced in Engine 1.9.0 (November 2015).

The CNM adds the notion of a *network*, and a new top-level command to manipulate and see those networks: `docker network`.

```bash
$ docker network ls
NETWORK ID          NAME                DRIVER
6bde79dfcf70        bridge              bridge
8d9c78725538        none                null
eb0eeab782f4        host                host
4c1ff84d6d3f        blog-dev            overlay
228a4355d548        blog-prod           overlay
```

---

## What's in a network?

* Conceptually, a network is a virtual switch.

* It can be local (to a single Engine) or global (spanning multiple hosts) in Swarm clustering.

* A network has an IP subnet associated to it.

* Docker will allocate IP addresses to the containers connected to a network.

* Containers can be connected to multiple networks.

* Containers can be given per-network names and aliases.

* The names and aliases can be resolved via an embedded DNS server.

---

## Network implementation details

* A network is managed by a *driver*.

* The built-in drivers include:

  * `bridge` (default)

  * `none`

  * `host`

  * `macvlan`

* A new multi-host driver, *overlay*, is available out of the box (Swarm clusters).

* More drivers can be provided by plugins (OVS, VLAN...)

* A network can have a custom IPAM (IP allocator).

---

class: pic

## Single container in a Docker network

![bridge0](images/bridge1.png)

---

class: pic

## Two containers on two Docker networks

![bridge3](images/bridge2.png)

---

## Creating a network

Let's create a network called `dev`.

```bash
$ docker network create dev
4c1ff84d6d3f1733d3e233ee039cac276f425a9d5228a4355d54878293a889ba
```

The network is now visible with the `network ls` command:

```bash
$ docker network ls
NETWORK ID          NAME                DRIVER
6bde79dfcf70        bridge              bridge
8d9c78725538        none                null
eb0eeab782f4        host                host
4c1ff84d6d3f        dev                 bridge
```

---

## Placing containers on a network

We will create a *named* container on this network.

It will be reachable with its name, `web`.

```bash
$ docker run -d --name web --net dev nginx
8abb80e229ce8926c7223beb69699f5f34d6f1d438bfc5682db893e798046863
```

---

## Communication between containers

Now, create another container on this network.

.small[
```bash
$ docker run -ti --net dev alpine sh
root@0ecccdfa45ef:/#
```
]

From this new container, we can resolve and ping the other one, using its assigned name:

.small[
```bash
/ # ping web
PING web (172.18.0.2) 56(84) bytes of data.
64 bytes from es.dev (172.18.0.2): icmp_seq=1 ttl=64 time=0.221 ms
64 bytes from es.dev (172.18.0.2): icmp_seq=2 ttl=64 time=0.114 ms
64 bytes from es.dev (172.18.0.2): icmp_seq=3 ttl=64 time=0.114 ms
^C
--- web ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2000ms
rtt min/avg/max/mdev = 0.114/0.149/0.221/0.052 ms
root@0ecccdfa45ef:/#
```
]

---

## Good to know ...

* Docker will not create network names and aliases on the default `bridge` network.

* Therefore, if you want to use those features, you have to create a custom network first.

* Enabling *Swarm Mode* gives access to clustering and load balancing with IPVS.

* Creation of networks and DNS names is generally automated with tools like Compose.

---

class: extra-details

## A few words about round robin DNS

Don't rely exclusively on round robin DNS to achieve load balancing.

Many factors can affect DNS resolution, and you might see:

- all traffic going to a single instance;
- traffic being split (unevenly) between some instances;
- different behavior depending on your application language;
- different behavior depending on your base distro;
- different behavior depending on other factors (sic).

It's OK to use DNS to discover available endpoints, but remember that you have to re-resolve every now and then to discover new endpoints.

---

class: extra-details

## Custom networks

When creating a network, extra options can be provided.

* `--internal` disables outbound traffic (the network won't have a default gateway).

* `--gateway` indicates which address to use for the gateway (when outbound traffic is allowed).

* `--subnet` (in CIDR notation) indicates the subnet to use.

* `--ip-range` (in CIDR notation) indicates the subnet to allocate from.

* `--aux-address` allows to specify a list of reserved addresses (which won't be allocated to containers).

---

class: extra-details

## Setting containers' IP address

* It is possible to set a container's address with `--ip`.
* The IP address has to be within the subnet used for the container.

A full example would look like this.

```bash
$ docker network create --subnet 10.66.0.0/16 pubnet
42fb16ec412383db6289a3e39c3c0224f395d7f85bcb1859b279e7a564d4e135
$ docker run --net pubnet --ip 10.66.66.66 -d nginx
b2887adeb5578a01fd9c55c435cad56bbbe802350711d2743691f95743680b09
```

*Note: don't hard code container IP addresses in your code!*

*I repeat: don't hard code container IP addresses in your code!*

---

class: extra-details

## Multi-host networking (overlay)

Out of the scope for this intro-level workshop!

Very short instructions:

- enable Swarm Mode (`docker swarm init` then `docker swarm join` on other nodes)
- `docker network create mynet --driver overlay`
- `docker service create --network mynet myimage`

See http://jpetazzo.github.io/container.training for all the deets about clustering!

---

class: extra-details

## Multi-host networking (plugins)

Out of the scope for this intro-level workshop!

General idea:

- install the plugin (they often ship within containers)

- run the plugin (if it's in a container, it will often require extra parameters; don't just `docker run` it blindly!)

- some plugins require configuration or activation (creating a special file that tells Docker "use the plugin whose control socket is at the following location")

- you can then `docker network create --driver pluginname`


