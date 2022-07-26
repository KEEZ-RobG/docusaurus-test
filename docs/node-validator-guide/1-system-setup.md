---
title: "Step 1 - System Setup"
sidebar_position: 1
---
:::danger ToDo
- Run
- Grammerly
:::
# Part 1 - System Setup

In this section we will cover the hardware and operating system needed for running a node.

Required equipment:
* USB Drive for Ubuntu Server installation
* Node Machine
* Personal computer


## Purchase Hardware
### Minimum Requirements
These are the minimum hardware requirements for a node machine. Always check [LUKSO's documentation](https://docs.lukso.tech/networks/l16-testnet/run-node#system-requirements) for updates.

* **OS:** Linux or Mac 
* **CPU:** 2 cores
* **RAM:** 16GB
* **Storage:** 100GB SSD (solid state is required)

For a future-proof setup, you may wish to choose a system with 4 cores, a 2.8GHz processor, and 1TB of SSD storage, and 32GB of RAM.

### Example Hardware
Mini PCs are designed for 24/7 use and low power consumption, making them ideal for running a LUKSO node. The Intel NUC line of mini PCs is a common choice within the community. Below are the specs and pricing of three different NUC models.




|           | Intel NUC 10 (Minimum)         | Intel NUC 10 |Intel NUC 10 Performance Kit|
| --------  | --------                           | -------- |-----
| Processor | i3 2-cores                         | i5 4-core| i7 6-core
| RAM       | 16GB                               | 16GB     | 32GB
| Storage   | 256GB                         | 512GB |1TB
| Price     | $489                               | $629     |$949
| Link      | [Amazon](https://a.co/d/3g1vg6G)   | [Amazon](https://a.co/d/1UdrolU)     |[Amazon](https://a.co/d/iE7niEu)

A backup system (or parts) is also something to consider. If your node is offline, you will incur slashing penalties roughly equal to the rewards you would have gained while being online. For example, if you average 1 LYX per day in rewards, you will lose 1 LYX per day if offline.




## Step 1 - Install Ubuntu Server

A node can run on either the Desktop or Server version of Ubuntu. In this guide, we will be using Ubuntu 22.04 LTS **Server** as our node operating system and interacting with out node remotely from a personal computer/laptop. 

Using Server + remote SSH access from a personal computer instead of directly attaching a keyboard/monitor to you node has security and performance benefits. 

- Nodes should not run unnecessary software, like web browsers and chat software.
- The Desktop GUI consumes RAM. 
- You can access your node from remote locations, like a tropical island.



### 1.1 - Create a bootable USB stick

#### Requirments
* 4GB or larger USB flash drive
* A personal computer


#### Download the ISO
1. Visit the [Ubuntu Server](https://ubuntu.com/download/server) download page.
2.  Choose "Option 2 - Manual Installation"
3. Choose "Download Ubuntu Server 22.04 LTS"
 
#### Image the USB drive
Follow the guide on Ubuntu's website for your personal computer's operating syste. There are directions for [Windows](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#3-usb-selection) and  [macOS](https://ubuntu.com/tutorials/create-a-usb-stick-on-macos#3-prepare-the-usb-stick). You will start with step 3. 

### 1.2 - Install the server

Attache a monitor and keyboard to your node machine. 

:::note
This will be the only time we will need input devices attached.
:::

Follow the official [Ubuntu guide](https://ubuntu.com/tutorials/install-ubuntu-server#1-overview) to install the operating system.
 

After step 14 is complete and the system reboots, you should see a login screen:
:::caution
inset image 1-1login.png
:::

Login with the username and password created during the installation.

### 1.2 Reserve Node IP Address

:::note
In this section you will will be using both your node machine and personal computer.
:::

Address reservation ensures your router always assigns the same IP address to your node. You will need the following information for these steps:
* The IP address of your router
* The IP address of your node machine
* The username and password of your router

#### Router Login
Find the IP address of your router:
```markdown title="enter this command on the command line"
netstat -nr | awk '$1 == "0.0.0.0"{print$2}'
```
Open a web browser on your personal computer and enter your router's IP address.

A username and password prompt will appear. If you do not know your credential, you will need to reset your router to its default setting. Refer to your routers manual for instructions.

#### Configure Router
Find the IP address of your node:
```markdown title="enter this command on the command line"
hostname -I
````

Find the setting for reserving IP addresses. The names of this setting can vary from router to router. It is often found under “DHCP Settings” or “DHCP Reservation.” Refer to your router’s manual for specific instructions.

Configure the router to reserve this address


Shut down the node

```markdown title="type this command on the command line"
shutdown -h now
```
### 1.3 Configure Auto Start

It is important to set up your node to power back automatically on after a power outage. The setting is usually found in the BIOS, but some systems use a jumper on the motherboard. Refer to your hardware manual for instructions.

For Intel NUC, follow these steps:
1. Power on you node machine
1. Press F2 before the server boots to enter BIOS setup
2. Go to `Power` -> `Secondary Power Settings` menu
3. Set the option for `After Power Failure` to `Power One`
4. Press F10 to save changes and exit BIOS

Test the setting by unplugging the power cord while the system is running. It should turn on and boot when you plug it back in.

### 1.4 Update System
Boot your node

Open the Terminal application and run the following commands. Multiple commands can be copy/pasted at the same time. To paste into the terminal window, right click on the screen and select paste.

```
sudo apt-get update -y && sudo apt dist-upgrade -y
sudo apt-get autoremove
sudo apt-get autoclean
```
Enable automatic updates
```
sudo apt-get install unattended-upgrades
sudo dpkg-reconfigure -plow unattended-upgrades
```

### 1.5 Install Dependencies
Nano is the command-line text editor used throughout this guide.
```
sudo apt install -y nano wget make git
```



---
Credits:
[Vlad's Guide](https://github.com/lykhonis/lukso-node-guide#auto-start)

[Setup an Eth2 Mainnet Validator System on Ubuntu](https://github.com/metanull-operator/eth2-ubuntu)


---
