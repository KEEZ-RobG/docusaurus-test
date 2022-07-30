

# Part 1 - Setup Node Machine

equired equipment:
* USB Drive for Ubuntu Server installation
* Node Machine
* Personal computer


##  Hardware
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

## Step 1 - Install Ubuntu Server

This guide will follow the steps for Ubuntu 22.04 LTS **Server** as our node operating system, and we will interact with the node remotely from a personal computer/laptop. 

You have the option to install the desktop version of Ubuntu and operate the node like a desktop computer with a keyboard, mouse and monitor attached. However, there are a number of reason to choose a server installation for you node.

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
 


## Step 2 - Configure Settings


### 2.1 - Open the SSH config file
```
sudo nano /etc/ssh/sshd_config
```
### 2.2 - Change SSH Port Number

The default SSH port (22) should be changed to a random number for security reasons.

1. Find `#Port 22`
2. Remove the `#`
3. Choose a number between 1024 thru 49141
4. Replace `22` with your number
5. Remain in the editor

:::note
For the rest of this guide when you see `<ssh-port>` replace it with the number you chose in this step.
:::

Close editor by pressing `ctrl` + `X`, then save.

### 2.3 - Configure firewall

By default deny all traffic:

```
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

### 2.4 - Allow SSH access

```sh title="replace <ssh-port> with you port number"
sudo ufw allow <ssh-port>/tcp
```


### 2.5 - Enable firewall
```
sudo ufw enable
```


## Step 3 - Configure Auto Start

It is important to set up your node to power on automatically after a power outage. The setting is usually found in the BIOS, but some systems use a jumper on the motherboard. Refer to your hardware manual for instructions.
### 3.1 - Shut down node machine
```
sudo shutdown -h now
```
### 3.2 - Configure BIOS
For Intel NUC, follow these steps:
1. Power on your node machine
1. Press F2 before the server boots to enter BIOS setup
2. Go to `Power` -> `Secondary Power Settings` menu
3. Set the option for `After Power Failure` to `Power On`
4. Press F10 to save changes and exit BIOS

### 3.3 - Test Settings
Test the setting by unplugging the power cord while the system is running. It should turn on and boot when you plug it back in.


## Step 4 - Reserve Node IP Address
Address reservation ensures your router always assigns the same IP address to your node.

### 4.1 - Determine node IP address
We will need to know the node's IP address for the next step in the guide.

Type` hostname -I` on the command line to determine the IP address of your node.

### 4.2 - Log in to router
1. Determine your router's IP address.
```markdown title="copy/paste this command"
netstat -nr | awk '$1 == "0.0.0.0"{print$2}'
```
2. Open a web browser on your personal computer and enter your router's IP address.

A username and password prompt will appear. You will need to reset your router to its default setting if you do not know your credential. Refer to your router manual for instructions.

### 4.3 - Configure Router
1. Find the IP address of your node:
```markdown title="copy/paste this command"
hostname -I
```
2. Find the setting for reserving IP addresses. “DHCP Settings” or “DHCP Reservation” are common terms. Refer to your router’s manual for specific instructions.

3. Configure the router to reserve this address




---
References
- [Vlad's Guide](https://github.com/lykhonis/lukso-node-guide#auto-start)