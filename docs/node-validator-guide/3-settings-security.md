

# Part 3 - Settings & Security

#### 4.3 Disable root and password log in:

1. Change `ChallengeResponseAuthentication` to `no`
2. Change `PasswordAuthentication` to `no`
3. Change `PermitRootLogin` to `prohibit-password`
4. Change `PermitEmptyPasswords` to `no`
5. Save and exit by pressing `Ctrl + X`, then `y` , then `Enter`
 
Validate SSH configuration and restart ssh service.

```
sudo sshd -t
sudo systemctl restart sshd

### update
#### Manually update the system
    
```shell=
sudo apt-get update -y
sudo apt dist-upgrade -y
sudo apt-get autoremove
sudo apt-get autoclean
```
#### Configure auto-update
Keep a system up to date automatically:

```shell=
sudo apt-get install unattended-upgrades
sudo dpkg-reconfigure -plow unattended-upgrades
```

#### 4.4 Disable root access
A root access should not be used. Instead, a user should be using sudo to perform privilged operations on a system.

```
sudo passwd -l root
```

## Block Unauthorized Access
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


## Improve SSH Connection


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

---
References
- [Vlad's Guide](https://github.com/lykhonis/lukso-node-guide#auto-start)
- [Setup an Eth2 Mainnet Validator System on Ubuntu](https://github.com/metanull-operator/eth2-ubuntu)