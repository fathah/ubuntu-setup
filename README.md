# Initial Server Setup with Ubuntu 20.04
## Introduction
When you first create a new Ubuntu 20.04 server, you should perform some important configuration steps as part of the basic setup. These steps will increase the security and usability of your server, and will give you a solid foundation for subsequent actions.

### Step 1 — Logging in as root
To log into your server, you will need to know your **server’s public IP address**. You will also need the password or — if you installed an SSH key for authentication — the private key for the **root** user’s account. If you have not already logged into your server, you may want to follow our guide on how to connect to Droplets with SSH, which covers this process in detail.

If you are not already connected to your server, log in now as the **root** user using the following command (substitute the highlighted portion of the command with your server’s public IP address):
```console
$ ssh root@your_server_ip
```
Accept the warning about host authenticity if it appears. If you are using password authentication, provide your **root** password to log in. If you are using an SSH key that is passphrase protected, you may be prompted to enter the passphrase the first time you use the key each session. If this is your first time logging into the server with a password, you may also be prompted to change the **root** password.
#### About root
The **root** user is the administrative user in a Linux environment that has very broad privileges. Because of the heightened privileges of the **root** account, you are discouraged from using it on a regular basis. This is because part of the power inherent with the **root** account is the ability to make very destructive changes, even by accident.

The next step is setting up a new user account with reduced privileges for day-to-day use. Later, we’ll teach you how to gain increased privileges during only the times when you need them.

### Step 2 — Creating a New User
Once you are logged in as **root**, we’re prepared to add the new user account. In the future, we’ll log in with this new account instead of **root**.

This example creates a new user called **fathah**, but you should replace that with a username that you like:
```shell
# adduser fathah
```
You will be asked a few questions, starting with the account password.

Enter a strong password and, optionally, fill in any of the additional information if you would like. This is not required and you can just hit ```ENTER``` in any field you wish to skip.

### Step 3 — Granting Administrative Privileges
Now, we have a new user account with regular account privileges. However, we may sometimes need to do administrative tasks.

To avoid having to log out of our normal user and log back in as the **root** account, we can set up what is known as superuser or **root** privileges for our normal account. This will allow our normal user to run commands with administrative privileges by putting the word sudo before each command.

To add these privileges to our new user, we need to add the user to the **sudo** group. By default, on Ubuntu 20.04, users who are members of the **sudo** group are allowed to use the sudo command.

As **root**, run this command to add your new user to the **sudo** group (substitute the highlighted username with your new user):
```shell
# usermod -aG sudo fathah
```
Now, when logged in as your regular user, you can type sudo before commands to perform actions with superuser privileges.

### Step 4 — Setting Up a Basic Firewall
Ubuntu 20.04 servers can use the UFW firewall to make sure only connections to certain services are allowed. We can set up a basic firewall very easily using this application.

Applications can register their profiles with UFW upon installation. These profiles allow UFW to manage these applications by name. OpenSSH, the service allowing us to connect to our server now, has a profile registered with UFW.

You can see this by typing:
```shell
ufw app list
```
```
Output
Available applications:
  OpenSSH
  ```
We need to make sure that the firewall allows SSH connections so that we can log back in next time. We can allow these connections by typing:
```
ufw allow OpenSSH

```
Afterwards, we can enable the firewall by typing:
```
ufw enable
```
Type ```y``` and press ```ENTER``` to proceed. You can see that SSH connections are still allowed by typing:
```
ufw status
```
```
Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
```
As **the firewall is currently blocking all connections except for SSH**, if you install and configure additional services, you will need to adjust the firewall settings to allow traffic in.

### Step 5 — Enabling External Access for Your Regular User
Now that we have a regular user for daily use, we need to make sure we can SSH into the account directly.
> Note: Until verifying that you can log in and use sudo with your new user, we recommend staying logged in as root. This way, if you have problems, you can troubleshoot and make any necessary changes as root.

