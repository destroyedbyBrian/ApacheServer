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
   * [Setup Secure Shell (SSH)](#setup-secure-shell)
   * [Create new administrative user with SSH access](#create-new-administrative-user-with-SSH-access)
   * [Manage Firewall](#manage-firewall)
   * [Log into SSH on local host](#log-into-ssh-on-local-host)
   * [Manage Security-Enhanced Linux SELinux](#manage-security-enhanced-linux-selinux)
   * [Configure SSH accessibility on Wide Area Network (WAN)](#configure-ssh-accessibility-on-wide-area-network)
   * [Transfer files from Host machine to VM](#transfer-files-from-host-machine-to-VM)

### Prerequisites
- - - -
* Any Virtual Machine from Oracle VM VirtualBox
* Basic Linux bash script
* Basic Powershell bash script

### Software Requirements
- - - -
* Oracle VM VirtualBox
* CentOS 8 VM
* Power Shell
* Pip
* Pipenv
* Apache HTTP
* Mod_wsgi
* PuTTY

### Step by Step Guide
- - - -
#### <ins>Install CentOS, Ubuntu or Fedora on Virtual Box</ins>
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
6. Allow firewall rules to accept incoming traffic on SSH port 22
```
firewall-cmd --zone=public --permanent --add-service=ssh 
```
7. Connect to SSH server with your ip address as root
```
ssh root@192.168.0.129
```

#### Create new administrative user with SSH access
* You will not want to always login as root due to security reasons.  
* So, a regular user that has the privileges to execute any command as root user would be recommended.  
1. Login to SSH as root
```
ssh root@192.168.0.129
```
2. Create new user
```
adduser jimmy
```
3. Assign password to user
```
passwd jimmy
```
4. Grants user with super user privilege
```
usermod -aG wheel jimmy
```
5. Permit root login, edit by pressing 'i' and change it from 'yes' to 'no'
```
sudo vi /etc/ssh/sshd_config
```
6. Exit editor page by pressing 'esc' button and enter ':wq'
7. Restart SSH service
```
systemctl restart sshd
```

#### Manage Firewall
* To prevent unauthorised personnel from accessing your private network, you have to establish a barrier between your trusted internal network and untrusted external network. One of the barriers is setting up Firewall.
* In Firewall, there are Zones, they are simply security borders of a network, and interfaces that are in the same zone have similar functions or features.
* In order to configure Firewall commands, you will need to be a user with root privileges or root itself. This saves you the hassle to type in sudo before every command. 
1. Install firewall
```
sudo dnf install firewalld
```
2. Enable firewall
```
systemctl enable firewalld
```
3. Start firewall
```
systemctl start firewalld
```
4. Check status of firewall
```
firewall-cmd --state
```
5. Check interfaces that are active in a zone
```
firewall-cmd --get-active-zones
```
6. To see what is running in the zone
```
firewall-cmd --zone-public --list-all
```

* So now, we will open up port 80, 80 is a port number commonly assigned to the internet communication protocol which is HTTP. It is the port from which a computer sends and receives web client-based communication and messages from a web server and is used to send and receive HTML pages or data.  
* In simpler terms, ports can be referred to as maritime ports where ships dock to load and unload cargos. A place where content (data) is transferred.
* TCP/IP (Transmission Control Protocol/Internet Protocol) is a set of transport and network-layer protocols for machines to communicate with each other over the network.

7. Open up port 80
```
firewall-cmd --zone=public --add-port=80/tcp --permanent
```
8. Reload firewall
```
firewall-cmd --reload
```
9. Check status of firewall
```
firewall-cmd --state
```
10. Enable services like httpd and sshd through firewall
```
firewall-cmd --zone=public --add-service=http --permanent
```
11. Reload firewall
```
firewall-cmd --reload 
```

* If you want to remove ports or services from firewall the following steps are the commands to do so.

12. Remove port
```
firewall-cmd --zone=public --remove-port=80/tcp --permanent 
```
13. Reload firewall
```
firewall-cmd --reload 
```

#### Log into SSH on local host
* To access your VM on your local host, you can use services like SSH.  
1. Check connection  between local host and VM
```
ping 192.168.0.129 // ip address of my CentOS
```
* If the process of pinging fails, or your local host is simply not connected to your VM you can follow the steps below.
2. Open Oracle VM VirtualBox and navigate to Settings/Network. Ensure 'Enable Network Adapter' is checked and for options to 'Attached to:', select 'Bridged Adapter'
3. Repeat step #1

##### Manage Security-Enhanced Linux SELinux 
* In order to have more control over who can access the system/server, SELinux can be used.
* This strengthens the security of the system as it enforces mandatory access control over programs, system services, files and network resources.
* SELinux has 2 global modes; enforcing and permissive.Enforcing mode means that the SELinux policy is in effect and the policies will be followed according to it very strictly. As for permissive mode, it comes in handy when trying to debug or allow some features to run on some instances which are not allowed in enforcing mode.This allows us to have a choice on the policies that are enforced.
1. Check status of SELinux
```
sestatus
```
2. Check whether SELinux is in Enforcing or Permissive mode
```
setenforce
```
3. To set SELinux in permissive mode
```
setenforce 0
```
4. To set SELinux in enforcing mode
```
setenforce 1
```
5. Edit system configuration code for SELinux
```
sudo vi /etc/sysconfig/selinux
```
6. Change selinux = enforcing

* As we try to make SSH accessible not only through LAN but WAN, you will face a few issues regarding port connection refusals. So, we will disable SELinux for now.

7. Disable enforcing mode
```
sudo setenforce 0
```
8. Edit system configuration code for SELinux
9. Chagne selinux = disabled
10. Reboot VM
11. Check status of SELinux
```
sestatus
```

#### Configure SSH accessibility on Wide Area Network (WAN)
* Expand the range of SSH accessibility on WAN, making it convenient for other authorized users to work remotely.
* To access SSH outside of LAN, you need to open the ports of the router that is connected to your host system.
* IF your IP address is 192.168.0.129, your router's IP would be 192.168.0.1
1. Edit the configuration file of SSH daemon
```
sudo nano /etc/ssh/sshd_config 
```
2. Once the editor is up, look for Port 22 and ‘uncomment’ the line. (Simply delete the hashtag)
3. Exit the editor
4. Restart SSH
```
sudo systemctl restart sshd
```
5. Open up your browser and key in your router's IP on the search bar e.g. 192.168.0.1
6. My router is a D-Link router, simply click login without typing in anything. If your router is different, look it up on how to access your router
7. After logging in, navigate to 'Advanced' and click on 'Port Forwarding' on the left-hand side
8. Edit form and save settings
```
Name: SSH
IP Address: *host ip address*
TCP: 22
UDP: 22
```
* To check if SSH is accessible out of LAN, you can download ssh apps on your smartphone. As for me, I would be working on the app ‘Termius’.
* Test the connection of SSH while your smartphone is still connected to your home router.
9. Login to SSH on Termius
```
ssh root@192.168.0.129
```
* Disconnect from the Wi-Fi on your smartphone and turn on your mobile data and repeat step #9.
10. Configuration of SSH accessibilty on WAN would be successful if SSH login attempt is successful.

#### Transfer files from Host machine to VM
* If you are running a VM without GUI, it is difficult to download images on the web.
* The solution to this problem is to download the images on your host machine and transfer them to your VM.
1. Install PuTTY on host machine
2. Once installation is completed, go over to the search bar and look for 'PuTTy'
3. Click on 'Open file location'
4. 
