
class: pic

# Mura with Docker Compose

![construction](images/title-advanced-dockerfiles.jpg)

---

## Objectives

- Lets take what we learned and apply it to running Mura locally.

- We're going to use docker-compose from here on out.

- Reiterate the point that we don't have to know how to install Mura.

---

## Resources

- Images: https://hub.docker.com/r/blueriver/mura

- Repo with Sample Compose files: https://github.com/blueriver/docker-muracms

- Lets download the compose sample repo and run Mura locally.

in some place you store code: 

```bash
git clone https://github.com/blueriver/docker-muracms.git
```
---

## Inspect our Compose file

- First, lets look at the compose file we'll use.

- We'll also change the mysql version to `latest`.

- Edit `docker-muracms/examples/blueriver-muracms-mysql/docker-compose.yml`

- Change line 35 `image: mysql:5.7.19` to `image: mysql:latest`

---

## Spin up Mura with docker-compose

```bash
cd docker-muracms/examples/blueriver-muracms-mysql
docker-compose up
```

---

## Lets do some interesting things

- Use up/down, and down -v to control environment
- Change envvars in compose file
- Edit something in bind-mounts

---

## Next steps?

- Enter the Mura experts :)




