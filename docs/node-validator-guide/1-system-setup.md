---
title: "Part 1 - System Setup"
sidebar_position: 1
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import Admonition from '@theme/Admonition';

:::danger ToDo
- Run
:::
# Part 1 - System Setup

This section will cover the hardware and operating system needed for running a node.

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

For a future-proof setup, you may wish to choose a system with 4 cores, a 2.8GHz processor, 1TB of SSD storage, and 32GB of RAM.

### Example Hardware
Mini PCs are designed for 24/7 use and low power consumption, making them ideal for running a LUKSO node. The Intel NUC mini PC line is a common choice within the community. Below are the specs and pricing of three different NUC models.




|           | Intel NUC 10 (Minimum)         | Intel NUC 10 |Intel NUC 10 Performance Kit|
| --------  | --------                           | -------- |-----
| Processor | i3 2-cores                         | i5 4-core| i7 6-core
| RAM       | 16GB                               | 16GB     | 32GB
| Storage   | 256GB                         | 512GB |1TB
| Price     | $489                               | $629     |$949
| Link      | [Amazon](https://a.co/d/3g1vg6G)   | [Amazon](https://a.co/d/1UdrolU)     |[Amazon](https://a.co/d/iE7niEu)

A backup system (or parts) is also something to consider. If your node is offline, you will incur slashing penalties roughly equal to the rewards you would have gained while being online. For example, if you average 1 LYX per day in rewards, you will lose 1 LYX per day if offline.




## User specific information

Usernames, passwords, and IP addresses will differ for each individual. The table below identifies these variables, the step where they are determined, and the generic syntax used to represent them in commands.


|Variable data     |Part/Section|Syntax        |
|------------------|------------|--------------|
|Node user name    |[P1 1.2](#12---install-the-server)      |`<node-user>` |
|Node password     |[P1 1.2](#12---install-the-server)      |`<password>`  |
|Node IP address   |[P1 1.3](#13---determine-node-ip-address)|`<node-ip>`   |
|Router IP address |[P1 4.1](#41---router-login)      |`<router-ip>` |
|SSH port          |P2          |`<ssh-port>`  |

## Step 1 - Install Ubuntu Server

This guide will follow the steps for Ubuntu 22.04 LTS **Server** as our node operating system, and we will interact with the node remotely from a personal computer/laptop. 

You do have the options to install the desktop version of Ubuntu and operate a node like a normal computer with a keyboard, mouse and monitor attached. However, there are a number of reason to choose a server installation for you node.

* **Security** - Most importantly, it is best security practice to run **only** the software needed to operate a node. To setup, troubleshoot, and maintain your node, you will rely on a web browser to follow guides and chat applications (like Discord) for community support. With remote access, you can use these application from your personal computer, and simply copy/paste commands into the SSH terminal.

* **Performance** - Ubuntu Desktop uses more system resources. Ubuntu Server makes more resources available to your node software.

* **Convenience** - Your node should be **wired** to your network, not connected through WiFi. Remote access removes the need to keep a keyboard and monitor connect to you node and allows you to place your node hidden away near your router. Only two connections are needed, one to your router and one to an electrical outlet.

Also, with SSH enabled, you can easily setup access to your from remote locations, allowing you to perform maintenance and troubleshooting from anywhere with an internet connection.




Using Server + remote SSH access from a personal computer instead of directly attaching a keyboard/monitor to your node has security and performance benefits and also adds convenience. 

- Nodes should not run unnecessary software, like web browsers needed to follow guides, or messaging platforms used for community support.
- The temptation to use the node for other purposes is removed
- The Desktop GUI consumes system resources
- You can place the node next to your router and avoid running wires.
- You can easily configure access to your node from remote locations by enabling a VPN server.
 

### 1.1 - Create a bootable USB stick

#### Requirments
* 4GB or larger USB flash drive
* A personal computer


#### Follow these steps to download the ISO file and image the USB flash drive:

1. Visit the [Ubuntu Server](https://ubuntu.com/download/server) download page.
2. Choose "Option 2 - Manual Installation"
3. Choose "Download Ubuntu Server 22.04 LTS"
4. Follow the guide on Ubuntu's website. Choose the appropriate guide below for your personal computer's operating system. You still start with step 3
* [Windows guide](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#3-usb-selection)
* [macOS guide](https://ubuntu.com/tutorials/create-a-usb-stick-on-macos#3-prepare-the-usb-stick).



### 1.2 - Install the server

1. Attach a monitor and keyboard to your node machine. 
2. Follow [Ubuntu's official guide](https://ubuntu.com/tutorials/install-ubuntu-server#1-overview) to install the operating system. After step 14 is complete, and the system reboots, you should see a login screen:
3. Login with the username and password created during the installation.

### 1.3 - Determine node IP address
We will need to know the node's IP address for the next step in the guide.

Type` hostname -I` on the command line to determine the IP address of your node.


## Step 2 - Connect to your node
:::note
If you installed Ubuntu Desktop, skip to step 3 and run commands in the desktop terminal application.
:::

This step will establish a basic SSH connection from your personal computer to your node. 

Choose the operating system of your personal computer below for the correct instructions.

<Tabs>
  <TabItem value="Windows" label="Windows (Putty)">

#### 2.1 - Download and install PuTTY
PuTTY is a free and popular SSH client for Windows
1. Visit the [PuTTY](https://putty.org) website
2. Download the MSI installer
3. Run the installer
4. Choose "Install anyway if prompted with a warning from Microsoft.
5. Accept the default options during installation
 
#### 2.2 - Configure PuTTY
1. Open PuTTY
2. Enter your node's IP address ([from step 1.3](#13---determine-node-ip-address)) in the Host Name box
3. Click "open"
4. Click "Accept" when prompted with the security alert.
5. Enter your node user name and password
 
You should now see the same command prompt you used while directly interacting with your node machine in the previous steps.

:::tip
In Step 5, we will copy and paste commands from this guide into the PuTTY terminal. 

To copy commands from the guide, hover over the top right corner of the gray command block and click the copy button.

Right-click anywhere inside the terminal window to paste the command.
:::
  </TabItem>
  <TabItem value="windows-ps" label="Windows PowerShell">

#### 2.1 - Open PowerShell as administrator
1. Press `Win+R` to open run
1. Type `powershell`
1. Press `Ctrl+Shift+Enter`

 

#### 2.2 - Install OpenSSH
Copy/Paste this command into PowerShell. To copy commands from the guide, hover over the top right corner of the gray command block and click the copy button. To paste into PowerShell, right-click anywhere in the PowerShell window.

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

#### 2.3 - Connect to node
1. Establish an SSH connection using a command with this syntax: `ssh <node-user>@<node-ip>`Example command: `ssh joe@192.168.1.5`
2. Type `yes` and press `Enter` when prompted with the authenticity warning.
3. Enter the node user's password

You should now see the same command prompt you used while directly interacting with your node machine in the previous steps.

  </TabItem>
  <TabItem value="macOS" label="macOS">

#### 2.1 - Open terminal
Go to Applications > Utilities, and then open Terminal

#### 2.2 - Connect to node
Establish an SSH connection using a command with this syntax: `ssh <node-user>@<node-ip>`
1. Example command: `ssh joe@192.168.1.5`
2. Type `yes` and press `Enter` when prompted with the authenticity warning.
3. Enter the node user's password

You should now see the same command prompt you used while directly interacting with your node machine in the previous steps.

Copy and paste this guide's commands from the grey block into the terminal window.

:::tip
In Step 5 we will copy and paste commands from this guide into the terminal.

To copy commands from the guide, hover over the top right corner of the gray command block and click the copy button.

To paste a copied command into a Mac terminal, choose `Edit` > `Paste` from the terminal window.
:::
  </TabItem>
</Tabs>

## Step 3 - Update System

It is vital to the security of your node that your server is kept up-to-date.

### 3.1 - Update the server
```
sudo apt-get update -y && sudo apt dist-upgrade -y
sudo apt-get autoremove
sudo apt-get autoclean
```

If a "Deamons using outdated libraries" screen appears, restart all.

### 3.2 - Enable automatic updates
```
sudo apt-get install unattended-upgrades
sudo dpkg-reconfigure -plow unattended-upgrades
```
Choose yes and press `Enter`

### 3.3 Install Dependencies
Nano is the command-line text editor used throughout this guide.
```
sudo apt install -y nano wget make git
```

## Step 4 - Reserve Node IP Address
Address reservation ensures your router always assigns the same IP address to your node. 

### 4.1 - Router Login
Determine your router's IP address.
```markdown title="copy/paste this command"
netstat -nr | awk '$1 == "0.0.0.0"{print$2}'
```
Open a web browser on your personal computer and enter your router's IP address.

A username and password prompt will appear. You will need to reset your router to its default setting if you do not know your credential. Refer to your router manual for instructions.

### 4.2 - Configure Router
Find the IP address of your node:
```markdown title="copy/paste this command"
hostname -I
```
Find the setting for reserving IP addresses. “DHCP Settings” or “DHCP Reservation” are common terms. Refer to your router’s manual for specific instructions.

Configure the router to reserve this address

Shut down the node

```markdown title="copy/paste this command"
shutdown -h now
```
## Step 5 - Configure Auto Start

It is important to set up your node to power on automatically after a power outage. The setting is usually found in the BIOS, but some systems use a jumper on the motherboard. Refer to your hardware manual for instructions.

For Intel NUC, follow these steps:
1. Power on your node machine
1. Press F2 before the server boots to enter BIOS setup
2. Go to `Power` -> `Secondary Power Settings` menu
3. Set the option for `After Power Failure` to `Power On`
4. Press F10 to save changes and exit BIOS

Test the setting by unplugging the power cord while the system is running. It should turn on and boot when you plug it back in.





---
Credits:

[Vlad's Guide](https://github.com/lykhonis/lukso-node-guide#auto-start)

[Setup an Eth2 Mainnet Validator System on Ubuntu](https://github.com/metanull-operator/eth2-ubuntu)

[ethstaker](https://discord.gg/enuHBXGS)

---
