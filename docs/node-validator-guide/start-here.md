---
title: "Start Here"
sidebar_position: 0

---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import Admonition from '@theme/Admonition';


:::danger
This guide is in beta and not ready for use 
:::

# How to use THE guide

## User specific information

Usernames, passwords, and IP addresses will differ for each user. The table below identifies these variables, the step where they are determined, and the syntax used to represent them in commands. Examples are provided in the next section.


|Variable data     |Part/Section|Syntax        |
|------------------|------------|--------------|
|Node user name    | |`<node-user>` |
|Node IP address   | |`<node-ip>`   |
|Router IP address | |`<router-ip>` |
|SSH port          | |`<ssh-port>`  |

## Codeblocks

You will see gray code blocks throughout the guide that allow you to easily copy and paste information.
```
example code block
```
Hovering over the top right side of a code block reveals a copy icon, which allows you to copy the information without highlighting the text.

Codeblocks are used for
- terminal commands
- configuration files
- examples

Codeblock will have titles that indicate their use.

```sh title="Code block title"
code block
```
The following are examples of the types of codeblocks you will see in this guide.

### Terminal commands
**no title**: all blocks without a title are terminal commands. copy and paste them directly to the terminal prompt.

```
nano ~/.ssh/config
```

### Configuration files

**filename**: codeblock titles that contain filenames contain information to copy/paste into a file editor.

```bash title=~/.ssh/config
Host lukso
  User <node-user>
  HostName <node-ip>
  Port <ssh-port>
```

### Example information
Some codeblocks will be used for example of what a file should look like or commands that need use-specific data. They will be titled **Do not copy/paste**

This block is for a step that requires modifying a configuration file. It shows you what the file should look like after you make the modification. In this example, we are instructed to find the `wifi.powersave` setting and change the value to `2`.

```sh title="Example file - do not copy/paste"
[connection]
wifi.powersave = 2
```

This block is a command that contains information specific to the individual user of the guide. In this example, a username and IP address is needed in the command.

```sh title="User-specific command - do not copy/paste"
ssh <node-user>@<node-ip>
```

If `joe`'s IP address is `192.168.1.10` he would type the command `ssh joe@192.168.1.10`


## Guide Order
When using this guide to setup a node from start to finish, use the "next" button at the bottom of the pages to ensure you complete all steps and in the correct order.

---
References
- [Vlad's Guide](https://github.com/lykhonis/lukso-node-guide#auto-start)
- [Setup an Eth2 Mainnet Validator System on Ubuntu](https://github.com/metanull-operator/eth2-ubuntu)
- [ethstaker](https://discord.gg/enuHBXGS)

---
