# Installing Ubuntu Server

## Note that this is a command line installer program - you must use your Tab key to navigate options, Spacebar to add checks to boxes, and Enter to select options.

1. On your Windows or macOS computer, download the ISO image for Ubuntu Server LTS for your architecture (x86/ARM) from https://ubuntu.com/download/server.
2. In your virtualization software, create a new virtual machine (VM) called Ubuntu Server. This VM should have:
   - 2GB of memory (not dynamically allocated)
   - A network connection via the network interface in your computer that provides Internet access – using an external virtual switch (Hyper-V) or bridged mode (VMWare, Oracle VirtualBox, UTM)
   - A 64GB SATA/SAS/SCSI virtual hard disk drive 
   - The virtual machine DVD drive attached to the ISO image of Ubuntu Server
3. Start your Ubuntu Server virtual machine. 
4. At the Welcome screen, GNU GRUB screen, ensure that ***Try or Install Ubuntu Server*** is selected and press Enter.
5. At the Welcome screen, ensure that ***English*** is selected and press Enter.
6. If you are presented with an ***Installer update available screen***, ensure that ***Continue without updating*** is selected and press Enter.
7. At the Keyboard configuration screen, select ***English (US)*** and select ***Done***. 
8. At the Choose the type of installation screen, ensure that ***Ubuntu Server*** is checked and select ***Done***.
7. At the Network configuration screen, ensure that your network interface is set to ***DHCP*** and select ***Done***.
8. At the Proxy configuration screen, select ***Done***.
9. At the Configure Ubuntu archive mirror screen, select ***Done*** after the tests are passed.
10. At the Guided storage configuration screen, ensure that ***Use an entire disk*** and ***Set up this disk as an LVM group*** are selected and select ***Done***.
11. At the Storage configuration screen, note that the root filesystem (ubuntu-lv) does not fill the entire disk. Select ***ubuntu-lv***, choose ***Edit***, enter the maximum size into the Size dialog box, and select ***Save***. Review your configuration, select ***Done***, and then ***Continue***.
12. At the Profile setup screen, supply the following and select Done when finished:
    - Your name = ***woot***   
    - Your server’s name = ***ubuntu***
    - Username = ***woot***    
    - Password = ***Secret555***
13. At the Upgrade to Ubuntu Pro screen, ensure that ***Skip for now*** is checked and select ***Continue***.
14. At the SSH setup, select ***Install OpenSSH server*** with your spacebar and select ***Done***.
13. At the Featured Server Snaps screen, select ***Done***.
14. At the Install complete screen, select ***Reboot Now***. When prompted to remove your installation medium, press Enter.
15. After the system has booted, log in as ***woot***, type `sudo passwd root` and set the root user password to `Secret555`.
16. Run `ip addr` and note your IP address. If you prefer to use a Terminal app to connect to this Ubuntu Server in the future, you could run the `ssh woot@IPaddress` command from your Fedora Workstation Terminal app, the Windows Terminal app, or the macOS Terminal app to obtain a remote shell on the Ubuntu Server using SSH. Once connected to this remote shell, you can run the `su -` command as usual to switch to the root user.
17. Type `poweroff` to shut down your Ubuntu Server virtual machine.
