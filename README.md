# ApacheServer
Create a secure and private server using Apache HTTP server

## Table of Content
* [Prerequisites](#prerequisites)
* [Software Requirements](#software-requirements)
* [Step by step Guide](#step-by-step-guide)
   * [Install CentOS, Ubuntu or Fedora on Virtual Box](#Install-CentOS-Ubuntu-or-Fedora-on-Virtual-Box)
   * [Enable Virtualisation](#Enable-Virtualisation)
   * [Set up Operating System on Virtual Box](#Set-up-Operating-System-on-Virtual-Box)
   * [Configure internet access on Operating System](#configure-internet-access-on-operating-system)
   * [Change Hostname](#change-hostname)

### Prerequisites
* Any Virtual Machine from Oracle VM VirtualBox
* Basic Linux bash script
* Basic Powershell bash script

#### Software Requirements
* Oracle VM VirtualBox
* CentOS 8 VM
* Power Shell
* Pip
* Pipenv
* Apache HTTP
* Mod_wsgi
* PuTTY

### Step by Step Guide

#### Install CentOS, Ubuntu or Fedora on Virtual Box 
* If you do not own a spare computer, you will have to install VirtualBox, and run the OS virtually on your host computer (Windows/Mac).  
1. Search for Oracle VM VirtualBox and download the latest version of VB.

#### Enable Virtualisation
* Since you are trying to set up a remote computer/Virtual Machine (VM) on your host computer, you first must enable virtualisation on your host.
* This step is IMPORTANT, if virtualisation setting is left disabled, you will not be able to run a VM. 
1. Reboot your machine
2. Enter BIOS by pressing the hotkey. The hotkeys may differ depending on the brand of the computer that you are using. Usually F1, F2, F3, F10, Esc or Delete keys are the hotkeys to enter BIOS
3. Navigate to 'Advanced' and look for 'Intel Virtualisation Technology'
4. Ensure that it is 'ENABLED'
5. Save current settings and exit BIOS

#### Set up Operating System on Virtual Box
1. Once Oracle VM VirtualBox has been downloaded, you will have to install a disk of your preferred Operating System (OS)
2. For instance, if your desired OS is CentOS, go over to CentOS.com and download a disk
3. After installation, open up VirtalBox and click 'Machine' on the top left-hand corner.
4. Click 'new' and select the software provider, select 'Linux' and Version as 'Red Hat (32 or 64-bit)'
5. To check whether your processor is 32 or 64-bits, go over to Settings -> About, under 'System Type'
6. The amount of RAM you allocate to this Virtual Machine (VM) should be at least 2048MB.
7. Once you have created the virtual hard disk, it should appear under ‘Tools’ on the left side of your screen.
8. The steps below are required to install the OS into the VM
   *  Click on ‘Settings’, over at ’Storage’, click on ‘Empty’ under ‘Controller: IDE’
   *  On the right-hand side of the screen under ‘Attributes\Optical Drive:’, click on the ‘disk-shape’  icon and insert your disk
   *  Press ‘Start’ and let it run
   *  Once installation is done, the same installation page will reappear. It will repeat the installation process again
   *  To stop that, click ‘Devices\Optical Disk’ and remove the disk from virtual drive. Under ‘Machine’ click ‘Restart’
   *  The installation screen should not appear anymore and you can let it run
   *  Once you have reached the stage where you select your preferred language and time zone, you can proceed as directed
  
#### Configure internet access on Operating System
* Once you have logged into your OS, you will then have to configure it with internet access.
* If your Virtual Machine (VM) has Graphical User Interface (GUI), simply go over to the top right-hand corner of the screen and select ‘Connect to network’ if it is not already connected to one.
* However, if your VM does not have GUI, please follow the steps below.
* To check if your VM has internet access, open up terminal/command prompt and type: ping 8.8.8.8 (8.8.8.8 is the primary DNS server for Google DNS, so if you can ping to google and there is not ‘Packet Loss’, your VM is connected to the internet
1. Run command below and look for a network interface that starts with the letter 'e'
```
ifconfig
```
2. Find your IP address
```
ip a
```
3. Open up Network Manager with super user privilege to configure network
```
sudo nmtui
```
4. Press enter on 'Edit a connection' and edit the ProfileName, Device, Addresses, Gateway and DNS servers
```
Profile name enp0s3
Device enp0s3
Addresses 192.168.2.50/24
Gateway 192.168.2.1
DNS servers 8.8.8.8
```
5. Select Okay and quit interface
6. Turn off and on connection
```
sudo nmcli connection down enp0s3
sudo nmcli connection up enp0s3
```
7. Check if internet configuration was successful, ping Google
```
ping 8.8.8.8
```

#### Change Hostname
* It can be very confusing when interacting with another computer, so to prevent any mix up, we can change the hostname to something unique.
```
hostnamectl --static set-hostname “uniqueVM”
hostnamectl --pretty set-hostname “uniqueVM”
cat /etc/hostname
```

#### Setup Secure Shell (SSH)
* SSH is a client-server service providing secure encrypted connections over the network connection.
* This simply means that you can access a computer that you do not have physical accessibility to.
* SSH also allows you to transfer files securely over the internet. 
1. Install openssh package
```
dnf install openssh-server
```
2. Start openssh service
```
systemctl start sshd55
```
3. Check status of sshd service
```
systemctl status sshd
```
4. Reload SSH service
```
systemctl enable sshd
```
5. Enable SSH service
```
systemctl enable sshd
```
6. Allow firewall rules to accept 

