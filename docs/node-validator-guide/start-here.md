---
title: "Start Here"
sidebar_position: 0

---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import Admonition from '@theme/Admonition';



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



---
References
- [Vlad's Guide](https://github.com/lykhonis/lukso-node-guide#auto-start)
- [Setup an Eth2 Mainnet Validator System on Ubuntu](https://github.com/metanull-operator/eth2-ubuntu)
- [ethstaker](https://discord.gg/enuHBXGS)

---
