# Installing Rocky Linux & Obtaining Sample Files

During this TTT, we’ll be installing two (2) different Linux distributions. For the first part of this TTT, we’ll use the latest Rocky Linux distribution. 

1. On your Windows or macOS computer, download the ISO image for Rocky Linux Workstation for your architecture (x86/ARM) from https://rockylinux.org/
2. Install a native installation of Rocky Linux on bare metal. This will save you the headaches of running and configuring a virtual machine and is a more realistic experience.
   - For macOS on the Apple Silicon (ARM) platform, you can use [UTM](https://mac.getutm.app/), [Oracle VirtualBox](https://www.virtualbox.org/), or [VMWare Fusion](https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion). 
   - For macOS on the x86 platform, you can use [Oracle Virtualbox](https://www.virtualbox.org/) or [VMWare Fusion](https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion).
   - For Windows on the x86 or Snapdragon (ARM) platform, you can use [Oracle Virtualbox](https://www.virtualbox.org/), [VMWare Workstation](https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion). 
   - Refer to the free documentation on the hypervisor website to learn how to create a virtual machine using the hypervisor software.
3. In your virtualization software, create a new virtual machine (VM) called Fedora Workstation. This VM should have:
   - 4GB of memory (not dynamically allocated)
   - A network connection via the network interface in your computer that provides Internet access – using an external virtual switch (Hyper-V) or bridged mode (VMWare, Oracle VirtualBox, UTM)
   - A 64GB SATA/SAS/SCSI virtual hard disk drive 
   - The virtual machine DVD drive attached to the ISO image of Fedora Workstation
4. Start your Fedora Workstation virtual machine. 
5. Press Enter to test your media and boot Fedora Live. 
6. Once the graphical desktop has loaded, click ***Install Fedora***.
7. Select ***English (United States)***.      
8. On the Installation method screen, note your disk name (e.g., sda) and select ***Launch storage editor*** from the ellipsis (...) menu in the upper-right and click ***Launch storage editor***.  
    - Select ***Create partition table*** from the ellipsis menu next to your storage device. 
    - Click ***Initialize***. 
    - Select ***Create partition*** from the ellipsis menu next to Free space. Enter the following information and click ***Create***:
      - Name = ***UEFI***, Mount point = ***/boot/efi***, Type = ***EFI System partition***, Size = ***1 GB***
    - Repeat the previous step (Select ***Create partition*** from the ellipsis menu next to Free space) to create the following two (2) partitions:
      - Name = ***Boot Partition***, Mount point = ***/boot***, Type = ***EXT4***, Size = ***1 GB***
      - Name = ***Root Partition***, Mount point = ***/***, Type = ***EXT4***, Size = ***50 GB***
    - This will leave some unpartitioned Free space for later exercise.
    - Click ***Return to installation***.
9. Click ***Continue***.
10. Click ***Install***.
11. Click ***Exit to live desktop*** and use the icon in the upper right to Power Off your system.
12. In the settings for your virtual machine, remove the Fedora Workstation ISO image from the virtual DVD and start your Fedora Workstation virtual machine.
13. On the first boot after installation, you must complete a Welcome wizard - make the following selections when prompted:
    - English (US) keyboard
    - Disable Location Services and Automatic Problem Reporting
    - Enable Third-Party Repositories
    - Create a regular user called ***woot*** with a password of ***Secret555***
14. Optionally take the GNOME tour.
15. Open a terminal app by navigating to the icon in the upper left corner, click the ***Show Apps*** icon in the bottom bar (dock), and click ***Terminal***. 
17. Type `sudo passwd root` at the command prompt and press Enter. 
    - Enter the password for your woot user account (`Secret555`)
    - Next, enter a password of `Secret555` for the root user when prompted (twice).
18. Type `su - root` and press Enter. 
    - Supply the root user’s password of Secret555 when prompted. 
    - Type `git clone https://github.com/jasoneckert/classfiles.git` and press Enter. 
    - Type `poweroff` to shut down your Fedora virtual machine.
