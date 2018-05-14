## Workshop setup: install docker, get data

* Windows 10 Pro/Enterprise: [Docker for Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows)
* Windows 7/8/10 Home: [Docker Toolbox](https://docs.docker.com/toolbox/toolbox_install_windows)
* macOS: [Docker for Mac](https://store.docker.com/editions/community/docker-ce-desktop-mac) (don't use brew)
* Linux: [Docker CE for your Linux distribution](https://store.docker.com/search?offering=community&operating_system=linux&q=&type=edition) (don't use default pkg)

.small[
(more install info in the [Docker install chapter](#toc-installing-docker))
]

Get this workshop repo: 

`git clone --depth 1 https://github.com/bretfisher/container.training.git`

Download images you'll need (totaling 4.3GB):

.small[
  ```bash
  docker pull ubuntu
  docker pull mysql
  docker pull redis
  docker pull jpetazzo/trainingwheels
  docker pull python
  docker pull ruby:2.1
  ```
]

---

## Hello

 - I'm Bret [@bretfisher](https://twitter.com/bretfisher), I like .emoji[☕🥂🏖️🥃🏋️‍♂️🐳]
   - I do things at https://bretfisher.com
   - Docker Captain, Udemy Course Author, DevOps Consultant, Meetup Organizer
   - I spend 100% helping people with Docker and container tools
   - Plugging a good cause: Improve your community with code [codeforamerica.org](https://www.codeforamerica.org)

- The workshop will run from 2:30pm to 5:15pm, with a break at 4

- Feel free to interrupt for questions at any time

- Live feedback, questions, help on @@CHAT@@
