---
title: "Install and run dnsmasq on Mac OS X Yosemite 10.10.4"
date: 2015-08-08
categories: dnsmasq,setup,devops
template: article.jade
author: marcoleong
---

# Preface

Good evening (at the time I'm writing this article)! Haven't come back into this blog for long time. I guess this is not a very popular blog. BUT if you happen to be one of the visitor who come across this article. Please drop a comment if you have any suggestion or question about the article you are reading. Or it could be just a chat with me :)

Tonight we are going to go through some necessary steps to install [dnsmasq](http://www.thekelleys.org.uk/dnsmasq/doc.html)

# What is dnsmasq? and why?

*you can skip this part if you know what it is*

It is a software that provide functionality of [DNS](https://en.wikipedia.org/wiki/Domain_Name_System) and [DHCP](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol) without complex setup procedure and configuration. This software is great tools for DevOps and Web Developer to easily setup test/dev environment for themselves or testing team.

**NOTE: This tool is great for test/dev, but not for production enviroment.**

If you are looking for software to use in production, please take a look at:

* DNS: [BIND](https://en.wikipedia.org/wiki/BIND)
* DHCP: it is built-in to many linux distro. I'm not going to go through that in this post, but if you want more information. Please comment below.

# Prerequisite

* Like to use command line
* [Homebrew](http://brew.sh/) - best package manager for Mac OS X (It should be trivial to install, follow the instruction on their homepage.)

*PS: Developer of Homebrew (@mxcl) posted this on Twitter [Google: 90% of our engineers use the software you wrote (Homebre...](https://twitter.com/mxcl/status/608682016205344768?lang=en)*

# Let's begin...

Install dnsmasq with Homebrew:

```bash
brew install dnsmasq
```

As instructed after this commmand finish

```bash
# Copy example config and becareful this will override config of dnsmasq if you did install it before
cp /usr/local/opt/dnsmasq/dnsmasq.conf.example /usr/local/etc/dnsmasq.conf

# Copy the .plist file to Mac OS X On-boot launch directory
sudo cp -fv /usr/local/opt/dnsmasq/*.plist /Library/LaunchDaemons
sudo chown root /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
```

# Configuration

There are many options available in dnsmasq. I'm not going through every single of it for now. I will just show you what is the most basic setup to use DNS functionality.

Open `/usr/local/etc/dnsmasq.conf` with you favorite editor

Add the following,

```ini
user=root
group=wheel
```

*This will force dnsmasq to run as root user. It's because it needs to bind to a privileged port 1~1024 used by DNS protocol (port: 53). If you want to know what privileged port is, go [here](http://www.w3.org/Daemon/User/Installation/PrivilegedPorts.html)*

Then add a new address record

```ini
address=/project-jamtube.local/127.0.0.1
```

There is another line that you would like to add is extra `resolv.conf`.
It is a file that define which DNS server your machine is using. By default, dnsmasq will use resolv.conf at path `/etc/resolv.conf`. However, this conf file is also managed by Mac OS. This means if dnsmasq is using what Mac OS X resolv is using. Then it will try to resolve *external* DNS record from itself. So we would like to create one here

```bash
sudo touch /etc/resolv-dnsmasq.conf
```

Edit this file and add the follow line

```ini
# This is Google public DNS server, or it could be your own DNS server
nameserver 8.8.8.8
```

Finally, here is the complete dnsmasq.conf

```ini
user=root
group=wheel

address=/project-jamtube.local/127.0.0.1

resolv-file=/etc/resolv-dnsmasq.conf
```

# Start it up !

On Mac OS, most of the daeman are managed by launchctl. It's a tool that take care of the startup process of a daemon. Inorder to:

Start the daemon

```bash
sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
```

Stop it

```bash
sudo launchctl unload /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
```

You would like to have some alias setup to reload after you change the config.
Edit your ~/.bash_profile
 
```bash
alias reload-dnsmasq="sudo launchctl unload /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist && sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist"
```

# At last, point DNS server to dnsmasq

Go to **Network** preference:

![Go to network preference](/articles/dnsmasq-install-and-run-on-mac-os-x-yosemite-10-10-4/network.jpg)

Change **DNS** to your local ip

![Change DNS Entry](/articles/dnsmasq-install-and-run-on-mac-os-x-yosemite-10-10-4/dns.jpg)


# I use it daily !

This tool I use it every day for development and also use to setup testing for other users. It's easy to setup and configure. You should give it a try !