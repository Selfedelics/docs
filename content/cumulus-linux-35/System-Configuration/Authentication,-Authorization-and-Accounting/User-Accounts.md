---
title: User Accounts
author: Cumulus Networks
weight: 269
aliases:
 - /display/CL35/User+Accounts
 - /pages/viewpage.action?pageId=8357334
pageID: 8357334
---
By default, Cumulus Linux has two user accounts: *cumulus* and *root*.

The *cumulus* account:

- Default password is *CumulusLinux\!*
- Is a user account in the *sudo* group with sudo privileges
- User can log in to the system via all the usual channels like console and
  [SSH](/cumulus-linux-35/System-Configuration/Authentication-Authorization-and-Accounting/SSH-for-Remote-Access)
- Along with the cumulus group, has both show and edit rights for
  [NCLU](/cumulus-linux-35/System-Configuration/Network-Command-Line-Utility-NCLU/)

The *root* account:

  - Default password is disabled by default
  - Has the standard Linux root user access to everything on the switch
  - Disabled password prohibits login to the switch by SSH, telnet, FTP,
    and so forth

For best security, you should change the default password (using the
`passwd` command) before you configure Cumulus Linux on the switch.

You can add more user accounts as needed. Like the *cumulus* account,
these accounts must use `sudo` to [execute privileged
commands](/cumulus-linux-35/System-Configuration/Authentication-Authorization-and-Accounting/Using-sudo-to-Delegate-Privileges),
so be sure to include them in the *sudo* group.

To access the switch without any password requires [booting into a
single shell/user
mode](/cumulus-linux-35/Monitoring-and-Troubleshooting/Single-User-Mode-Boot-Recovery).

You can add and configure user accounts in Cumulus Linux with read-only
or edit permissions for NCLU. For more information, see [Configuring
User
Accounts](/cumulus-linux-35/System-Configuration/Network-Command-Line-Utility-NCLU/).

## Enabling Remote Access for the root User

As mentioned above, the root user does not have a password set for it,
and it cannot log in to a switch via SSH. This default account behavior
is consistent with Debian. In order to connect to a switch using the
root account, you can do one of two things for the account:

  - Generate an SSH key

  - Set a password

### Generating an SSH Key for the root Account

1.  First, in a terminal on your host system (not the switch), check to
    see if a key already exists:
    
        root@host:~# ls -al ~/.ssh/
    
    The key is named something like `id_dsa.pub`, `id_rsa.pub` or
    `id_ecdsa.pub`.

2.  If a key doesn't exist, generate a new one by first creating the RSA
    key pair:
    
        root@host:~# ssh-keygen -t rsa

3.  A prompt appears, asking you to *Enter file in which to save the key
    (/root/.ssh/id\_rsa):*. Press Enter to use the root user's home
    directory, or else provide a different destination.

4.  You are prompted to *Enter passphrase (empty for no passphrase):*.
    This is optional but it does provide an extra layer of security.

5.  The public key is now located in `/root/.ssh/id_rsa.pub`. The
    private key (identification) is now located in `/root/.ssh/id_rsa`.

6.  Copy the public key to the switch. SSH to the switch as the cumulus
    user, then run:
    
        cumulus@switch:~$ sudo mkdir -p /root/.ssh
        cumulus@switch:~$ echo <SSH public key string> | sudo tee -a /root/.ssh/authorized_keys

### Setting the root User Password

1.  Run:
    
        cumulus@switch:~$ sudo passwd root

2.  Change the `/etc/ssh/sshd_config` file's `PermitRootLogin` setting
    from *without-password* to *yes*.
    
    ``` 
    cumulus@switch:~$ sudo nano /etc/ssh/sshd_config
     
    ... 
          
    # Authentication:
    LoginGraceTime 120
    PermitRootLogin yes
    StrictModes yes
          
    ...  
    ```

3.  Restart the `ssh` service:
    
        cumulus@switch:~$ sudo systemctl reload ssh.service
