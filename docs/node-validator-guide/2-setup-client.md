import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import Admonition from '@theme/Admonition';



# Part 2 - Configure SSH
In this guide we will configure and secure the SSH connection between you personal computer and node machine.
:::note
If you installed Ubuntu Desktop and do not plan to control your node from a personal computer, skip to step 3.
:::

### Choose your personal computer's operating system

<Tabs>
<TabItem value="windows-terminal" label="Windows Terminal">

The next steps will configure Windows Terminal (or PowerShell) to connect to you node device.

#### Step 1 - Open Terminal

1. Search windows for the Terminal application
2. Right click
3. Run as administrator




#### Step 2 - Install OpenSSH
Copy/Paste this command into Terminal. To copy commands from the guide, hover over the top right corner of the gray command block and click the copy button. To paste into Terminal, right-click anywhere in the Terminal window.

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```



#### Step 3 - Connect to node
1. Copy/paste command. Before pressing enter, replace `<node-user>`, `<node-ip>`, and `<ssh-port>` with your information
```
ssh <node-user>@<node-ip> -p <ssh-port>
```

2. Execute the above command. 
3. When Promted with the authenticity warning, type `yes` and press `Enter` 
4. Then enter the node's password, press `enter`. You should now see your node's command line.
5. Disconnect from node machine
```sh
exit
```

#### Steps 1 - 3 Example
![1-3 Example](./img-p2/win01-03.gif)



#### Step 4 - Generate SSH Keys

```sh
ssh-keygen -t rsa -b 4096
```

- When prompted for "file in which to save," press `Enter`

- The passphrase is optional, but it is a good idea.

#### Step 5 - Copy SSH keys to node machine
Replace `<node-user>`, `<node-ip>`, and `<ssh-port>` with your information

```
cat ~/.ssh/id_rsa.pub | ssh <node-user>@<node-ip> -p <ssh-port> "cat >> ~/.ssh/authorized_keys"
```

#### Step 6: Simplify connection
We will create a desktop icon to simplify logging into your node

1. Right-click on the Windows desktop --> new -->  shortcut
2. In the "Type the location of the item" box, paste the command below. Replace the user-specific information.
```
wt ssh <node-user>@<node-ip> -p <ssh-port>
```
3. Choose any name for the shortcut.

You should now have an icon on your desktop that will automatcially open the SSH connection to your node machine.





  </TabItem>
  <TabItem value="ubuntu" label="Ubuntu">

## Configure Personal Device - Ubuntu
The next steps will configure the client software on a personal device running Ubuntu. You will use this device to control your node remotely.

#### Step 1: Connect to node (todo)



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

#### Step 3: Generate SSH Keys

SSH is more secure when using public/pricate keys instead of a password. In this step we well generate keys on your personal device and send the public key to the node machine.

On your **personal device**, create a new key pair for ssh authentication.

```
ssh-keygen -t rsa -b 4096
```

Copy the public key **keyname.pub** to a node machine. Replace **keyname.pub** with a key in home directory.

```
ssh-copy-id -i ~/.ssh/keyname.pub lukso
```

Test key login.  This time it should not prompt for a password.
```
ssh lukso
```
  </TabItem>
  <TabItem value="Windows" label="Windows (Putty)">
Coming Soon
  </TabItem>
  <TabItem value="macOS" label="macOS">
Coming Soon
  </TabItem>
</Tabs>

Remain connected and proceed to the next step

---
References
- https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement
- https://support.hostway.com/hc/en-us/articles/115001509884-How-To-Use-SSH-Keys-on-Windows-Clients-with-PuTTY-
- https://www.how2shout.com/how-to/how-to-login-into-ubuntu-using-ssh-from-windows-10-8-7.html
