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

The next steps will configure Windows PowerShell. You will use this device to control your node remotely.

#### Step 1 - Open Terminal as administrator

1. Search windows for the Terminal application
1. Right-click --> Run as administrator

:::tip
🔗[This guide has instruction for configuring Windows Terminal to run as administrator by default.](https://pureinfotech.com/always-run-windows-terminal-administrator-windows-10/#always_open_terminal_admin_settings) 
:::

#### Step 2 - Install OpenSSH
Copy/Paste this command into Terminal. To copy commands from the guide, hover over the top right corner of the gray command block and click the copy button. To paste into Terminal, right-click anywhere in the Terminal window.

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

#### Step 3 - Connect to node
1. Execute the command to test the SSH connection

```sh title="user-specific command - do not copy/paste"
ssh <node-user>@<node-ip> -p <ssh-port>
```

```sh title="example command"
ssh node@192.168.0.150 -p 1025
```

2. Type `yes` and press `Enter` when prompted with the authenticity warning.

3. Enter the node user's password

You should now see your node's command line.

The output should look like this

4. Disconnect from node machine
```sh
exit
```

#### Step 4 - Generate SSH Keys

```sh
ssh-keygen -t rsa -b 4096
```

- When prompted for "file in which to save," press `Enter`

- The passphrase is optional, but it is a good idea.

#### Step 5 - Copy SSH keys to node machine
Replace `<node-user>`, `<node-ip>`, and `<ssh-port>` with your information

```sh title="user-specific do not copy/paste"
cat ~/.ssh/id_rsa.pub | ssh <node-user>@<node-ip> -p <ssh-port> "cat >> ~/.ssh/authorized_keys"
```

```sh title="example command"
cat ~/.ssh/id_rsa.pub | ssh node@192.168.0.150 -p 1025 "cat >> ~/.ssh/authorized_keys"
```

#### Step 4: Simplify connection
We will create a config file to simplify the ssh command to `ssh lukso`

1. Open the config file in notepad
```
notepad.exe $env:userprofile\.ssh\config
```

2. Click "yes" when prompted to create a new file.

3. Copy/paste the text to notepad, then replace the user-specifit information

```sh title=\.ssh\config
Host lukso
  User <node-user>
  HostName <node-ip>
  Port <ssh-port>
```

4. Save and close the file.



  </TabItem>
  <TabItem value="Windows" label="Windows (Putty)">

The next steps will configure PuTTY (SSH client software) on a personal device running Windows. You will use this device to control your node remotely.

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

### Step 4: Create a PuTTY Profile to Save Your Server's Settings
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

:::
  </TabItem>
  <TabItem value="macOS" label="macOS">

The next steps will configure the client software on a personal device running Mac OS. You will use this device to control your node remotely.

#### Step 1: Open terminal
Go to Applications > Utilities, and then open Terminal

#### Step 2: Connect to node
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
</Tabs>

Remain connected and proceed to the next step

---
References
- https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement
- https://support.hostway.com/hc/en-us/articles/115001509884-How-To-Use-SSH-Keys-on-Windows-Clients-with-PuTTY-
- https://www.how2shout.com/how-to/how-to-login-into-ubuntu-using-ssh-from-windows-10-8-7.html
