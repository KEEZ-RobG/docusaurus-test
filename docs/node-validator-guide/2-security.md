---
title: "Part 2 - Security"
sidebar_position: 2
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import Admonition from '@theme/Admonition';

:::note ToDo

- Grammerly
:::

# SSH hardening
This guide will add security to the SSH connection by
* Configuring private keys only authentication
* Disabling SSH for root
* Changing the SSH port to a random number

Choose your personal computer's operating system and SSH client

<Tabs>
  <TabItem value="Windows" label="Windows (Putty)">

### Step 1: Configure SSH 

### Step 2: Generate the OpenSSH-compatible Keys with PuTTYgen
1. Start the PuTTYgen utility, by double-clicking on its .exe file or pressing the Windows key and searching for PuTTYgen
2. For Type of key to generate, select SSH-2 RSA
3. In the Number of bits in a generated key field, specify 4096
4. Click the Generate button
5. Move your mouse pointer around in the blank area of the Key section, below the progress bar (to generate some randomness) until the progress bar is full
6. A private/ public key pair has now been generated
8. Type a passphrase in the Key passphrase field & re-type the same passphrase in the Confirm passphrase field
9. Click the Save public key button & choose whatever filename you'd like (some users create a folder in their computer named my_keys)
10. Click the Save private key button & choose whatever filename you'd like (you can save it in the same location as the public key, but it should be a location that only you can access)
11. Right-click in the text field labeled Public key for pasting into OpenSSH authorized_keys file and choose Select All
12. Right-click again in the same text field and choose Copy

### Step 3: Save The Public Key On The Server
You now need to paste the public key copied in the last step in the `authorized_keys` file on your node.

1. Log into you node `ssh <node-user>@<node-ip>`
1. Open the file
```
nano ~/.ssh/authorized_keys
```
1. Paste the SSH public key into the file
1. Save and exit by pressing `Ctrl + X`, then `y` , then `Enter`

### Step 4: Secure SSH access

#### 4.1 Open the SSH config file
```
sudo nano /etc/ssh/sshd_config
```
#### 4.2 Change SSH Port Number

The default SSH port (22) should be changed to a random number for security reasons.

1. Find `#Port 22`
2. Remove the `#`
3. Choose a number between 1024 thru 49141
4. Replace `22` with your number
5. Remain in the editor
:::Note
For the rest of this guide when you see `<ssh-port>` replace it with the number you chose in this step.
:::


#### 4.3 Disable root and password log in:

1. Change `ChallengeResponseAuthentication` to `no`
2. Change `PasswordAuthentication` to `no`
3. Change `PermitRootLogin` to `prohibit-password`
4. Change `PermitEmptyPasswords` to `no`
5. Save and exit by pressing `Ctrl + X`, then `y` , then `Enter`

#### 4.4 Disable root access
A root access should not be used. Instead, a user should be using sudo to perform privilged operations on a system.

```
sudo passwd -l root
```

#### 4.5 Configure firewall

By default deny all traffic:

```
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

Allow SSH access

Type `sudo ufw allow <ssh-port>/tcp` with `<ssh-port>` replaced


Enable firewall
```
sudo ufw enable
```
Logout

### Step 5: Create a PuTTY Profile to Save Your Server's Settings
Create a profile to save you connection information.

1. Start PuTTY by double-clicking its executable file or pressing the Windows key and searching for PuTTY
1. Navigate PuTTY's various categories, along the left-hand side of the window
1. PuTTY's initial window is the Session Category
1. In the Host Name field, enter the IP address of your node `<node-ip>`
1. Enter the port number in the Port field `<ssh-port>`
1. Select SSH under Protocol
1. Along the left-hand side of the window, select the Data sub-category, under Connection;
1. Enter your node username in the Auto-login username field `<node-user>`
1. Expand the SSH sub-category, under Connection;
1. Highlight the Auth sub-category and click the Browse button, on the right-hand side of the PuTTY window;
1. Browse to and select your previously-created private key;
1. Return to the Session Category and enter a name for this profile in the Saved Sessions field;
1. Click the Save button for the Load, Save or Delete a stored session

Now you can log in to your node and you will not be prompted for a password. However, if you had set a passphrase on your public key, you will be asked to enter the passphrase at that time (and every time you log in, in the future).



  </TabItem>
  <TabItem value="windows-ps" label="Windows PowerShell">



  </TabItem>
  <TabItem value="macOS" label="macOS">



  </TabItem>
</Tabs>
>




:::info
The next steps will be performed your **personal device**.

Choose you device's operating system from the tabs below.
:::



<Tabs>
    <TabItem value="ubuntu" label="Ubuntu">

:::danger ToDo
- Run
- Grammerly
:::
## Configure Personal Device - Ubuntu
The next steps will configure the client software on a personal device running Ubuntu. You will use this device to control your node remotely.

### Update SSH Config
The SSH command requires the username, IP address, and port number of the node machine. 

Simplify the SSH command by updating the SSH config file with your node credentials.
```
nano ~/.ssh/config 
```

Copy/Paste the following into the config file.
Replace `<node-user>`, `<node-ip>`, and `<ssh-port>`
IP address and port number have been determined in previous steps. 
`<node-user>` is your node machine's user name.

```
Host lukso
  User <node-user>
  HostName <node-ip>
  Port <ssh-port>
```

Attempt to connect to verify the configuration:

```
ssh lukso
```
Enter the password of your node machine. 

You should now see the command line of you node machine.

Type `exit` to close the SSH connection. You are not back on the command line of your personal device.

### Generate SSH Keys
SSH is more secure when using public/pricate keys instead of a password. In this step we well generate keys on your personal device and send the public key to the node machine.

On your **personal device**, create a new key pair for ssh authentication.

```
ssh-keygen -t rsa -b 4096
```

Copy the public key **keyname.pub** to a node machine. Replace **keyname.pub** with a key in home directory.

```
ssh-copy-id -i ~/.ssh/keyname.pub lukso
```

### Disable Non-Key Remote Access

On a personal computer, try to ssh again. This time it should not prompt for a password.

```
ssh lukso
```

Open the SSH configuration file.

```
sudo nano /etc/ssh/sshd_config
```

Find and modify the following options:


`ChallengeResponseAuthentication no`
`PasswordAuthentication no`
`PermitRootLogin prohibit-password`
`PermitEmptyPasswords no`

Save changes and close editor. (`ctrl` + `X`, `y` , `Enter`)

Validate SSH configuration and restart ssh service.

```
sudo sshd -t
sudo systemctl restart sshd
```

Close the ssh connection

```
exit
```

### Verify Remote Access
Reconnect to your node machine
```
ssh lukso
```
Stay connected to a remote node machine to perform next steps.

### Keep System Up to Date
    
Update your system manually:
    
```
sudo apt-get update -y
sudo apt dist-upgrade -y
sudo apt-get autoremove
sudo apt-get autoclean
```

Configure your system to automatically update:

```
sudo apt-get install unattended-upgrades
sudo dpkg-reconfigure -plow unattended-upgrades
```

### Disable Root Access
    
A root access should not be used. Instead, a user should be using the `sudo` command to perform privilged operations on a system.

```
sudo passwd -l root
```

### Block Unauthorized Access

Install Fail2ban to block IP addresses that are attempting to access our node. Fail2ban blocks addresses after a certain number of failed attempts.

```
sudo apt-get install fail2ban -y
```

Edit a config to monitor ssh logins

```
sudo nano /etc/fail2ban/jail.local
```

Replace *replace-port* to match the ssh port number.

```
[sshd]
enabled=true
port=replace-port
filter=sshd
logpath=/var/log/auth.log
maxretry=3
ignoreip=
```

Save changes and close the editor

Restart the service

```
sudo systemctl restart fail2ban
```


### Improve SSH Connection

WiFi power managment may slow down SSH connections. Modifying a config file will disable it.

```
sudo nano /etc/NetworkManager/conf.d/default-wifi-powersave-on.conf
```

Find the wifi.power setting and change to to match the following:
```
[connection]
wifi.powersave = 2
```

Save changes and close the editor.

Restart `NetworkManager` service:

```
sudo systemctl restart NetworkManager
```
   </TabItem>
    <TabItem value="windows" label="Windows (PuTTY)">

## Configure Personal Device - Windows

:::danger ToDo
- Write
- Run
- Grammerly
 

:::
The next steps will configure PuTTY (SSH client software) on a personal device running Windows. You will use this device to control your node remotely.

### Comming Soon

   </TabItem>
    <TabItem value="mac" label="Mac">

## Configure Personal Device - Mac
:::danger ToDo
- find someone with a mac
:::
The next steps will configure the client software on a personal device running Mac OS. You will use this device to control your node remotely.

### Comming Soon

   </TabItem>

</Tabs>

Credits
[Vlad's Guide](https://github.com/lykhonis/lukso-node-guide#auto-start)
[Setup an Eth2 Mainnet Validator System on Ubuntu](https://github.com/metanull-operator/eth2-ubuntu)

https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement

https://www.how2shout.com/how-to/how-to-login-into-ubuntu-using-ssh-from-windows-10-8-7.html


https://support.hostway.com/hc/en-us/articles/115001509884-How-To-Use-SSH-Keys-on-Windows-Clients-with-PuTTY-