class: title

# Checking Our Setup

---

## Your Terminal for Docker

Terminal GUI: Window management program that runs various shells

  - e.g. Terminal, Command Prompt, iTerm2

Shell: Command line interface with various features

  - e.g. Bash, PowerShell.exe, Zsh, cmd.exe

Linux: Whatever Terminal GUI your Distro has, with Bash shell

Windows 10 Pro/Ent: PowerShell GUI/shell. ( optional)

Windows 7/8 or 10 Home: Docker Quickstart Terminal with Bash shell

MacOS: Terminal with Bash shell

.footnote[

Lots of other advanced GUI's like cmder, iTerm2 and new cross-OS options like Hyper, Terminus, or Upterm 

]

---

## Docker Version Check

Let's check that docker is working.

.exercise[
  ```
  docker version
  ```
]

This is a great command to run on a new setup. It does several things:

  1. Verifies the docker client can talk to the docker server
  2. Shows you versions, which don't have to match but ideally client >= server
  3. Shows you edition: CE, EE, test, or dev branches. You should have CE

---

## Docker Info

You can get a lot of useful config info with the info command:

.exercise[
  ```
  docker info
  ```
]

Most of this won't matter for now, but will be useful throughout your journey

