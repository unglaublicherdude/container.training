## Setup: install docker, get data, chill out

.small[
* Windows 10 Pro/Enterprise: [Docker for Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows)
* Windows 7/8/10 Home: [Docker Toolbox](https://docs.docker.com/toolbox/toolbox_install_windows)
* macOS: [Docker for Mac](https://store.docker.com/editions/community/docker-ce-desktop-mac) (don't use brew)
* Linux: [Docker CE for your Linux distribution](https://store.docker.com/search?offering=community&operating_system=linux&q=&type=edition) (don't use default pkg)

(more install info in the [Docker install chapter](#toc-installing-docker))
]

1. Docker for Windows people! Set to "Linux Containers" in system tray icon

2. Get this workshop repo (preferability somewhere in your user profile): 
.small[
    ```bash
    git clone --depth 1 https://github.com/bretfisher/container.training.git
    ```
]

3. Download images you'll need:

.small[
    ```bash
    docker pull microsoft/dotnet:2.1-sdk
    docker pull mysql
    docker pull jpetazzo/trainingwheels
    docker pull python
    docker pull ruby:2.1
    ```
]

.small[
  BONUS: Get my [Docker Mastery Video Course for Free](https://www.udemy.com/docker-mastery/?couponCode=FAITHLIFE18) (Must Signup Today, Class Attendees Only)
]

---

## Hello

 - I'm Bret [@bretfisher](https://twitter.com/bretfisher), I like .emoji[☕🥂🏖️🥃🏋️‍♂️🐳]
   - I do things at https://bretfisher.com
   - Docker Captain, Udemy Course Author, DevOps Consultant, Meetup Organizer
   - I spend 100% helping people with Docker and container tools

- The workshop will run from 9pm to 5pm, with a short break every 90min

- Feel free to interrupt for questions at any time

- Live feedback, questions, help on @@CHAT@@
