---
title: "Raspberyy Pi 4 setup"
linktitle: "RPi4 setup"
date: 2023-07-08
weight: 2
collapsible: false
---


## OS Installation

{{< alert title="Initial OS setup" >}}
The instructions for basic OS configuration are extracted from the [Lluvia project](https://lluvia.ai/docs/gettingstarted/installation/raspberry_pi_4/).
{{< /alert >}}

On the desktop machine, download the Raspberry Pi Imager and install a fresh version of the operating system in a micro SD card.

```shell
sudo apt install rpi-imager
```

![](rpi-imager.png)
![](rpi-imager-os-select.png)
![](rpi-imager-sd.png)

Go to extra settings and enable SSH access, configure the WiFi, if needed. Click in **write** and wait for the process to complete.

Insert the micro SD card in the RPi and next, update and upgrade the operating system:

```shell
sudo apt update
sudo apt upgrade

sudo reboot
```

### Expand storage

```shell
sudo raspi-config
```

![](raspi-config-advanced-options.png)
![](raspi-config-expand-storage.png)

```shell
sudo reboot
```

### Increase swap memory

Some of the instructions bellow require more memory than the one available in the RPi4. For this, it is recommended to increase the SWAP memory available to the system so that compilation does not fail unexpectedly. Open the `/sbin/dphys-swapfile` and `/etc/dphys-swapfile`, and edit the line `CONF_MAXSWAP=<some_value>` to 4096. After it, reboot the RPi.

```shell
# unmount the swap
sudo dphys-swapfile swapoff

# edit the swap configuration to:
# CONF_SWAPSIZE=2048 
# CONF_MAXSWAP=4096
sudo nano /sbin/dphys-swapfile
sudo nano /etc/dphys-swapfile

# configure with new values
sudo dphys-swapfile setup

# start swap
sudo dphys-swapfile swapon
```

### Camera module

If you have a camera module available, you can enable it by following the [official guide](https://www.raspberrypi.com/documentation/accessories/camera.html)

```shell
sudo raspi-config
```

![](raspi-config-interface-options.png)
![](raspi-config-legacy-camera.png)

```shell
sudo reboot
```

## Docker

{{< alert title="Installation instruction" >}}
The instructions here provided are extracted from [Simplelearn](https://www.simplilearn.com/tutorials/docker-tutorial/raspberry-pi-docker). The official instruction avaiable at [Docker.com](https://docs.docker.com/engine/install/raspbian/) did not work at the time of writing this guide.
{{< /alert >}}

In the Raspberry terminal, run:


```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Grant priviledges to the rospi user to run docker containers
sudo usermod -aG docker raspi

# restart the Raspberry
sudo reboot
```

Once logged in, try running the following commands

```bash
docker version
```

that should give an output similar to:

```
Client: Docker Engine - Community
 Version:           24.0.4
 API version:       1.43
 Go version:        go1.20.5
 Git commit:        3713ee1
 Built:             Fri Jul  7 14:50:52 2023
 OS/Arch:           linux/arm64
 Context:           default

Server: Docker Engine - Community
 Engine:
  Version:          24.0.4
  API version:      1.43 (minimum version 1.12)
  Go version:       go1.20.5
  Git commit:       4ffc614
  Built:            Fri Jul  7 14:50:52 2023
  OS/Arch:          linux/arm64
  Experimental:     false
 containerd:
  Version:          1.6.21
  GitCommit:        3dce8eb055cbb6872793272b4f20ed16117344f8
 runc:
  Version:          1.1.7
  GitCommit:        v1.1.7-0-g860f061
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

and run a demo container:

```bash
docker run hello-world
```

which should output some text like this:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm64v8)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
