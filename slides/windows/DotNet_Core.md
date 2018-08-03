class: title

# .NET Core Cross-Platform In Docker

  ![strange containers](images/strange-containers.jpg)

---

## Objectives

  At the end of this section, you will be able to:

  * Run .NET Core apps on Windows and Linux.

  * Understand IDE and Editor plugin options.

---

  
## .NET Core Base Images

https://hub.docker.com/r/microsoft/dotnet/

- microsoft/dotnet or microsoft/dotnet:latest (alias for the SDK image)
- (1.7GB) microsoft/dotnet:2.1-sdk
- (255MB) microsoft/dotnet:2.1-aspnetcore-runtime
- (180MB) microsoft/dotnet:2.1-runtime

---

## Tool Support for .NET Core

Most IDE's and editors have Docker support, plug-in's, etc.

- Visual Studio 2017 [can support Docker](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/docker/visual-studio-tools-for-docker) in projects
- [Visual Studio for Mac too](https://github.com/Microsoft/vs4mac-labs/tree/master/Docker/Getting-Started) (now supported in stable release)
- Visual Studio Code has a Docker Plug-in that also supports docker-compose

--

Why does this matter, can't I just run the docker CLI?

- IDE may create Dockerfile or docker-compose.yml for you!
- IDE will often support one-click docker builds or runs
- IDE Debuggers will better support network debugging
- IDE will use network to talk to apps in containers, not sockets or cli

---

## Sample ASP.NET Core App

Let's clone, build, and run a [cross-platform .NET Core web app](https://docs.microsoft.com/en-us/dotnet/core/docker/building-net-docker-images)

```bash
git clone https://github.com/dotnet/dotnet-docker
```

--

cd into `dotnet-docker/samples/aspnetapp` (forward or backslashes depending on OS)

```bash
docker build -t aspnetapp .
docker run -it --rm -p 5000:80 --name aspnetcore_sample aspnetapp
```

open `http://localhost:5000`

---

## Sample ASP.NET Core App Cont.

On Your OS, which OS did this build and run on?

--

- macOS: Linux 64-bit
- Linux: Linux 64-bit
- Windows "Linux Containers": Linux 64-bit
- **Windows "Windows Containers": Windows 64-bit**

--

Can we force the Windows Containers to build it on Linux?

(lets [look at the Dockerfile(s)](https://github.com/dotnet/dotnet-docker/tree/master/samples/aspnetapp))




