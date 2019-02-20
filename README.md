# How To Secure A Linux Server

An evolving how-to guide for securing a Linux server that, hopefully, also teaches you a little about security and why it matters.

## Table of Contents

- [Table of Contents](#table-of-contents)
- [Introduction](#introduction)
  - [Guide Objective](#guide-objective)
  - [Why Secure Your Server](#why-secure-your-server)
  - [Why Yet Another Guide](#why-yet-another-guide)
  - [To Do / To Add](#to-do--to-add)
- [Guide Overview](#guide-overview)
  - [About This Guide](#about-this-guide)
  - [My Use-Case](#my-use-case)
  - [Editing Configuration Files - For The Lazy](#editing-configuration-files---for-the-lazy)
  - [Contributing](#contributing)
- [Before You Start](#before-you-start)
  - [Identify Your Principles](#identify-your-principles)
  - [Picking A Linux Distribution](#picking-a-linux-distribution)
  - [Installing Linux](#installing-linux)
  - [Pre/Post Installation Requirements](#prepost-installation-requirements)
  - [Other Important Notes](#other-important-notes)
- [The SSH Server](#the-ssh-server)
  - [SSH Public/Private Keys](#ssh-publicprivate-keys)
  - [Create SSH Group For `AllowGroups`](#create-ssh-group-for-allowgroups)
  - [Secure `/etc/ssh/sshd_config`](#secure-etcsshsshd_config)
  - [Remove Short Diffie-Hellman Keys](#remove-short-diffie-hellman-keys)
  - [2FA/MFA for SSH](#2famfa-for-ssh)
- [The Basics](#the-basics)
  - [Limit Who Can Use `sudo`](#limit-who-can-use-sudo)
  - [NTP Client](#ntp-client)
  - [Force Accounts To Use Secure Passwords](#force-accounts-to-use-secure-passwords)
  - [Automatic Security Updates and Alerts (INCOMPLETE)](#automatic-security-updates-and-alerts-incomplete)
  - [Apticron - Automatic Update Notifier (INCOMPLETE)](#apticron---automatic-update-notifier-incomplete)
- [The Firewall](#the-firewall)
  - [UFW: Uncomplicated Firewall](#ufw-uncomplicated-firewall)
  - [PSAD: `iptables` Intrusion Detection And Prevention](#psad-iptables-intrusion-detection-and-prevention)
  - [Fail2ban: Application Intrusion Detection And Prevention](#fail2ban-application-intrusion-detection-and-prevention)
- [The Danger Zone](#the-danger-zone)
  - [Proceed At Your Own Risk](#proceed-at-your-own-risk)
  - [Linux Kernel `sysctl` Hardening](#linux-kernel-sysctl-hardening)
  - [Password Protect GRUB](#password-protect-grub)
  - [Disable Root Login](#disable-root-login)
  - [Change Default `umask`](#change-default-umask)
  - [Orphaned Software](#orphaned-software)
- [The Auditing](#the-auditing)
  - [Lynis - Linux Security Auditing](#lynis---linux-security-auditing)
- [The Miscellaneous](#the-miscellaneous)
  - [Configure Gmail as MTA](#configure-gmail-as-mta)
  - [Separate `iptables` Log File](#separate-iptables-log-file)
- [Left Over](#left-over)
  - [Contacting Me](#contacting-me)
  - [Additional References](#additional-references)
  - [Acknowledgments](#acknowledgments)
  - [Disclaimer / Warranty](#disclaimer--warranty)

(TOC made with [nGitHubTOC](https://imthenachoman.github.io/nGitHubTOC/))

## Introduction

### Guide Objective

This guide's purpose is to teach you how to secure a Linux server. 

There are a lot of things you can do to secure a Linux server and this guide will attempt to cover as many of them as possible. More topics/material will be added as I learn, or as folks [contribute](#contributing).

([Table of Contents](#table-of-contents))

### Why Secure Your Server

I assume you're using this guide because you, hopefully, already understand why good security is important. That is a heavy topic onto itself and breaking it down is out-of-scope for this guide. If you don't know the answer to that question, I advise you research it first.

At a high level, the second a  device, like a server, is in the public domain -- i.e visible to the outside world -- it becomes a target for bad-actors. An unsecured device is a playground for bad-actors who want access to your data, or to use your server as another node for their large-scale DDOS attacks.

What's worse is, without good security, you may never know if your server has been compromised. A bad-actor may have gained unauthorized access to your server and copied your data without changing anything so you'd never know. Or your server may have been part of a DDOS attack and you wouldn't know. Look at many of the large scale data breaches in the news -- the companies often did not discover the data leak or intrusion until long after the bad-actors were gone.

Contrary to popular belief, bad-actors don't always want to change something or [lock you out of your data for money](). Sometimes they just want the data on your server for their data warehouses (there is big money in big data) or to covertly use your server for their nefarious purposes.

([Table of Contents](#table-of-contents))

### Why Yet Another Guide

This guide may appear duplicative/unnecessary because there are countless articles online that tell you [how to secure Linux](https://duckduckgo.com/?q=how+to+secure+linux&t=ffab&atb=v151-7&ia=web), but the information is spread across different articles, that cover different things, and in different ways. Who has time to scour through hundreds of articles?

As I was going through research for my Debian build, I kept notes. At the end I realized that, along with what I already knew, and what I was learning, I had the makings of a how-to guide. I figured I'd put it online to hopefully help others **learn**, and **save time**. 

I've never found one guide that covers everything -- this guide is my attempt.

Many of the things covered in this guide may be rather basic/trivial, but most of us do not install Linux every day and it is easy to forget those basic things.

IT automation tools like [Ansible](https://www.ansible.com/), [Chef](https://www.chef.io/), [Jenkins](https://jenkins.io/), [Puppet](https://puppet.com/), etc. help with the tedious task of installing/configuring a server but IMHO they are better suited for multiple or large scale deployments. IMHO, the overhead required to use those kinds of automation tools is wholly unnecessary for a one-time single server install for home use.

([Table of Contents](#table-of-contents))

### To Do / To Add

- [ ] [Custom Jails for Fail2ban](#custom-jails)
- [x] [Linux Kernel `sysctl` Hardening](#linux-kernel-sysctl-hardening)
- [ ] [Security-Enhanced Linux / SELinux](https://en.wikipedia.org/wiki/Security-Enhanced_Linux)
- [ ] disk encryption
- [x] BIOS password
- [ ] Anti-Virus
- [x] use ed25519 keys instead of RSA for SSH public/private keys
- [x] `psad`
- [ ] unattended upgrades for critical security updates and patches
- [ ] `logwatch`
- [ ] Rkhunter and chrootkit
- [ ] AppArmor
- [ ] port knockers for SSH
- [ ] https://linux-audit.com/linux-system-hardening-adding-hidepid-to-proc/
- [ ] https://likegeeks.com/secure-linux-server-hardening-best-practices/#Secure-Mounted-Filesystems
- [ ] shipping/backing up logs
- [ ] Tripwire
- [ ] MAC (Mandatory Access Control) and Linux Security Modules (LSMs)

([Table of Contents](#table-of-contents))

## Guide Overview

### About This Guide

This guide...

- ...**is** a work in progress.
- ...**is** focused on **at-home** Linux servers. All of the concepts/recommendations here apply to larger/professional environments but those use-cases call for more advanced and specialized configurations that are out-of-scope for this guide.
- ...**does not** teach you about Linux, how to [install Linux](#installing-linux), or how to use it.
- ...**is** meant to be [Linux distribution agnostic](#picking-a-linux-distribution).
- ...**does not** teach you everything you need to know about security nor does it get into all aspects of system/server security. For example, physical security is out of scope for this guide.
- ...**does not** talk about how programs/tools work, nor does it delve into their nook and crannies. Most of the programs/tools this guide references are very powerful and highly configurable. The goal is to cover the bare necessities -- enough to wet your appetite and make you hungry enough to want to go and learn more.
- ...**aims** to make it easy by providing code you can copy-and-paste. You might need to modify the commands before you paste so keep your favorite [text editor](https://notepad-plus-plus.org/) handy.
- ...**is** organized in an order that makes logical sense to me -- i.e. securing SSH before installing a firewall. As such, this guide is intended to be followed in the order it is presented but it is not necessary to do so. Just be careful if you do things in a different order -- some sections require previous sections to be completed. 

([Table of Contents](#table-of-contents))

### My Use-Case

There are many types of servers and different use-cases. While I want this guide to be as generic as possible, there will be some things that may not apply to all/other use-cases. Use your best judgement when going through this guide.

To help put context to many of the topics covered in this guide, my use-case/configuration is:

- A desktop class computer...
- With a single NIC...
- Connected to a consumer grade router...
- Getting a dynamic WAN IP provided by the ISP...
- With WAN+LAN on IPV4...
- And LAN using [NAT](https://en.wikipedia.org/wiki/Network_address_translation)...
- That I want to be able to SSH to remotely from unknown computers and unknown locations (i.e. a friend's house).

([Table of Contents](#table-of-contents))

### Editing Configuration Files - For The Lazy

I am very lazy and do not like to edit files by hand if I don't need to. I also assume everyone else is just like me. :)

So, when and where possible, I have provided `code` snippets to quickly do what is needed, like add or change a line in a configuration file.

The `code` snippets use basic commands like `echo`, `cat`, `sed`, `awk`, and `grep`. How the `code` snippets work, like what each command/part does, is out of scope for this guide -- the `man` pages are your friend.

**Note:** The `code` snippets do not validate/verify the change went through -- i.e. the line was actually added or changed. I'll leave the verifying part in your capable hands. The steps in this guide do include taking backups of all files that will be changed.

Not all changes can be automated with `code` snippets. Those changes need good, old fashioned, manual editing. For example, you can't just append a line to an [INI](https://en.wikipedia.org/wiki/INI_file) type file. Use your [favorite](https://en.wikipedia.org/wiki/Vi) Linux text editor.

([Table of Contents](#table-of-contents))

### Contributing

I wanted to put this guide on [GitHub](http://www.github.com) to make it easy to collaborate. The more folks that contribute, the better and more complete this guide will become.

To contribute you can fork and submit a pull request or submit a [new issue](https://github.com/imthenachoman/How-To-Secure-A-Linux-Server/issues/new).

([Table of Contents](#table-of-contents))

## Before You Start

### Identify Your Principles

Before you start you will want to identify what your Principles are. What is your [threat model](https://en.wikipedia.org/wiki/Threat_model)? Some things to think about:

- Why do you want to secure your server?
- How much security do you want or not want?
- How much convenience are you willing to compromise for security and vice-versa?
- What are the threats you want to protect against? What are the specifics to your situation? For example:
  - Is physical access to your server/network a possible attack vector? 
  - Will you be opening ports on your router so you can access your server from outside your home?
  - Will you be hosting a file share on your server that will be mounted on a desktop class machine? What is the possibility of the desktop machine getting infected and, in turn, infecting the server?
 - Do you have a means of recovering if your security implementation locks you out of your own server? For example, you [disabled root login](#disable-root-login) or [password protected GRUB](#password-protect-grub).

These are just **a few things** to think about. Before you start securing your server you will want to understand what you're trying to protect against and why so you know what you need to do.

([Table of Contents](#table-of-contents))

### Picking A Linux Distribution

<a name="distribution-agnostic"></a>This guide is intended to be distribution agnostic so users can use [any distribution](https://distrowatch.com/) they want. With that said, there are a few things to keep in mind:

You want a distribution that...

- ...**is stable**. Unless you like debugging issues at 2 AM, you don't want an [unattended upgrade](#tbd), or a manual package/system update, to render your server inoperable. But this also means you're okay with not running the latest, greatest, bleeding edge software. 
- ...**stays up-to-date with security patches**. You can secure everything on your server, but if the core OS or applications you're running have known vulnerabilities, you'll never be safe.
- ...**you're familiar with.** If you don't know Linux, I would advise you play around with one before you try to secure it. You should be comfortable with it and know your way around, like how to install software, where configuration files are, etc...
- ...**is well supported.** Even the most seasoned admin needs help every now and then. Having a place to go for help will save your sanity.

([Table of Contents](#table-of-contents))

### Installing Linux

Installing Linux is out-of-scope for this guide because each distribution does it differently and the installation instructions are usually well documented. If you need help, start with your distribution's documentation. Regardless of the distribution, the high-level process usually goes like so:

1. download the ISO
1. burn/copy/transfer it to your install medium (e.g. a CD or USB stick)
1. boot your server from your install medium
1. follow the prompts to install

Where applicable, use the expert install option so you have tighter control of what is running on your server. **Only install what you absolutely need.** I, personally, do not install anything other than SSH.

([Table of Contents](#table-of-contents))

### Pre/Post Installation Requirements

- If you're opening ports on your router so you can access your server from the outside, disable the port forwarding until your system is up and secured. 
- Unless you're doing everything physically connected to your server, you'll need remote access so be sure SSH works.
- Keep your system up-to-date (i.e. `sudo apt update && sudo apt upgrade` on Debian based systems).
- Make sure you perform any tasks specific to your setup like:
  - Configuring network
  - Configuring mount points in `/etc/fstab`
  - Creating the initial user accounts
  - Installing core software you'll want like `man`
  - Etc...
- Your server will need to be able to send e-mails so you can get important security alerts. If you're not setting up a mail server check [Configure Gmail as MTA](#configure-gmail-as-mta). 

([Table of Contents](#table-of-contents))

### Other Important Notes

- This guide is being written and tested on Debian. Most things below should work on other distributions. If you find something that does not, please [contact me](#contacting-me). The main thing that separates each distribution will be its package management system. Since I use Debian, I will provide the appropriate `apt` commands that should work on all [Debian based distributions](https://www.debian.org/derivatives/). If someone is willing to [provide](#contributing) the respective commands for other distributions, I will add them.
- File paths and settings also may differ slightly -- check with your distribution's documentation if you have issues.
- Read the whole guide before you start. Your use-case and/or principals may call for not doing something or for changing the order.
- Do not **blindly** copy-and-paste without understanding what you're pasting. Some commands will need to be modified for your needs before they'll work -- usernames for example.

([Table of Contents](#table-of-contents))

## The SSH Server

### SSH Public/Private Keys

#### Why

Using SSH public/private keys is more secure than using a password. It also makes it easier and faster, to connect to our server because you don't have to enter a password.

#### How It Works

Check the [references](#ssh-key-references) below for more details but, at a high level, public/private keys work by using a pair of keys to verify identity.

1. One key, the **public** key, **can only encrypt data**, not decrypt it
1. The other key, the **private** key, can decrypt the data

For SSH, a public and private key is created on the client. You want to keep both keys secure, especially the private key. Even though the public key is meant to be public, it is wise to make sure neither keys fall fall in the wrong hands.

When you connect to an SSH server, SSH will look for a public key that matches the client you're conneting from in the file `~/.ssh/authorized_keys` on the server you're connecting to. Notice the file is in the **home folder** of the ID you're trying to connect to. So, after creating the public key, you need to append it to `~/.ssh/authorized_keys`. One approach is to copy it to a USB stick and physically transfer it to the server. Or, if you're sure there is [nobody listening between the client you're on and your server](https://en.wikipedia.org/wiki/Man-in-the-middle_attack), you can use `ssh-copy-id` to transfer and append the public key.

After the keys have been created and the public key has been appended to `~/.ssh/authorized_keys` on the host, SSH uses the public and private keys to verify identity and then establish a secure connection. How identity is verified is a complicated process but [Digital Ocean](https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process) has a very nice writeup of how it works. At a high level, identity is verified by the server encrypting a challenge message with the public key, then sending it to the client. If the client cannot decrypt the challenge message with the private key, the identity can't be verified and a connection will not be established.

They are considered more secure because you need the private key to establish an SSH connection. If you set [`PasswordAuthentication no` in `/etc/ssh/sshd_config`](#PasswordAuthentication), then SSH won't let you connect without the private key. 

You can also set a passphrase for the keys which would require you to enter the key passphrase when connecting using public/private keys. Keep in mind doing this means you can't use the key for automation because you'll have no way to send the passphrase in your scripts. `ssh-agent` is a program that is shipped in many Linux distros (and usually already running) that will allow you to hold your unencrypted private key in memory for a configurable duration. Simply run `ssh-add` and it will prompt you for your passphrase. You will not be prompted for your passphrase again until the configurable duration has passed.

We will be using Ed25519 keys which, according to [https://linux-audit.com/](https://linux-audit.com/using-ed25519-openssh-keys-instead-of-dsa-rsa-ecdsa/):

> It is using an elliptic curve signature scheme, which offers better security than ECDSA and DSA. At the same time, it also has good performance.

#### Goals

- Ed25519 public/private SSH keys:
  - private key on your client
  - public key on your server

#### Notes

- You'll need to do this step for every computer and account you'll be connecting to your server from/as.

#### <a name="ssh-key-references"></a>References

- https://www.ssh.com/ssh/public-key-authentication
- https://help.ubuntu.com/community/SSH/OpenSSH/Keys
- https://linux-audit.com/using-ed25519-openssh-keys-instead-of-dsa-rsa-ecdsa/
- https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process
- https://wiki.archlinux.org/index.php/SSH_Keys
- `man ssh-keygen`
- `man ssh-copy-id`
- `man ssh-add`

#### Steps

1. From the computer you're going to use to connect to your server, **the client**, not the server itself, create an [Ed25519](https://linux-audit.com/using-ed25519-openssh-keys-instead-of-dsa-rsa-ecdsa/) key with `ssh-keygen`:

    ``` bash
    ssh-keygen -t ed25519
    ```

    > ```
    > Generating public/private ed25519 key pair.
    > Enter file in which to save the key (/home/user/.ssh/id_ed25519):
    > Created directory '/home/user/.ssh'.
    > Enter passphrase (empty for no passphrase):
    > Enter same passphrase again:
    > Your identification has been saved in /home/user/.ssh/id_ed25519.
    > Your public key has been saved in /home/user/.ssh/id_ed25519.pub.
    > The key fingerprint is:
    > SHA256:F44D4dr2zoHqgj0i2iVIHQ32uk/Lx4P+raayEAQjlcs user@client
    > The key's randomart image is:
    > +--[ED25519 256]--+
    > |xxxx  x          |
    > |o.o +. .         |
    > | o o oo   .      |
    > |. E oo . o .     |
    > | o o. o S o      |
    > |... .. o o       |
    > |.+....+ o        |
    > |+.=++o.B..       |
    > |+..=**=o=.       |
    > +----[SHA256]-----+
    > ```
    
    **Note**: If you set a passphrase, you'll need to enter it every time you connect to your server using this key, unless you're using `ssh-agent`.

1. Now you need to **append** the public key `~/.ssh/id_ed25519.pub` from your client to the `~/.ssh/authorized_keys` file on your server. Since we're presumable still at home on the LAN, we're probably safe from [MIM](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) attacks, so we will use `ssh-copy-id` to transfer and append the public key:

    ``` bash
    ssh-copy-id user@server    
    ```
    
    > ```
    > /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/user/.ssh/id_ed25519.pub"
    > The authenticity of host 'host (192.168.1.96)' can't be established.
    > ECDSA key fingerprint is SHA256:QaDQb/X0XyVlogh87sDXE7MR8YIK7ko4wS5hXjRySJE.
    > Are you sure you want to continue connecting (yes/no)? yes
    > /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
    > /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
    > user@host's password:
    > 
    > Number of key(s) added: 1
    > 
    > Now try logging into the machine, with:   "ssh 'user@host'"
    > and check to make sure that only the key(s) you wanted were added.
    > ```

Now would be a good time to [perform any tasks specific to your setup](#prepost-installation).

([Table of Contents](#table-of-contents))

### Create SSH Group For `AllowGroups`

#### Why

To make it easy to control who can SSH to the server. By using a group, we can quickly add/remove accounts to the group to quickly allow or not allow SSH access to the server.

#### How It Works

We will use the [`AllowGroups` option](#AllowGroups) in SSH's configuration file [`/etc/ssh/sshd_config`](#secure-etcsshsshd_config). to tell the SSH server to only allow users to SSH in if they are a member of a certain UNIX group. Anyone not in the group will not be able to SSH in. 

#### Goals

- a UNIX group that we'll use in [Secure `/etc/ssh/sshd_config`](#secure-etcsshsshd_config) to limit who can SSH to the server

#### Notes

- This is a per-requisite step to support the `AllowGroup` setting set in [Secure `/etc/ssh/sshd_config`](#secure-etcsshsshd_config).

#### References

- `man groupadd`
- `man usermod`

#### Steps

1. Create a group:

    ``` bash
    sudo groupadd sshusers
    ```

1. Add account(s) to the group:

    ``` bash
    sudo usermod -a -G sshusers user1
    sudo usermod -a -G sshusers user2
    sudo usermod -a -G sshusers ...
    ```
    
    You'll need to do this for every account on your server that needs SSH access.

([Table of Contents](#table-of-contents))

### Secure `/etc/ssh/sshd_config`

#### Why

SSH is a door into your server. This is especially true if you are opening ports on your router so you can SSH to your server from outside your home network. If it is not secured properly, a bad-actor could use it to gain unauthorized access to your system.

#### How It Works

`/etc/ssh/sshd_config` is the default configuration file that the SSH server uses. We will use this file to tell what options the SSH server should use.

#### Goal

- a secure SSH configuration

#### Notes

- Make sure you've completed [Create SSH Group For `AllowGroups`](#create-ssh-group-for-allowgroups) first.

#### References

- Mozilla's OpenSSH guidelines for OpenSSH 6.7+ at https://infosec.mozilla.org/guidelines/openssh#modern-openssh-67
- https://linux-audit.com/audit-and-harden-your-ssh-configuration/
- https://www.ssh.com/ssh/sshd_config/
- https://www.techbrown.com/harden-ssh-secure-linux-vps-server/
- https://serverfault.com/questions/660160/openssh-difference-between-internal-sftp-and-sftp-server/660325
- `man sshd_config`

#### Steps

1. Make a backup of `/etc/ssh/sshd_config` and remove default comments to make it easier to read:

    ``` bash
    sudo cp --preserve /etc/ssh/sshd_config /etc/ssh/sshd_config.$(date +"%Y%m%d%H%M%S")
    sudo sed -i -r -e '/^#|^$/ d' /etc/ssh/sshd_config
    ```
    
1. Edit `/etc/ssh/sshd_config` then **find and edit or add** these settings that should apply regardless of your configuration/setup:
    
    **Note**: Your `/etc/ssh/sshd_config` file may already have some of these settings/lines. You will want to remove those and replace them with the ones below.
    
    ```
    ########################################################################################################
    # start settings from https://infosec.mozilla.org/guidelines/openssh#modern-openssh-67 as of 2019-01-01
    ########################################################################################################

    # Supported HostKey algorithms by order of preference.
    HostKey /etc/ssh/ssh_host_ed25519_key
    HostKey /etc/ssh/ssh_host_rsa_key
    HostKey /etc/ssh/ssh_host_ecdsa_key

    KexAlgorithms curve25519-sha256@libssh.org,ecdh-sha2-nistp521,ecdh-sha2-nistp384,ecdh-sha2-nistp256,diffie-hellman-group-exchange-sha256

    Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr

    MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-512,hmac-sha2-256,umac-128@openssh.com

    # LogLevel VERBOSE logs user's key fingerprint on login. Needed to have a clear audit track of which key was using to log in.
    LogLevel VERBOSE

    # Use kernel sandbox mechanisms where possible in unprivileged processes
    # Systrace on OpenBSD, Seccomp on Linux, seatbelt on MacOSX/Darwin, rlimit elsewhere.
    # Note: This setting is deprecated in OpenSSH 7.5 (https://www.openssh.com/txt/release-7.5)
    UsePrivilegeSeparation sandbox

    ########################################################################################################
    # end settings from https://infosec.mozilla.org/guidelines/openssh#modern-openssh-67 as of 2019-01-01
    ########################################################################################################
    
    # Log sftp level file access (read/write/etc.) that would not be easily logged otherwise.
    Subsystem sftp  internal-sftp -f AUTHPRIV -l INFO
    
    # only use the newer, more secure protocl
    Protocol 2

    # disable X11 forwarding as X11 is very insecure
    # you really shouldn't be running X on a server anyway
    X11Forwarding no

    # disable port forwarding
    AllowTcpForwarding no
    AllowStreamLocalForwarding no
    GatewayPorts no
    PermitTunnel no

    # don't allow login if the account has an empty password
    PermitEmptyPasswords no

    # ignore .rhosts and .shosts
    IgnoreRhosts yes

    # verify hostname matches IP
    UseDNS no

    Compression no
    TCPKeepAlive no
    AllowAgentForwarding no
    PermitRootLogin no
    ```
   
1. Then **find and edit or add** these settings, and set values as per your requirements:

    |Setting|Valid Values|Example|Description|Notes|
    |--|--|--|--|--|
    |<a name="AllowGroups"></a>**AllowGroups**|local UNIX group name|`AllowGroups sshusers`|group to allow SSH access to||
    |**ClientAliveCountMax**|number|`ClientAliveCountMax 0`|maximum number of client alive messages sent without response||
    |**ClientAliveInterval**|number of seconds|`ClientAliveInterval 300`|timeout in seconds before a response request||
    |**ListenAddress**|space separated list of local addresses|<ul><li>`ListenAddress 0.0.0.0`</li><li>`ListenAddress 192.168.1.100`</li></ul>|local addresses `sshd` should listen on|See [Issue #1](https://github.com/imthenachoman/How-To-Secure-A-Linux-Server/issues/1) for important details.|
    |**LoginGraceTime**|number of seconds|`LoginGraceTime 30`|time in seconds before login times-out||
    |**MaxAuthTries**|number|`MaxAuthTries 2`|maximum allowed attempts to login||
    |**MaxSessions**|number|`MaxSessions 2`|maximum number of open sessions||
    |**MaxStartups**|number|`MaxStartups 2`|maximum number of login sessions||
    |<a name="PasswordAuthentication"></a>**PasswordAuthentication**|`yes` or `no`|`PasswordAuthentication no`|if login with a password is allowed||
    |**Port**|any open/available port number|`Port 22`|port that `sshd` should listen on||
    
    Check `man sshd_config` for more details what these settings mean.

1. Restart ssh:

    ``` bash
    sudo service sshd restart
    ```

([Table of Contents](#table-of-contents))

### Remove Short Diffie-Hellman Keys

#### Why

Per [Mozilla's OpenSSH guidelines for OpenSSH 6.7+](https://infosec.mozilla.org/guidelines/openssh#modern-openssh-67), "all Diffie-Hellman moduli in use should be at least 3072-bit-long".

The Diffie-Hellman algorithm is used by SSH to establish a secure connection. The larger the moduli (key size) the stronger the encryption. 

#### Goal

- remove all Diffie-Hellman keys that are less than 3072 bits long

#### References

- Mozilla's OpenSSH guidelines for OpenSSH 6.7+ at https://infosec.mozilla.org/guidelines/openssh#modern-openssh-67
- https://infosec.mozilla.org/guidelines/key_management
- `man moduli`

#### Steps

1. Make a backup of `/etc/ssh/moduli`:

    ``` bash
    sudo cp --preserve /etc/ssh/moduli /etc/ssh/moduli.$(date +"%Y%m%d%H%M%S")
    ```

1. Remove short moduli:

    ``` bash
    sudo awk '$5 >= 3071' /etc/ssh/moduli | sudo tee /etc/ssh/moduli.tmp
    sudo mv /etc/ssh/moduli.tmp /etc/ssh/moduli
    ````

([Table of Contents](#table-of-contents))

### 2FA/MFA for SSH

#### Why

Even though SSH is a pretty good security guard for your doors and windows, it is still a visible door that bad-actors can see and try to brute-force in. [Fail2ban](#fail2ban-application-intrusion-detection-and-prevention) will monitor for these brute-force attempts but there is no such thing as being too secure. Requiring two factors adds an extra layer of security.

#### Why Not

Many folks might find the experience cumbersome or annoying. And, access to your system is dependent on the accompanying authenticator app that generates the code.

#### How It work

Using Two Factor Authentication (2FA) / Multi Factor Authentication (MFA) requires anyone entering to have **two** keys to enter which makes it harder for bad actors. The two keys are:

1. Their password 
1. A 6 digit token that changes every 30 seconds

Without both keys, they won't be able to get in.

WIP

#### Goals

- 2FA/MFA enabled for all SSH connections

#### Notes

- Before you do this, you should have an idea of how 2FA/MFA works and you'll need an authenticator app on your phone to continue. 
- We'll use [google-authenticator-libpam](https://github.com/google/google-authenticator-libpam).
- With the below configuration, a user will only need to enter their 2FA/MFA code if they are logging on with their password but **not** if they are using [SSH public/private keys](#ssh-publicprivate-keys). Check the documentation on how to change this behavior to suite your requirements.

#### References

- https://github.com/google/google-authenticator-libpam

#### Steps

1. Install it `libpam-google-authenticator`.
    
    On Debian based systems:
    
    ``` bash
    sudo apt install libpam-google-authenticator
    ```
    
1. **Make sure you're logged in as the ID you want to enable 2FA/MFA for** and **execute** `google-authenticator` to create the necessary token data:

    ``` bash
    google-authenticator
    ```
    
    > ```
    > Do you want authentication tokens to be time-based (y/n) y
    > https://www.google.com/chart?chs=200x200&chld=M|0&cht=qr&chl=otpauth://totp/user@host%3Fsecret%3DR4ZWX34FQKZROVX7AGLJ64684Y%26issuer%3Dhost
    > 
    > ...
    > 
    > Your new secret key is: R3NVX3FFQKZROVX7AGLJUGGESY
    > Your verification code is 751419
    > Your emergency scratch codes are:
    >   12345678
    >   90123456
    >   78901234
    >   56789012
    >   34567890
    > 
    > Do you want me to update your "/home/user/.google_authenticator" file (y/n) y
    > 
    > Do you want to disallow multiple uses of the same authentication
    > token? This restricts you to one login about every 30s, but it increases
    > your chances to notice or even prevent man-in-the-middle attacks (y/n) Do you want to disallow multiple uses of the same authentication
    > token? This restricts you to one login about every 30s, but it increases
    > your chances to notice or even prevent man-in-the-middle attacks (y/n) y
    > 
    > By default, tokens are good for 30 seconds. In order to compensate for
    > possible time-skew between the client and the server, we allow an extra
    > token before and after the current time. If you experience problems with
    > poor time synchronization, you can increase the window from its default
    > size of +-1min (window size of 3) to about +-4min (window size of
    > 17 acceptable tokens).
    > Do you want to do so? (y/n) y
    > 
    > If the computer that you are logging into isn't hardened against brute-force
    > login attempts, you can enable rate-limiting for the authentication module.
    > By default, this limits attackers to no more than 3 login attempts every 30s.
    > Do you want to enable rate-limiting (y/n) y
    > ```
    
    Notice this is **not run as root**.
    
    Select default option (y in most cases) for all the questions it asks and remember to save the emergency scratch codes.
    
1. Now we need to enable it as an authentication method for SSH by **adding** this line to `/etc/pam.d/sshd`:

    ```
    auth       required     pam_google_authenticator.so nullok
    ```
    
    Check [here](https://github.com/google/google-authenticator-libpam/blob/master/README.md#nullok) for what `nullok` means.
    
    [For the lazy](#editing-configuration-files---for-the-lazy):
    
    ``` bash
    sudo cp --preserve /etc/pam.d/sshd /etc/pam.d/sshd.$(date +"%Y%m%d%H%M%S")
    
    echo -e "\nauth       required     pam_google_authenticator.so nullok         # added by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")" | sudo tee -a /etc/pam.d/sshd
    ```
    
1. Tell SSH to levearage it by **adding** this line in `/etc/ssh/sshd_config`:
    
    ```
    ChallengeResponseAuthentication yes
    ```
    
    [For the lazy](#editing-configuration-files---for-the-lazy):
    
    ``` bash
    sudo cp --preserve /etc/ssh/sshd_config /etc/ssh/sshd_config.$(date +"%Y%m%d%H%M%S")
    
    echo -e "\nChallengeResponseAuthentication yes         # added by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")" | sudo tee -a /etc/ssh/sshd_config
    ```
    
1. Restart `ssh`:

    ``` bash
    sudo service sshd restart
    ```

([Table of Contents](#table-of-contents))

## The Basics

### Limit Who Can Use `sudo`

#### Why

`sudo` lets accounts run commands as other accounts, including **root**. We want to make sure that only the accounts we want can use `sudo`.

#### How It Works

WIP

#### Goals

- `sudo` privileges limited to those who are in a group we specify

#### Notes

- Your installation may have already done this, or may already have a special group intended for this purpose so check first.
  - Debian creates the `sudo` group
  - RedHat creates the `wheel` group

#### Steps

1. Create a group:

    ``` bash
    sudo groupadd sudousers
    ```

1. Add account(s) to the group:

    ``` bash
    sudo usermod -a -G sudousers user1
    sudo usermod -a -G sudousers user2
    sudo usermod -a -G sudousers  ...
    ```
    
    You'll need to do this for every account on your server that needs `sudo` privileges.

1. Edit `/etc/sudoers`:

    ``` bash
    sudo cp --preserve /etc/sudoers /etc/sudoers.$(date +"%Y%m%d%H%M%S")
    sudo visudo
    ```

1. Tell `sudo` to only allow users in the `sudousers` group to use `sudo` by **adding** this line if it is not already there:

    ```
    %sudousers   ALL=(ALL:ALL) ALL
    ```

([Table of Contents](#table-of-contents))

### NTP Client

#### Why

Many security protocols leverage the time. If your system time is incorrect, it could have negative impacts to your server. An NTP client can solve that problem by keeping your system time in-sync with [global NTP servers](https://www.pool.ntp.org/en/).

#### How It Works

WIP

#### Goals

- NTP client installed and keeping server time in-sync

#### References

- https://cloudpro.zone/index.php/2018/01/27/debian-9-3-server-setup-guide-part-4/

#### Steps

1. Install `ntp`.

    On Debian based systems:
    
    ``` bash
    sudo apt install ntp
    ```

1. Check `ntp`'s status:

    ``` bash
    sudo systemctl status ntp
    ```

    > ```
    > ● ntp.service - LSB: Start NTP daemon
    >    Loaded: loaded (/etc/init.d/ntp; generated; vendor preset: enabled)
    >    Active: active (running) since Sat 2019-02-16 00:32:20 EST; 3s ago
    >      Docs: man:systemd-sysv-generator(8)
    >    CGroup: /system.slice/ntp.service
    >            └─1051 /usr/sbin/ntpd -p /var/run/ntpd.pid -g -u 109:114
    > 
    > Feb 16 00:32:20 host ntpd[1051]: Listen normally on 3 enp0s3 192.168.1.96:123
    > Feb 16 00:32:20 host ntpd[1051]: Listen normally on 4 lo [::1]:123
    > Feb 16 00:32:20 host ntpd[1051]: Listen normally on 5 enp0s3 [fe80::a00:27ff:feb6:ed8e%2]:123
    > Feb 16 00:32:20 host ntpd[1051]: Listening on routing socket on fd #22 for interface updates
    > Feb 16 00:32:21 host ntpd[1051]: Soliciting pool server 173.255.206.154
    > Feb 16 00:32:22 host ntpd[1051]: Soliciting pool server 216.6.2.70
    > Feb 16 00:32:22 host ntpd[1051]: Soliciting pool server 82.197.188.130
    > Feb 16 00:32:23 host ntpd[1051]: Soliciting pool server 95.215.175.2
    > Feb 16 00:32:23 host ntpd[1051]: Soliciting pool server 107.155.79.108
    > Feb 16 00:32:23 host ntpd[1051]: Soliciting pool server 212.110.158.28
    > ```
    
    ``` bash
    sudo ntpq -p
    ```
    
    > ```
    >      remote           refid      st t when poll reach   delay   offset  jitter
    > ==============================================================================
    >  0.debian.pool.n .POOL.          16 p    -   64    0    0.000    0.000   0.000
    >  1.debian.pool.n .POOL.          16 p    -   64    0    0.000    0.000   0.000
    >  2.debian.pool.n .POOL.          16 p    -   64    0    0.000    0.000   0.000
    >  3.debian.pool.n .POOL.          16 p    -   64    0    0.000    0.000   0.000
    > -li216-154.membe 45.56.123.24     3 u  119   64    2   51.912    0.663   2.311
    > +eudyptula.init7 162.23.41.10     2 u   60   64    3   99.378    1.563   3.485
    > +107.155.79.108  129.7.1.66       2 u  119   64    2   49.171   -1.372   1.441
    > -212.110.158.28  89.109.251.21    2 u  120   64    2  167.465   -1.064   1.263
    > *ec2-54-242-183- 128.10.19.24     2 u   62   64    3   19.157    2.536   4.434
    > -69.195.159.158  128.252.19.1     2 u  119   64    2   42.990    6.302   3.507
    > -200.89.75.198 ( 200.27.106.115   2 u   58   64    3  160.786   42.737  12.827
    > ```

([Table of Contents](#table-of-contents))

### Force Accounts To Use Secure Passwords

#### Why

By default, accounts can use any password they want, including bad ones. [pwquality](https://linux.die.net/man/5/pwquality.conf)/[pam_pwquality](https://linux.die.net/man/8/pam_pwquality) addresses this security gap by providing "a way to configure the default password quality requirements for the system passwords" and checking "its strength against a system dictionary and a set of rules for identifying poor choices."

#### How It Works

WIP

#### Goal

- enforced strong passwords

#### Steps

1. Install `libpam-pwquality`.

    On Debian based systems:
    
    ``` bash
    sudo apt install libpam-pwquality
    ```

1. Tell PAM to use `libpam-pwquality` to enforce strong passwords by editing the file `/etc/pam.d/common-password` and **change** the line that starts like this:

    ```
    password        requisite                       pam_pwquality.so
    ```
    
    to this:

    ```
    password        requisite                       pam_pwquality.so retry=3 minlen=10 difok=3 ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1 maxrepeat=3 gecoschec
    ```
   
   The above options are:

     - `retry=3` = prompt user 3 times before returning with error.
     - `minlen=10` = the minimum length of the password, factoring in any credits (or debits) from these:
       - `dcredit=-1` = must have at least **one digit**
       - `ucredit=-1` = must have at least **one upper case letter**
       - `lcredit=-1` = must have at least **one lower case letter**
       - `ocredit=-1` = must have at least **one non-alphanumeric character**
     - `difok=3` = at least 3 characters from the new password cannot have been in the old password
     - `maxrepeat=3` = allow a maximum of 3 repeated characters
     - `gecoschec` = do not allow passwords with the account's name


    [For the lazy](#editing-configuration-files---for-the-lazy):
   
    ``` bash
    sudo cp --preserve /etc/pam.d/common-password /etc/pam.d/common-password.$(date +"%Y%m%d%H%M%S")
    
    sudo sed -i -r -e "s/^(password\s+requisite\s+pam_pwquality.so)(.*)$/# \1\2         # commented by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")\n\1 retry=3 minlen=10 difok=3 ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1 maxrepeat=3 gecoschec         # added by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")/" /etc/pam.d/common-password
    ```

([Table of Contents](#table-of-contents))

### Automatic Security Updates and Alerts (INCOMPLETE)

#### Why

It is important to keep a server updated with the latest security patches and updates. You can do this manually or have it automated. 

#### Why Not

Automatic and unattended updates may break your system and you may not be near your server to fix it. This would be especially problematic if it broke your SSH access.

#### Notes

- Each distribution manages packages and updates differently. So far I only have steps for Debian based systems.

#### Goals

- Automatic, unattended, updates of critical security patches
- Automatic emails of remaining pending updates 

#### Debian Based Systems

##### How It Works

On Debian based systems you can use:

- `unattended-upgrades` to automatically do system updates you want (i.e. critical security updates)
- `apt-listchanges` to get emails about package changes
- `apticron` to get emails for pending package updates

##### References

- https://wiki.debian.org/UnattendedUpgrades
- https://debian-handbook.info/browse/stable/sect.regular-upgrades.html
- https://blog.sleeplessbeastie.eu/2015/01/02/how-to-perform-unattended-upgrades/
- https://github.com/mvo5/unattended-upgrades
- https://www.vultr.com/docs/how-to-set-up-unattended-upgrades-on-debian-9-stretch

##### Steps

1. Install `unattended-upgrades`, `apt-listchanges`, and `apticron`:

    ``` bash
    sudo apt install unattended-upgrades apt-listchanges apticron
    ```

1. Now we need to configure `unattended-upgrades` to automatically apply critical security patches. This is typically done by editing the file `/etc/apt/apt.conf.d/50unattended-upgrades` that was created by the package. However, because this file may get overwritten with a future update, we'll create a new file instead. **Create** the file `/etc/apt/apt.conf.d/51myunattended-upgrades` and add this:

    ```
    APT::Periodic::Update-Package-Lists "1";
    APT::Periodic::Download-Upgradeable-Packages "1";
    APT::Periodic::AutocleanInterval "7";
    APT::Periodic::Unattended-Upgrade "1";
    Unattended-Upgrade::Mail "root";

    // Automatically upgrade packages from these 
    Unattended-Upgrade::Origins-Pattern {
          "o=Debian,a=stable";
          "o=Debian,a=stable-updates";
          "o=Debian,a=proposed-updates";
          "origin=Debian,codename=${distro_codename},label=Debian-Security";
    };

    // You can specify your own packages to NOT automatically upgrade here
    Unattended-Upgrade::Package-Blacklist {
    //      "vim";
    //      "libc6";
    //      "libc6-dev";
    //      "libc6-i686";

    };

    Unattended-Upgrade::MailOnlyOnError "true";
    Unattended-Upgrade::Automatic-Reboot "false";
    ```
    
    **Notes**
    
    - Your server will need to be able to send e-mails so letting you when it has updated the system and when packages are avi
    - Check `/etc/apt/apt.conf.d/50unattended-upgrades`

### Apticron - Automatic Update Notifier (INCOMPLETE)

#### Why

It is important to keep your server up-to-date with all security patches. Otherwise you're at risk of known security vulnerabilities that bad-actors could use to gain unauthorized access to your server.

You have two options:

- Configure your server for unattended updates
- Be notified when updates are available

Which option you pick is up to you but I prefer being notified by e-mail when updates are available. This is because an update may break something else. If the server updates it-self then I may not know and, if I do find out, I'll have to scramble to fix it. If it e-mails me when updates are available, then I can do the updates at my schedule.

#### How It Works

WIP

#### Notes

- Your server will need a way to send e-mails for this to work

#### References

- https://wiki.debian.org/UnattendedUpgrades#apt-listchanges
- https://www.cyberciti.biz/faq/apt-get-apticron-send-email-upgrades-available/
- https://www.unixmen.com/how-to-get-email-notifications-for-new-updates-on-debianubuntu/

#### Steps

1. Install `apticron`.
    
    On Debian based systems:
    
    ``` bash
    sudo apt install apticron
    ```
1. Set the value of `EMAIL` in `/etc/apticron/apticron.conf` to your e-mail address. 

([Table of Contents](#table-of-contents))

## The Firewall

### UFW: Uncomplicated Firewall

#### Why

Call me paranoid, and you don't have to agree, but I want to deny all traffic in and out of my server except what I explicitly allow. Why would my server be sending traffic out that I don't know about? And why would external traffic be trying to access my server if I don't know who or what it is? When it comes to good security, my opinion is to reject/deny by default, and allow by exception.

Of course, if you disagree, that is totally fine and can configure UFW to suit your needs.

Either way, ensuring that only traffic we explicitly allow is the job of a firewall. On Linux, the most common firewall is [`iptables`](https://en.wikipedia.org/wiki/Iptables). `iptables`, however, is rather complicated and confusing (IMHO). This is where UFW comes in. UFW simplifies the process of creating and managing `iptables` rules. 

**UFW** works by letting you configure rules that:

- **allow** or **deny**
- **input** or **output** traffic
- **to** or **from** ports

You can create rules by explicitly specifying the ports or with application configurations that specify the ports.

#### How It Works

WIP

#### Goal

 - all network traffic, input and output, blocked except those we explicitly allow

#### Notes

- As you install other programs, you'll need to enable the necessary ports/applications.

#### References

- https://launchpad.net/ufw

#### Steps

1. Install `ufw`.
    
    On Debian based systems:
    
    ``` bash
    sudo apt install ufw
    ```

1. Deny all outgoing traffic:
    
    ``` bash
    sudo ufw default deny outgoing comment 'deny all outgoing traffic'
    ```
    
    > ```
    > Default outgoing policy changed to 'deny'
    > (be sure to update your rules accordingly)
    > ```
    
    If you are not as paranoid as me, and don't want to deny all outgoing traffic, you can allow it instead:
    
    ``` bash
    sudo ufw default allow outgoing comment 'allow all outgoing traffic'
    ```
   
1. Deny all incoming traffic:
    
    ``` bash
    sudo ufw default deny incoming comment 'deny all incoming traffic'
    ```
    
1. Obviously we want SSH connections in:
    
    ``` bash
    sudo ufw limit in ssh comment 'allow SSH connections in'
    ```
    
    > ```
    > Rules updated
    > Rules updated (v6)
    > ```
    
1. Allow additional traffic as per your needs. Some common use-cases:

    ``` bash
    # allow traffic out on port 53 -- DNS
    sudo ufw allow out 53 comment 'allow DNS calls out'
	
	# allow traffic out on port 123 -- NTP
    sudo ufw allow out 123 comment 'allow NTP out'
    
    # allow traffic out for HTTP, HTTPS, or FTP
    # apt might needs these depending on which sources you're using
    sudo ufw allow out http comment 'allow HTTP traffic out'
    sudo ufw allow out https comment 'allow HTTPS traffic out'
    sudo ufw allow out ftp comment 'allow FTP traffic out'
    
    # allow mail to go out
    sudo ufw allow out 'Mail submission' comment 'allow mail out'
    
    # allow whois
    sudo ufw allow out whois comment 'allow whois'
    
    # allow traffic out on port 68 -- the DHCP client
    # you only need this if you're using DHCP
    sudo ufw allow out 68 comment 'allow the DHCP client to update'
    ```

1. Start `ufw`:
    
    ``` bash
    sudo ufw enable
    ```
    
    > ```
    > Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
    > Firewall is active and enabled on system startup
    > ```

1. If you want to see a status:

    ``` bash
    sudo ufw status
    ```
    
    > ```
    > Status: active
    > 
    > To                         Action      From
    > --                         ------      ----
    > 22/tcp                     LIMIT       Anywhere                   # allow SSH connections in
    > 22/tcp (v6)                LIMIT       Anywhere (v6)              # allow SSH connections in
    > 
    > 53                         ALLOW OUT   Anywhere                   # allow DNS calls out
    > 123                        ALLOW OUT   Anywhere                   # allow NTP out
    > 80/tcp                     ALLOW OUT   Anywhere                   # allow HTTP traffic out
    > 443/tcp                    ALLOW OUT   Anywhere                   # allow HTTPS traffic out
    > 21/tcp                     ALLOW OUT   Anywhere                   # allow FTP traffic out
    > Mail submission            ALLOW OUT   Anywhere                   # allow mail out
    > 43/tcp                     ALLOW OUT   Anywhere                   # allow whois
    > 53 (v6)                    ALLOW OUT   Anywhere (v6)              # allow DNS calls out
    > 123 (v6)                   ALLOW OUT   Anywhere (v6)              # allow NTP out
    > 80/tcp (v6)                ALLOW OUT   Anywhere (v6)              # allow HTTP traffic out
    > 443/tcp (v6)               ALLOW OUT   Anywhere (v6)              # allow HTTPS traffic out
    > 21/tcp (v6)                ALLOW OUT   Anywhere (v6)              # allow FTP traffic out
    > Mail submission (v6)       ALLOW OUT   Anywhere (v6)              # allow mail out
    > 43/tcp (v6)                ALLOW OUT   Anywhere (v6)              # allow whois
    > ```
    
    or
    
    ``` bash
    sudo ufw status verbose
    ```
    
    > ```
    > Status: active
    > Logging: on (low)
    > Default: deny (incoming), deny (outgoing), disabled (routed)
    > New profiles: skip
    > 
    > To                         Action      From
    > --                         ------      ----
    > 22/tcp                     LIMIT IN    Anywhere                   # allow SSH connections in
    > 22/tcp (v6)                LIMIT IN    Anywhere (v6)              # allow SSH connections in
    > 
    > 53                         ALLOW OUT   Anywhere                   # allow DNS calls out
    > 123                        ALLOW OUT   Anywhere                   # allow NTP out
    > 80/tcp                     ALLOW OUT   Anywhere                   # allow HTTP traffic out
    > 443/tcp                    ALLOW OUT   Anywhere                   # allow HTTPS traffic out
    > 21/tcp                     ALLOW OUT   Anywhere                   # allow FTP traffic out
    > 587/tcp (Mail submission)  ALLOW OUT   Anywhere                   # allow mail out
    > 43/tcp                     ALLOW OUT   Anywhere                   # allow whois
    > 53 (v6)                    ALLOW OUT   Anywhere (v6)              # allow DNS calls out
    > 123 (v6)                   ALLOW OUT   Anywhere (v6)              # allow NTP out
    > 80/tcp (v6)                ALLOW OUT   Anywhere (v6)              # allow HTTP traffic out
    > 443/tcp (v6)               ALLOW OUT   Anywhere (v6)              # allow HTTPS traffic out
    > 21/tcp (v6)                ALLOW OUT   Anywhere (v6)              # allow FTP traffic out
    > 587/tcp (Mail submission (v6)) ALLOW OUT   Anywhere (v6)              # allow mail out
    > 43/tcp (v6)                ALLOW OUT   Anywhere (v6)              # allow whois
    > ```

#### Default Applications

`ufw` ships with some default applications. You can see them with:

``` bash
sudo ufw app list
```

> ```
> Available applications:
>   AIM
>   Bonjour
>   CIFS
>   DNS
>   Deluge
>   IMAP
>   IMAPS
>   IPP
>   KTorrent
>   Kerberos Admin
>   Kerberos Full
>   Kerberos KDC
>   Kerberos Password
>   LDAP
>   LDAPS
>   LPD
>   MSN
>   MSN SSL
>   Mail submission
>   NFS
>   OpenSSH
>   POP3
>   POP3S
>   PeopleNearby
>   SMTP
>   SSH
>   Socks
>   Telnet
>   Transmission
>   Transparent Proxy
>   VNC
>   WWW
>   WWW Cache
>   WWW Full
>   WWW Secure
>   XMPP
>   Yahoo
>   qBittorrent
>   svnserve
> ```

To get details about the app, like which ports it includes, type:

``` bash
sudo ufw app info [app name]
```

> ``` bash
> sudo ufw app info DNS
> ```
> 
> ```
> Profile: DNS
> Title: Internet Domain Name Server
> Description: Internet Domain Name Server
> 
> Port:
>   53
> ```

#### Custom Application

If you don't want to create rules by explicitly providing the port number(s), you can create your own application configurations. To do this, create a file in `/etc/ufw/applications.d`.

For example, here is what you would use for [Plex](https://support.plex.tv/articles/201543147-what-network-ports-do-i-need-to-allow-through-my-firewall/):

``` bash
cat /etc/ufw/applications.d/plexmediaserver
```

> ```
> [PlexMediaServer]
> title=Plex Media Server
> description=This opens up PlexMediaServer for http (32400), upnp, and autodiscovery.
> ports=32469/tcp|32413/udp|1900/udp|32400/tcp|32412/udp|32410/udp|32414/udp|32400/udp
> ```

Then you can enable it like any other app:

```bash
sudo ufw allow plexmediaserver
```

([Table of Contents](#table-of-contents))

### PSAD: `iptables` Intrusion Detection And Prevention

#### Why

I can't explain it any better than user [FINESEC](https://serverfault.com/users/143961/finesec) from https://serverfault.com/ did at: https://serverfault.com/a/447604/289829.

> Fail2BAN scans log files of various applications such as apache, ssh or ftp and automatically bans IPs that show the malicious signs such as automated login attempts. PSAD on the other hand scans iptables and ip6tables log messages (typically /var/log/messages) to detect and optionally block scans and other types of suspect traffic such as DDoS or OS fingerprinting attempts. It's ok to use both programs at the same time because they operate on different level.

And, since we're already using [UFW](#ufw-uncomplicated-firewall) so we'll follow the awesome instructions by [netson](https://gist.github.com/netson) at https://gist.github.com/netson/c45b2dc4e835761fbccc to make PSAD work with UFW.

#### How It Works

WIP

#### References

- http://www.cipherdyne.org/psad/
- http://www.cipherdyne.org/psad/docs/config.html
- https://www.thefanclub.co.za/how-to/how-install-psad-intrusion-detection-ubuntu-1204-lts-server
- https://serverfault.com/a/447604/289829
- https://serverfault.com/a/770424/289829
- https://gist.github.com/netson/c45b2dc4e835761fbccc- 

#### Steps

1. Install `psad`.

    On Debian based systems:
    
    ``` bash
    sudo apt install psad
    ```

1. Make a backup of `/etc/psad/psad.conf`:
    
    ``` bash
    sudo cp /etc/psad/psad.conf /etc/psad/psad.conf.$(date +"%Y%m%d%H%M%S")
    ```
    
1. Review and update configuration options in `/etc/psad/psad.conf`. Pay special attention to these:

   |Setting|Set To
   |--|--|
   |[`EMAIL_ADDRESSES`](http://www.cipherdyne.org/psad/docs/config.html#EMAIL_ADDRESSES)|your email address(s)|
   |`HOSTNAME`|your server's hostname|
   |[`ENABLE_AUTO_IDS`](http://www.cipherdyne.org/psad/docs/config.html#ENABLE_AUTO_IDS)|`ENABLE_AUTO_IDS Y;`|
   |`ENABLE_AUTO_IDS_EMAILS`|`ENABLE_AUTO_IDS_EMAILS Y;`|
   |`EXPECT_TCP_OPTIONS`|`EXPECT_TCP_OPTIONS Y;`|
   
   Check the configuration file `psad`'s documentation at http://www.cipherdyne.org/psad/docs/config.html for more details.

1. <a name="psad_step4"></a>Now we need to make some changes to `ufw` so it works with `psad` by telling `ufw` to log all traffic so `psad` can analyze it. Do this by editing **two files** and **adding** these lines **at the end but before the COMMIT line**.
    
    Make backups:
    
    ``` bash
    sudo cp /etc/ufw/before.rules /etc/ufw/before.rules.$(date +"%Y%m%d%H%M%S")
    sudo cp /etc/ufw/before6.rules /etc/ufw/before6.rules.$(date +"%Y%m%d%H%M%S")
    ```
    
    Edit the files:
    
    - `/etc/ufw/before.rules`
    - `/etc/ufw/before6.rules`
    
    And add **add** this **at the end but before the COMMIT line**:
    
    ```
    # log all traffic so psad can analyze
    -A INPUT -j LOG --log-tcp-options --log-prefix "[IPTABLES] "
    -A FORWARD -j LOG --log-tcp-options --log-prefix "[IPTABLES] "
    ```
    
    **Note**: We're adding a log prefix to all the `iptables` logs. We'll need this for [seperating `iptables` logs to their own file](#ns-separate-iptables-log-file).
    
    For example:
    
    > ```
    > ...
    > 
    > # log all traffic so psad can analyze
    > -A INPUT -j LOG --log-tcp-options --log-prefix "[IPTABLES] "
    > -A FORWARD -j LOG --log-tcp-options --log-prefix "[IPTABLES] "
    > 
    > # don't delete the 'COMMIT' line or these rules won't be processed
    > COMMIT
    > ```
    

1. Now we need to reload/restart `ufw` and `psad` for the changes to take effect:

    ``` bash
    sudo ufw reload
    
    sudo psad -R
    sudo psad --sig-update
    sudo psad -H
    ```
    
1. Analyze `iptables` rules for errors:
    
    ``` bash
    sudo psad --fw-analyze
    ```
    
    > ```
    > [+] Parsing INPUT chain rules.
    > [+] Parsing INPUT chain rules.
    > [+] Firewall config looks good.
    > [+] Completed check of firewall ruleset.
    > [+] Results in /var/log/psad/fw_check
    > [+] Exiting.
    > ```
    
    **Note**: If there were any issues you will get an e-mail with the error.
    
1. Check the status of `psad`:
    
    ``` bash
    sudo psad --Status
    ```
    
    > ```
    > [-] psad: pid file /var/run/psad/psadwatchd.pid does not exist for psadwatchd on vm
    > [+] psad_fw_read (pid: 3444)  %CPU: 0.0  %MEM: 2.2
    >     Running since: Sat Feb 16 01:03:09 2019
    > 
    > [+] psad (pid: 3435)  %CPU: 0.2  %MEM: 2.7
    >     Running since: Sat Feb 16 01:03:09 2019
    >     Command line arguments: [none specified]
    >     Alert email address(es): root@localhost
    > 
    > [+] Version: psad v2.4.3
    > 
    > [+] Top 50 signature matches:
    >         [NONE]
    > 
    > [+] Top 25 attackers:
    >         [NONE]
    > 
    > [+] Top 20 scanned ports:
    >         [NONE]
    > 
    > [+] iptables log prefix counters:
    >         [NONE]
    > 
    >     Total protocol packet counters:
    > 
    > [+] IP Status Detail:
    >         [NONE]
    > 
    >     Total scan sources: 0
    >     Total scan destinations: 0
    > 
    > [+] These results are available in: /var/log/psad/status.out
    > ```

([Table of Contents](#table-of-contents))

### Fail2ban: Application Intrusion Detection And Prevention

#### Why

A firewall will board up all the doors and windows you don't want anyone using so nobody can see they are even there. But what about the doors and windows you want visible so approved folks can use them? Even if the door is locked, how do you ensure that someone doesn't try to force their way in?

That is where **Fail2ban** comes in. It will monitor network traffic/logs and prevent intrusions by blocking suspicious activity (e.g. multiple successive failed connections in a short time-span).

#### How It Works

WIP

#### Goal

- network monitoring for suspicious activity with automatic banning of offending IPs

#### Notes

- As of right now, the only thing running on this server is SSH so we'll want Fail2ban to monitor SSH and ban as necessary.
- As you install other programs, you'll need to create/configure the appropriate jails and enable them.

#### References

- https://www.fail2ban.org/
- https://blog.vigilcode.com/2011/05/ufw-with-fail2ban-quick-secure-setup-part-ii/
- https://dodwell.us/security/ufw-fail2ban-portscan.html
- https://www.howtoforge.com/community/threads/fail2ban-and-ufw-on-debian.77261/

#### Steps

1. Install `fail2ban`.
    
    On Debian based systems:
    
    ``` bash
    sudo apt install fail2ban
    ```
    
1. We don't want to edit `/etc/fail2ban/fail2ban.conf` or `/etc/fail2ban/jail.conf` because a future update may overwrite those so we'll update a local copy instead. **Add** this to `/etc/fail2ban/jail.local` after replacing `[LAN SEGMENT]` and `[your email]` with the appropriate values:
    
    ```
    [DEFAULT]
    # the IP address range we want to ignore
    ignoreip = 127.0.0.1/8 [LAN SEGMENT]

    # who to send e-mail to
    destemail = [your e-mail]

    # who is the email from
    sender = [your e-mail]

    # since we're using exim4 to send emails
    mta = mail

    # get email alerts
    action = %(action_mwl)s
    ```
    
    **Note**: Your server will need to be able to send e-mails so Fail2ban can let you know of suspicious activity and when it banned an IP.

1. We need to create a jail for `ssh` that tells `fail2ban` to look at `ssh` logs and use `ufw` to ban/unban IPs as needed. Create a jail for `ssh` by **adding** this to `/etc/fail2ban/jail.d/ssh.local`:

    ```
    [sshd]
    enabled = true
    banaction = ufw
    port = ssh
    filter = sshd
    logpath = %(sshd_log)s
    maxretry = 5
    ```
    
1. In the above we tell `fail2ban` to use the `ufw` as the `banaction`. Fail2ban ships with an action configuration file for `ufw`. You can see it in `/etc/fail2ban/action.d/ufw.conf`
    
1. Enable `fail2ban` and the jail for SSH:

    ``` bash
    sudo fail2ban-client start
    sudo fail2ban-client reload
    sudo fail2ban-client add sshd
    ```

1. To check the status:

    ``` bash
    sudo fail2ban-client status
    ```

    > ```
    > Status
    > |- Number of jail:      1
    > `- Jail list:   sshd
    > ```
    
    ``` bash
    sudo fail2ban-client status sshd
    ```
    
    > ```
    > Status for the jail: sshd
    > |- Filter
    > |  |- Currently failed: 0
    > |  |- Total failed:     0
    > |  `- File list:        /var/log/auth.log
    > `- Actions
    >    |- Currently banned: 0
    >    |- Total banned:     0
    >    `- Banned IP list:
    > ```

#### Custom Jails

I have not needed to create a custom jail yet. Once I do, and I figure out how, I will update this guide. Or, if you know how please help [contribute](#contributing).

#### Unban an IP

To unban an IP use this command:

``` bash
fail2ban-client set [jail] unbanip [IP]
```

`[jail]` is the name of the jail that has the banned IP and `[IP]` is the IP address you want to unban. For example, to unaban `192.168.1.100` from SSH you would do:

``` bash
fail2ban-client set sshd unbanip 192.168.1.100
```

([Table of Contents](#table-of-contents))

## The Danger Zone

### Proceed At Your Own Risk

This sections cover things that are high risk because there is a possibility they can make your system unusable, or are considered unnecessary by many because the risks outweigh any rewards.

**!! PROCEED AT YOUR OWN RISK !!**

<details><summary>!! PROCEED AT YOUR OWN RISK !!</summary>

### Linux Kernel `sysctl` Hardening

<details><summary>!! PROCEED AT YOUR OWN RISK !!</summary>

#### Why

The kernel is the brains of a Linux system. Securing it just makes sense. 

#### Why Not

Changing kernel settings with `sysctl` is risky and could break your server. If you don't know what you are doing, don't have the time to debug issues, or just don't want to take the risks, I would advise from not following these steps.

#### How It Works

WIP

#### Disclaimer

I am not as knowledgeable about hardening/securing a Linux kernel as I'd like. As much as I hate to admit it, I do not know what all of these settings do. My understanding is that most of them are general kernel hardening and performance, and the others are to protect against spoofing and DOS attacks. 

In fact, since I am not 100% sure exactly what each setting does, I took recommended settings from numerous sites (all linked [below](#sysctl-references)) and combined them to figure out what should be set. I figure if multiple reputable sites mention the same setting, it's probably safe.

If you have a better understanding of what these settings do, or have any other feedback/advice on them, please [let me know](#contacting-me).

I won't provide [For the lazy](#editing-configuration-files---for-the-lazy) code in this section.

#### Notes

- Documentation on all the `sysctl` settings/keys is severely lacking. The [documentation I can find](https://github.com/torvalds/linux/tree/master/Documentation) seems to reference the 2.2 version kernel. I could not find anything newer. If you know where I can, please [let me know](#contacting-me).
- The reference sites listed [below](#sysctl-references) have more comments on what each setting does.

#### <a name="sysctl-references"></a>References

- https://github.com/torvalds/linux/tree/master/Documentation
- https://www.cyberciti.biz/faq/linux-kernel-etcsysctl-conf-security-hardening/
- https://geektnt.com/sysctl-conf-hardening.html
- https://linoxide.com/how-tos/linux-server-protection/
- https://github.com/klaver/sysctl/blob/master/sysctl.conf
- https://cloudpro.zone/index.php/2018/01/30/debian-9-3-server-setup-guide-part-5/ 

#### Steps

1. The `sysctl` settings can be found in the [linux-kernel-sysctl-hardening.md](https://github.com/imthenachoman/How-To-Secure-A-Linux-Server/blob/master/linux-kernel-sysctl-hardening.md) file in this repo.

1. Before you make a kernel `sysctl` change permanent, you can test it with the `sysctl` command:
    
    ``` bash
    sudo sysctl -w [key=value]
    ```
    
    Example:
    
    ``` bash
    sudo sysctl -w kernel.ctrl-alt-del=0
    ```
    
    **Note**: There are no spaces in `key=value`, including before and after the space.

1. Once you have tested a setting, and made sure it works without breaking your server, you can make it permanent by adding the values to `/etc/sysctl.conf`. For example:

    ``` bash
    $ sudo cat /etc/sysctl.conf
    kernel.ctrl-alt-del = 0
    fs.file-max = 65535
    ...
    kernel.sysrq = 0
    ```

1. After updating the file you can reload the settings or reboot. To reload:

    ``` bash
    sudo sysctl -p
    ```
    
**Note**: If `sysctl` has trouble writing any settings then `sysctl -w` or `sysctl -p` will write an error to `stderr`. You can use this to quickly find invalid settings in  your `/etc/sysctl.conf` file:

``` bash
sudo sysctl -p >/dev/null
```

</details><br />

([Table of Contents](#table-of-contents))

### Password Protect GRUB

<details><summary>!! PROCEED AT YOUR OWN RISK !!</summary>

#### Why

If a bad actor has physical access to your server, they could use GRUB to gain unauthorized access to your system.

#### Why Not

If you forget the password, you'll have to go through [some work](https://www.cyberciti.biz/tips/howto-recovering-grub-boot-loader-password.html) to recover the password.

#### How It Works

WIP

#### Goals

- auto boot the default Debian install and require a password for anything else

#### Notes

- This will only protect GRUB and anything behind it like your operating systems. Check your motherboard's documentation for password protecting your BIOS to prevent a bad actor from circumventing GRUB.

#### References

- https://selivan.github.io/2017/12/21/grub2-password-for-all-but-default-menu-entries.html
- https://help.ubuntu.com/community/Grub2/Passwords
- https://computingforgeeks.com/how-to-protect-grub-with-password-on-debian-ubuntu-and-kali-linux/
- `man grub`
- `man grub-mkpasswd-pbkdf2`

#### Steps

1. Create a [Password-Based Key Derivation Function 2 (PBKDF2)](https://en.wikipedia.org/wiki/PBKDF2) hash of your password:

    ``` bash
    grub-mkpasswd-pbkdf2 -c 100000
    ```

   The below output is from using `password` as the password:

    > ```
    > Enter password:
    > Reenter password:
    > PBKDF2 hash of your password is grub.pbkdf2.sha512.100000.2812C233DFC899EFC3D5991D8CA74068C99D6D786A54F603E9A1EFE7BAEDDB6AA89672F92589FAF98DB9364143E7A1156C9936328971A02A483A84C3D028C4FF.C255442F9C98E1F3C500C373FE195DCF16C56EEBDC55ABDD332DD36A92865FA8FC4C90433757D743776AB186BD3AE5580F63EF445472CC1D151FA03906D08A6D
    > ```

1. Copy everything **after** `PBKDF2 hash of your password is `, **starting from and including** `grub.pbkdf2.sha512...` to the end. You'll need this in the next step.

1. The `update-grub` program uses scripts to generate configuration files it will use for GRUB's settings.  Create the file `/etc/grub.d/01_password` and **add** the below code after replacing `[hash]` with the hash you copied from the first step. This tells `update-grub` to use this username and password for GRUB.

    ``` bash
    #!/bin/sh
    set -e
    
    cat << EOF
    set superusers="grub"
    password_pbkdf2 grub [hash]
    EOF
    ```
    
    For example:
    
    > ``` bash
    > #!/bin/sh
    > set -e
    > 
    > cat << EOF
    > set superusers="grub"
    > password_pbkdf2 grub grub.pbkdf2.sha512.100000.2812C233DFC899EFC3D5991D8CA74068C99D6D786A54F603E9A1EFE7BAEDDB6AA89672F92589FAF98DB9364143E7A1156C9936328971A02A483A84C3D028C4FF.C255442F9C98E1F3C500C373FE195DCF16C56EEBDC55ABDD332DD36A92865FA8FC4C90433757D743776AB186BD3AE5580F63EF445472CC1D151FA03906D08A6D
    > EOF
    > ```

1. Set the file's execute bit so `update-grub` includes it when it updates GRUB's configuration:

   ``` bash
   sudo chmod a+x /etc/grub.d/01_password
   ```

1. Make a backup of `/etc/grub.d/10_linux` and unset execute bit so `update-grub` doesn't try to run it:

    ``` bash
    sudo cp --preserve /etc/grub.d/10_linux /etc/grub.d/10_linux.$(date +"%Y%m%d%H%M%S")
    sudo chmod a-x /etc/grub.d/10_linux.*
    ```

1. To make the default Debian install unrestricted (**without** the password) while keeping everything else restricted (**with** the password) modify `/etc/grub.d/10_linux` and **add** `--unrestricted` to the `CLASS` variable.
 
    [For the lazy](#editing-configuration-files---for-the-lazy):
    
    ``` bash
    sudo sed -i -r -e "/^CLASS=/ a CLASS=\"\${CLASS} --unrestricted\"         # added by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")" /etc/grub.d/10_linux
    ```

1. Update GRUB with `update-grub`:

    ``` bash
    sudo update-grub
    ```

</details><br />

([Table of Contents](#table-of-contents))

### Disable Root Login

<details><summary>!! PROCEED AT YOUR OWN RISK !!</summary>

#### Why

If you have `sudo` [configured properly](#limit-who-can-use-sudo), then the **root** account will mostly never need to log in directly -- either at the terminal or remotely.

#### Why Not

**Be warned, this can cause issues with some configurations!**

If your installation uses [`sulogin`](https://linux.die.net/man/8/sulogin) (like Debian) to drop to a **root** console during boot failures, then locking the **root** account will prevent `sulogin` from opening the **root** shell and you will get this error:

    Cannot open access to console, the root account is locked.
    
    See sulogin(8) man page for more details.
    
    Press Enter to continue.

To work around this, you can use the `--force` option for `sulogin`. Some distributions already include this, or some other, workaround.

An alternative to locking the **root** acount is set a long/complicated **root** password and store it in a secured, non digital format. That way you have it when/if you need it.

#### How It Works

WIP

#### Goal

- locked **root** account that nobody can use to log in as **root**

#### Notes

- Some distributions disable **root** login by default (e.g. Ubuntu) so you may not need to do this step. Check with your distribution's documentation.

#### References

- https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=806852
- https://github.com/systemd/systemd/issues/7115
- https://github.com/karelzak/util-linux/commit/7ff1162e67164cb4ece19dd809c26272461aa254
- https://github.com/systemd/systemd/issues/11596
- `man systemd`

#### Steps

1. Lock the **root** account:

    ``` bash
    sudo passwd -l root
    ```

</details><br />

([Table of Contents](#table-of-contents))

### Change Default `umask`

<details><summary>!! PROCEED AT YOUR OWN RISK !!</summary>

#### Why

`umask` controls the **default** permissions of files/folders when they are created. Insecure file/folder permissions give other accounts potentially unauthorized access to your data. This may include the ability to make configuration changes.

- For **non-root** accounts, there is no need for other accounts to get any access to the account's files/folders **by default**.
- For the **root** account, there is no need for the file/folder primary group or other accounts to have any access to **root**'s files/folders **by default**.

When and if other accounts need access to a file/folder, you want to explicitly grant it using a combination of file/folder permissions and primary group.

#### <a name="umask-root"></a>Why Not

Changing the default `umask` can create unexpected problems. For example, if you set `umask` to `0077` for **root**, then **non-root** accounts **will not** have access to application configuration files/folders in `/etc/` which could break applications that do not run with **root** privileges.

#### How It Works

WIP

#### Goals

- set default `umask` for **non-root** accounts to **0027**
- set default `umask` for the **root** account to **0077**

#### Notes

- `umask` is a Bash built-in which means a user can change their own `umask` setting. 

#### References

- https://www.linuxnix.com/umask-define-linuxunix/
- https://serverfault.com/questions/818783/which-umask-is-more-secure-in-linux-022-or-027
- https://www.cyberciti.biz/tips/understanding-linux-unix-umask-value-usage.html
- `man umask`

#### Steps

1. Set default `umask` for **non-root** accounts to **0027** by **adding** this line to `/etc/profile` and `/etc/bash.bashrc`:
    
    ```
    umask 0027
    ```
    
    [For the lazy](#editing-configuration-files---for-the-lazy):
    
    ``` bash
    sudo cp --preserve /etc/profile /etc/profile.$(date +"%Y%m%d%H%M%S")
    sudo cp --preserve /etc/bash.bashrc /etc/bash.bashrc.$(date +"%Y%m%d%H%M%S")
    
    echo -e "\numask 0027         # added by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")" | sudo tee -a /etc/profile /etc/bash.bashrc
    ```

1. We also need to **add** this line to `/etc/login.defs`:
    
    ```
    UMASK 0027
    ```
    
    [For the lazy](#editing-configuration-files---for-the-lazy):
    
    ``` bash
    sudo cp --preserve /etc/login.defs /etc/login.defs.$(date +"%Y%m%d%H%M%S")
    
    echo -e "\nUMASK 0027         # added by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")" | sudo tee -a /etc/login.defs 
    ```

1. Set default `umask` for the **root** account to **0077** by **adding** this line to `/root/.bashrc`:
    
    ```
    umask 0077
    ```
    
    [For the lazy](#editing-configuration-files---for-the-lazy):
    
    ``` bash
    sudo cp --preserve /root/.bashrc /root/.bashrc.$(date +"%Y%m%d%H%M%S")
    
    echo -e "\numask 0077         # added by $(whoami) on $(date +"%Y-%m-%d @ %H:%M:%S")" | sudo tee -a /root/.bashrc
    ```

</details><br />

([Table of Contents](#table-of-contents))

### Orphaned Software

<details><summary>!! PROCEED AT YOUR OWN RISK !!</summary>

#### Why

As you use your system, and you install and uninstall software, you'll eventually end up with orphaned, or unused software/packages/libraries. You don't need to remove them, but if you don't need them, why keep them? When security is a priority, anything not explicitly needed is a potential security threat. You want to keep your server as trimmed and lean as possible.

#### Notes

- Each distribution manages software/packages/libraries differently so how you find and remove orphaned packages will be different. So far I only have steps for Debian based systems.

#### Debian Based Systems

##### <a name="orphaned-software-why-not"></a>Why Not

Keep in mind, `deborphan` finds packages that have **no package dependencies**. That does not mean they are not used. You could very well have a package you use every day that has no dependencies that you wouldn't want to remove. And, if `deborphan` gets anything wrong, then removing critical packages may break your system.

##### Steps

On Debian based systems, you can use [`deborphan`](http://freshmeat.sourceforge.net/projects/deborphan/) to find orphaned packages. 
 
1. Install `deborphan`.
       
    ``` bash
    sudo apt install deborphan
    ```
       
1. Run `deborphan` as **root** to see a list of orphaned packages:

    ``` bash
    sudo deborphan
    ```
    
    > ```
    > libxapian30
    > libpipeline1
    > ```
       
1. [Assuming you want to remove all of the packages `deborphan` finds](#orphaned-software-why-not), you can pass it's output to `apt` to remove them:
       
    ``` bash
    sudo apt --autoremove purge $(deborphan)
    ```

</details>

</details><br />

([Table of Contents](#table-of-contents))

## The Auditing

### Lynis - Linux Security Auditing

#### Why

From [https://cisofy.com/lynis/](https://cisofy.com/lynis/):

> Lynis is a battle-tested security tool for systems running Linux, macOS, or Unix-based operating system. It performs an extensive health scan of your systems to support system hardening and compliance testing.

#### How It Works

WIP

#### Goals

- Lynis installed

#### Notes

- CISOFY offers packages for many distributions. Check https://packages.cisofy.com/ for distribution specific installation instructions. 

#### References

- https://cisofy.com/documentation/lynis/get-started/
- https://packages.cisofy.com/community/#debian-ubuntu
- https://thelinuxcode.com/audit-lynis-ubuntu-server/
- https://www.vultr.com/docs/install-lynis-on-debian-8

#### Steps

1. Install `lynis`. https://cisofy.com/lynis/#installation has detailed instructions on how to install it for your distribution.
    
    On Debian based systems, using CISOFY's community software repository:
    
    ``` bash
    sudo wget -O - https://packages.cisofy.com/keys/cisofy-software-public.key | sudo apt-key add -
    sudo apt install apt-transport-https
    sudo echo "deb https://packages.cisofy.com/community/lynis/deb/ stable main" | sudo tee /etc/apt/sources.list.d/cisofy-lynis.list
    sudo apt update
    sudo apt install lynis
    ```

1. Update it:

    ``` bash
    sudo lynis update info
    ```

1. Run a security audit:

    ``` bash
    sudo lynis audit system
    ```
    
    This will scan your server, report its audit findings, and at the end it will give you suggestions. Spend some time going through the output and address gaps as necessary.

([Table of Contents](#table-of-contents))

## The Miscellaneous

### Configure Gmail as MTA

#### Why

Unless you're planning on setting up your own mail server, you'll need a way to send e-mails from your server. This will be important for system alerts/messages.

You can use any Gmail account but I recommend you create one specific for this server. That way if your server **is** compromised, the bad-actor won't have any passwords for your primary account. Granted, if you have 2FA/MFA enabled and you use an app password, there isn't much a bad-actor can do with just the app password but why take the risk?

#### How It Works

WIP

#### Goals

- `mail` configured to send e-mails from your server using [Gmail](https://mail.google.com/)

#### References

- https://php.quicoto.com/setup-exim4-to-use-gmail-in-ubuntu/

#### Steps

1. Install `exim4`.

    On Debian based systems:
    
    ``` bash
    sudo apt install exim4
    ```

1. Configure `exim4`:
    
    For Debian based systems:
    ``` bash
    sudo dpkg-reconfigure exim4-config
    ```
    
    You'll be prompted with some questions:

    |Prompt|Answer|
    |--:|--|
    |General type of mail configuration|`mail sent by smarthost; no local mail`|
    |System mail name|(default)|
    |IP-addresses to listen on for incoming SMTP connections|`127.0.0.1`|
    |Other destinations for which mail is accepted|(default)|
    |Visible domain name for local users|(default)|
    |IP address or host name of the outgoing smarthost|`smtp.gmail.com::587`|
    |Keep number of DNS-queries minimal (Dial-on-Demand)?|`No`|
    |Split configuration into small files?|`No`|
    
1. Make a backup of `/etc/exim4/passwd.client`:
    
    ``` bash
        sudo cp /etc/exim4/passwd.client /etc/exim4/passwd.client.$(date +"%Y%m%d%H%M%S")
    ```
    
1. **Add** a line like this to `/etc/exim4/passwd.client`
    
    ```
    *.google.com:yourAccount@gmail.com:yourPassword
    ```
    
    Replace `yourAccount@gmail.com` and `yourPassword` with your details. If you have 2FA/MFA enabled on your Gmail then you'll need to create and use an app password.

1. This file has your Gmail password so we need to lock it down:

    ``` bash
    sudo chown root:Debian-exim /etc/exim4/passwd.client
    sudo chmod 640 /etc/exim4/passwd.client
    ```

1. Restart `exim4`:
    
    ``` bash
    sudo service exim4 restart
    ```
    
1. Add some mail aliases so we can send e-mails to local accounts by **adding** lines like this to `/etc/aliases`:
    
    ```
    user1: user1@gmail.com
    user2: user2@gmail.com
    ...
    ```
    
    You'll need to add all the local accounts that exist on your server.

([Table of Contents](#table-of-contents))

### Separate `iptables` Log File

#### Why

There will come a time when you'll need to look through your `iptables` logs. Having all the `iptables` logs go to their own file will make it a lot easier to find what you're looking for.

#### How It Works

WIP

#### References

- https://blog.shadypixel.com/log-iptables-messages-to-a-separate-file-with-rsyslog/
- https://gist.github.com/netson/c45b2dc4e835761fbccc
- https://www.rsyslog.com/doc/v8-stable/configuration/actions.html

#### Steps

1. The first step is by telling your firewall to prefix all log entries with some unique string. If you're using `iptables` directly, you would do something like `--log-prefix "[IPTABLES] "` for all the rules. We took care of this in step [step 4 of installing `psad`](#psad_step4).

1. After you've added a prefix to the firewall logs, we need to tell `rsyslog` to send those lines to its own file. Do this by **creating** the file `/etc/rsyslog.d/10-iptables.conf` and **adding** this:

    ```
    :msg, contains, "[IPTABLES] " /var/log/iptables.log
    & stop
    ```
    
    If you're expecting a lot if data being logged by your firewall, prefix the filename with a `-` ["to omit syncing the file after every logging"](https://www.rsyslog.com/doc/v8-stable/configuration/actions.html#regular-file). For example:

    ```
    :msg, contains, "[IPTABLES] " -/var/log/iptables.log
    & stop
    ```
    
    **Note**: Remember to change the prefix to whatever you use.

1. Since we're logging firewall messages to a different file, we need to tell `psad` where the new file is. Edit `/etc/psad/psad.conf` and set `IPT_SYSLOG_FILE` to the path of the log file. For example:

    ```
    IPT_SYSLOG_FILE /var/log/iptables.log;
    ```

1. Restart `psad` and `rsyslog` to activate the changes (or reboot):

    ``` bash
    sudo psad -R
    sudo psad --sig-update
    sudo psad -H
    sudo service rsyslog restart
    ```

1. The last thing we have to do is tell `logrotate` to rotate the new log file so it doesn't get to big and fill up our disk. **Create** the file `/etc/logrotate.d/iptables` and **add** this:

    ```
    /var/log/iptables.log
    {
        rotate 7
        daily
        missingok
        notifempty
        delaycompress
        compress
        postrotate
            invoke-rc.d rsyslog rotate > /dev/null
        endscript
    }
    ```
   
([Table of Contents](#table-of-contents))

## Left Over

### Contacting Me

For any questions, comments, concerns, feedback, or issues, submit a [new issue](https://github.com/imthenachoman/How-To-Secure-A-Linux-Server/issues/new).

([Table of Contents](#table-of-contents))

### Additional References

- [https://github.com/pratiktri/server_init_harden](https://github.com/pratiktri/server_init_harden) - Bash script that automates few of the tasks that you need to perform on a new Linux server to give it basic amount security.
- https://security.utexas.edu/os-hardening-checklist/linux-7 -  Red Hat Enterprise Linux 7 Hardening Checklist
- https://cloudpro.zone/index.php/2018/01/18/debian-9-3-server-setup-guide-part-1/ - # Debian 9.3 server setup guide
- https://blog.vigilcode.com/2011/04/ubuntu-server-initial-security-quick-secure-setup-part-i/ - Ubuntu Server Initial Security guide

([Table of Contents](#table-of-contents))

### Acknowledgments

- [/r/linuxquestions](https://www.reddit.com/r/linuxquestions/comments/aopzl7/new_guide_created_by_me_how_to_secure_a_linux/)
- [/r/selfhosted](https://www.reddit.com/r/selfhosted/comments/aoxd4l/new_guide_created_by_me_how_to_secure_a_linux/)
- [Hacker News](https://news.ycombinator.com/item?id=19177435#19178618)

([Table of Contents](#table-of-contents))

### Disclaimer / Warranty

This guide comes with ABSOLUTELY NO WARRANTY. Use with caution. I take no responsibility for anything, related to or not related to this guide. 
 
([Table of Contents](#table-of-contents))
