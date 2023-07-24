---
title: "Bringup Mardan Robot"
linktitle: "Bringup"
date: 2023-07-21
weight: 2
collapsible: false
---

{{< alert title="Bringup" >}}
The code will be extracted from the [MardanRobotRpi](https://github.com/roboticamed/MardanRobotRpi) GitHub repository. 
{{< /alert >}}

### 1. Install Docker Compose  into RPi 4
``` shell
sudo apt-get install -y libffi-dev libssl-dev python3-dev python3 python3-pip

sudo pip3 install docker-compose

sudo systemctl enable docker
```

#### 2. Clone the repository
Clone the  [repository](https://github.com/roboticamed/MardanRobotRpi) on the Raspberry Pi using the following command:
```shell
sudo git clone https://github.com/roboticamed/MardanRobotRpi

```

### 3. Navigate to MardanRobotRpi
Once the repository is cloned, navigate to the **MardanRobotRpi** folder, and then enter on the **RPi** folder.
Please, follow the below commands:

``` shell
cd MardanRobotRpi/
cd RPi/
```



### 4. Run Docker Compose 

Run  Docker Compose in the directory ~/MardanRobotRpi/RPi.
```shell
docker-compose up
```
That should give an output similar to:

![](docker-compose.png)

>**Note:** Be sure that ROS master is connected

Now, the next step is to connect the MardanRobot with the Android application to put it into operation.