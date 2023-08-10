---
title: "Jupyter lab"
linktitle: "Jupyter lab"
date: 2023-08-02
weight: 2
collapsible: false
---

The following instructions configure [Jupyterlab](https://jupyter.org/) as a service inside of the Raspberry Pi to offer an easier alternative to access the card than SSH.


## Installation

Connec to the raspberry via SSH using the configured `<username>` and `<hosname>` in the [setup guide](../rpi4_setup/):

```bash
ssh <username>@<hostname>
```

Install python3 package manager:

```bash
sudo apt install python3-pip
```

Install jupyterlab

```bash
sudo python3 -m pip install jupyterlab
```

## Configuration

Next, Juupyterlab needs to be configured so that it can be accessed from outiside the Raspberry Pi localhost network. For this, create a blank file in home directory as:

```bash
touch ~/.jupyter/jupyter_lab_config.py
```

Edit the file, using `nano` for instance, and insert the following text:

```python
# Configuration file for lab.
 
c = get_config()  #noqa

c.NotebookApp.allow_origin = '*'
c.NotebookApp.ip = '0.0.0.0'
c.NotebookApp.open_browser = False

# This sha1 hash is for password: pi
c.ServerApp.password = 'argon2:$argon2id$v=19$m=10240,t=10,p=8$Fj7xenAYBXANnbsHhLT2uw$mLeEHcj9AvNHRPa1oFRyI6jBsqFc2GzvPm8v00a31Ho'
```

Save the file and exit the editor.

Finally, test the configuration by running Jupyter from the current SSH session:

```bash
jupyter lab
```

Open a web browser and access Jupyter lab by using `http://<hostname>:8888`. When asked for the password, type `pi`.

![](launcher.png)


## Service configuration

To make jupyterlab server available automatically from boot up, create a systemctl service to configure its launch.

```
# create the service file
sudo touch /etc/systemd/system/jupyter.service
sudo chmod 644 /etc/systemd/system/jupyter.service
```

Open an editor with `sudo nano /etc/systemd/system/jupyter.service` and copy/paste the following text:

```
[Unit]
Description=Jupyter Notebook

[Service]
Type=simple
ExecStart=/usr/local/bin/jupyter lab
User=<username>
Group=<username>
WorkingDirectory=/home/<username>
Restart=always
RestartSec=10
#KillMode=mixed

[Install]
WantedBy=multi-user.target
```

Replace `<username>` with the username configured in the [setup guide](../rpi4_setup/).

Enable the service upon system boot:

```bash
sudo systemctl enable jupyter.service 
```

Reboot the system. Next time the Raspberry Pi starts, jupyter lab should start automatically and be available in the browser:

```
sudo reboot
```

{{% alert title="Additional commands" color="primary" %}}

Systemctl services, such as the Jupyterlab just configured can be manually controlled with commands such as:

```bash
sudo systemctl start jupyter.service 
sudo systemctl stop jupyter.service 
sudo systemctl status jupyter.service 
```
{{% /alert %}}
