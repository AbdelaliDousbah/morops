---
layout: post
title:  "How to Install Docker on Ubuntu 21.10"
author: abdel
categories: [ ubuntu, docker, tutorial ]
image: assets/images/docker-linux.jpg

---
Docker is an application that makes it simple and easy to run application processes in a container, which are like virtual machines, only more portable, more resource-friendly, and more dependent on the host operating system.

In this tutorial, you’ll learn how to install and use it on an existing installation of Ubuntu 21.10.

>Note: Docker requires a 64-bit version of Ubuntu as well as a kernel version equal to or greater than 3.10. The default 64-bit Ubuntu 21.10 server meets these requirements.

## Installing Docker

>Note: All the commands in this tutorial should be run as a non-root user. If root access is required for the command, it will be preceded by <mark>sudo</mark>.

The Docker installation package available in the official Ubuntu 21.10 repository may not be the latest version. To get the latest and greatest version, install Docker from the official Docker repository. This section shows you how to do just that.

First, add the GPG key for the official Docker repository to the system:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Add the Docker repository to APT sources:

```bash
sudo bash -c 'echo "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker-ce.list'
```

Next, update the package database with the Docker packages from the newly added repo:

```bash
sudo apt-get update
```

Make sure you are about to install from the Docker repo instead of the default Ubuntu 21.10 repo:

```bash
apt-cache policy docker-ce
```

You should see output similar to the follow:

```bash

docker-ce:
  Installed: (none)
  Candidate: 5:20.10.12~3-0~ubuntu-impish
  Version table:
     5:20.10.12~3-0~ubuntu-impish 500
        500 https://download.docker.com/linux/ubuntu impish/stable amd64 Packages
     5:20.10.11~3-0~ubuntu-impish 500
        500 https://download.docker.com/linux/ubuntu impish/stable amd64 Packages
     5:20.10.10~3-0~ubuntu-impish 500
        500 https://download.docker.com/linux/ubuntu impish/stable amd64 Packages
```

Notice that <mark>docker-ce</mark> is not installed, but the candidate for installation is from the Docker repository for Ubuntu 21.10. The <mark>docker-ce</mark> version number might be different.

Finally, install Docker:

```bash
sudo apt-get install -y docker-ce
```

Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it’s running:

```bash
sudo systemctl status docker
```

The output should be similar to the following, showing that the service is active and running:

```console
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2022-02-28 06:54:43 UTC; 8s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 2398 (dockerd)
      Tasks: 8
     Memory: 29.9M
        CPU: 375ms
     CGroup: /system.slice/docker.service
             └─2398 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Feb 28 06:54:43 ubuntu dockerd[2398]: time="2022-02-28T06:54:43.547798752Z" level=info msg="scheme \"unix\" not registered, fallback to default scheme" module=grpc
Feb 28 06:54:43 ubuntu dockerd[2398]: time="2022-02-28T06:54:43.547839103Z" level=info msg="ccResolverWrapper: sending update to cc: {[{unix:///run/containerd/conta>
Feb 28 06:54:43 ubuntu dockerd[2398]: time="2022-02-28T06:54:43.547857684Z" level=info msg="ClientConn switching balancer to \"pick_first\"" module=grpc
Feb 28 06:54:43 ubuntu dockerd[2398]: time="2022-02-28T06:54:43.601928785Z" level=info msg="Loading containers: start."
Feb 28 06:54:43 ubuntu dockerd[2398]: time="2022-02-28T06:54:43.803682067Z" level=info msg="Default bridge (docker0) is assigned with an IP address 172.17.0.0/16. D>
Feb 28 06:54:43 ubuntu dockerd[2398]: time="2022-02-28T06:54:43.912000602Z" level=info msg="Loading containers: done."
Feb 28 06:54:43 ubuntu dockerd[2398]: time="2022-02-28T06:54:43.935127854Z" level=info msg="Docker daemon" commit=459d0df graphdriver(s)=overlay2 version=20.10.12
Feb 28 06:54:43 ubuntu dockerd[2398]: time="2022-02-28T06:54:43.935361734Z" level=info msg="Daemon has completed initialization"
Feb 28 06:54:43 ubuntu systemd[1]: Started Docker Application Container Engine.
Feb 28 06:54:43 ubuntu dockerd[2398]: time="2022-02-28T06:54:43.978811872Z" level=info msg="API listen on /run/docker.sock"
```

### Installing Docker now gives you not just the Docker service (daemon) but also the docker command line utility, or the Docker client. Have fun!

By default, running the <mark>docker</mark> command requires root privileges — that is, you have to prefix the command with <mark>sudo</mark>. It can also be run by a user in the docker group, which is automatically created during the installation of Docker. If you attempt to run the <mark>docker</mark> command without prefixing it with <mark>sudo</mark> or without being in the docker group, you'll get an output like this:

```console
docker: Cannot connect to the Docker daemon. Is the docker daemon running on this host?.
See 'docker run --help'.
```

If you want to avoid typing <mark>sudo</mark> whenever you run the <mark>docker</mark> command, add your username to the docker group:

```bash
sudo usermod -aG docker ${USER}
```

To apply the new group membership, you can log out of the server and back in, or you can type the following:

```bash
su ${USER}
```

You will be prompted to enter your user’s password to continue. Afterwards, you can confirm that your user is now added to the <mark>docker</mark> group by typing:

```bash
id -nG
```

Output:

```console
username sudo docker
```

If you need to add a user to the docker group that you're not logged in as, declare that username explicitly using:

```bash
sudo usermod -aG docker username
```

The rest of this article assumes you are running the <mark>docker</mark> command as a user in the docker user group. If you choose not to, please prepend the commands with <mark>sudo</mark>.
