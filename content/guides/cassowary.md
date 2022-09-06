---
title: "Seamless Windows Applications in Linux"
description: Guide on using Cassowary with a headless Windows VM to allow for Windows applications to integrate seamlessly in Linux.
slug: cassowary
date: 2022-09-05T21:38:37-07:00
draft: false
toc: true
images:
tags: ['Cassowary','Windows','VM','Virtual Machine','RDP','Linux']
type:
---

## Introduction

One frustration that every Linux user has experienced is when you need to run a particular program that is only available for Windows.  

While for the overwhelming majority of applications there are equivalent solutions in Linux, sometimes the native Windows application is just what you need to use.  

Whether it is because your professional workflow requires you to use it or the equivalent solutions are just not nearly as powerful as the original (looking at you Excel), there are times where your use case demands the Windows program.  

While traditionally, you would setup and use a Windows Virtual Machine (VM), that comes with its own set of hassles and most importantly does not integrate well with a Linux workflow.

## Cassowary

This is where [**Cassowary**](https://github.com/casualsnek/cassowary) comes in, it is a program developed by [Casualsnek](https://github.com/casualsnek) which allows you to run Windows applications as if they were native in Linux, thanks to some clever use of RDP.  

Cassowary does not eliminate the need for a Windows VM, if you are running Windows applications, you will need to run them on Windows some way or another, however, what it does allow you is to have the Windows programs seamlessly integrate in your Linux workflow.  
Additionally, there is no to very little lag, resulting in a truly seamless workflow.  

For example below, you can see I have two Excel spreadsheets alongside the Kitty terminal emulator.
![Cassowary demo](/guides/cassowary/cassowary-demo.png)

## Contraints

One of the primary contraints of Cassowary is that while you can run multiple instances of the same application, you can only run one type of application at a time.  
For example, you can have two Excel instances open however, you cannot have both a Word and Excel instance open.  

Additionally, some applications may experience minor issues if you use tiling window managers such as i3. However, this is almost always resolved by placing the application in full screen to perform that particular action (e.g. resizing columns in Excel).

Finally, the first time since you started your Windows VM that you start an application via Cassowary, a small black box may appear. Close it and relaunch your desired application and the first time, you may have to wait 15-20 seconds as it establishes the connection to RDP and authenticates.  
After that, you should be fine and see applications launch quasi-instantly.

## Prerequisites

Prior to continuing, ensure that you have a Windows virtual machine setup.   
There are no restrictions regarding the hypervisor however, this guide will only cover VMware and Virtual Box.  

## Installing Cassowary

**Follow the official installation guide __until__ you reach the "Launch cassowary configuration utility" step, then return here.**  

Official installation guide: [https://github.com/casualsnek/cassowary/blob/main/docs/2-cassowary-install.md](https://github.com/casualsnek/cassowary/blob/main/docs/2-cassowary-install.md)


### Libvirt

Firstly, ensure that you have the Libvirt daemon (`libvirtd`) enabled and running otherwise you might encounter issues when starting the Cassowary configuration utility.

Enable: `sudo systemctl enable libvirtd;`  
Start: `sudo systemctl start libvirtd;`


### Enable RDP in Windows VM

Since Cassowary uses RDP to display application windows in Linux, you need to ensure the following:
- RDP is enabled in your Windows settings.
- Port `3389` for RDP is open to inbound connections (you can simply use the Windows Firewall RDP rule).
- The desired user is allowed to connect via RDP by adding it in the RDP 'allowed users' setting.


**From here on, please follow the instructions specific to the hypervisor you are using.**


### KVM

If you are using KVM, you are lucky and have nothing else to do!  

Return to the guide and launch the Cassowary configuration utility.  
You can now configure Cassowary and add application shortcuts.


### VMware

Firstly, ensure that the networking mode of the VM is set to `NAT`.  

Additionally, find the IP of your Windows VM by entering `ipconfig` in the command prompt and note down the IPv4 address returned.  

#### Port Forward

To connect to your VM via RDP, you need to setup a port forward in your NAT.

1. Open the VMware Virtual Networks Editor by either selecting it under the 'Edit' tab or by launching it manually by executing `sudo /usr/lib/vmware/bin/vmware-netcfg;`.
2. Select the NAT network (if multiple, select the one to whicht he VM is connected to), and then click on 'NAT' settings on the right.
3. Under port forwarding, add a new rule, set the VM IP to be the IPv4 of your VM which you noted down earlier, and set the port values to `3389`.

See the image below for reference:

![VMware net cfg](/guides/cassowary/vmware-netcfg-cassowary.png)

Save and then you can return to the official installation guide and launch the Cassowary configuration utility.


### Virtual Box

Firstly, ensure that the networking mode of the VM is set to `NAT`.  

Additionally, find the IP of your Windows VM by entering `ipconfig` in the command prompt and note down the IPv4 address returned.  

#### Port Forward

To connect to your VM via RDP, you need to setup a port forward in your NAT.

1. In Virtual Box, open the settings of your Windows VM and click on 'Network'.
2. Ensure that the adapter is enabled and attached to NAT.
3. Click the 'Advanced' drop down.
4. Select 'Port Forwarding' and then create a new rule. Set the protocol to TCP, the guest IP to the IPv4 of your VM which you noted down earlier and the port to 3389. You can leave the IP columns blank.

See the image below for reference:

![Virtual Box network config](/guides/cassowary/virtualbox-cassowary.png)

Save and then you can return to the official installation guide and launch the Cassowary configuration utility.

## Post Installation

Once you have Cassowary configured, simply add your desired applications to your application menu.

Whenever you want to use it, all you need to do it is start your Windows VM and let it run in the background.

Enjoy your seamless Linux-Windows workflow and I hope this guide was useful!
