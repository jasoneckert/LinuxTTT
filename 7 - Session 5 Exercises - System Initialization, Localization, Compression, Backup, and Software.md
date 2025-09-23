# System Initialization & Daemons
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `vi /etc/default/grub` (change the `GRUB_TIMEOUT` to `30` seconds and `GRUB_DISABLE_SUBMENU` to `"false"`, saving your changes)
   - `cat /boot/efi/EFI/fedora/grub.cfg` (note that it refers to /boot/grub2/grub.cfg so that you can run the next command regardless of whether your hypervisor emulates UEFI or not!)
   - `grub2-mkconfig –o /boot/grub2/grub.cfg` 
   - `grep "timeout=30" /boot/grub2/grub.cfg` 
   - `cat /boot/loader/entries/latestkernel.conf` (use [Tab] to autocomplete latestkernel, note the contents for later!)
   - `reboot` (at the GRUB menu, press `c` to obtain a command prompt and run the following)
     - `[Tab]`
     - `ls`
     - `[Esc]`
     - `e` (scroll down to the `linux ($root)/vmlinuz-*` line, and append the word `single` to the end) 
     - `[Ctrl]+x` (supply the root password once the system reaches single user mode) 
   - `runlevel` 
   - `reboot` (press `[Enter]` at the GRUB menu to boot the default kernel normally - following boot, open a Terminal as root)  
   - `runlevel` (note that your current runlevel is 5 and previous is N)
   - `init 3`
   - `runlevel` (note that your current runlevel is 3 and previous is 5)
   - `init 6` (following boot, open a Teminal as root)
   - `systemctl | grep crond` (note that crond is a modern Systemd-managed daemon)
   - `systemctl status crond.service` (crond is started, and set to start in the default target)
   - `systemctl restart crond.service`
   - `systemctl disable crond.service`
   - `systemctl status crond.service` (note that crond not set to start in the default target)
   - `systemctl enable crond.service`
   - `systemctl get-default` (note that graphical.target is the default target)
   - `systemctl daemon-reload` (you would run this after changing one or more daemon config files, forcing all daemons to reload their config files if they have been modified)
   - `cat /lib/systemd/system/crond.service` 
   - `find /lib/systemd -name "*.mount"` 
   - `find /lib/systemd -name "*.timer"` 
   - `find /lib/systemd -name "*.target"` 
   - `journalctl -b` 

# Localization
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `date +%s` 
   - `hwclock`
   - `date`
   - `timedatectl`
   - `ll /etc/localtime`
   - `locale`
   - `localectl`
   - `localectl list-locales | less`
   - `localectl list-keymaps | less` (note the us-mac keymap for U.S. Apple keyboards) 

# Compression
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `zip -v bigfile.zip bigfile` (note the compression ratio)
   - `unzip bigfile.zip` (choose to overwrite bigfile)
   - `7z a bigfile.7z bigfile` (calculate the compression ratio from the info shown)
   - `7z e -aoa bigfile.7z`  
   - `gzip -v -9 bigfile` (note the compression ratio)
   - `cat bigfile.gz`
   - `zcat bigfile.gz`
   - `zless bigfile.gz`
   - `gunzip bigfile.gz`
   - `xz -v -9 bigfile` (note the compression ratio)
   - `unxz bigfile.xz`
   - `bzip2 -v bigfile` (note the compression ratio is better than other commands)
   - `bunzip2 bigfile.bz2`
 
# Backup
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `tar -zcvf /poems.tar.gz Poems`
   - `tar -ztvf /poems.tar.gz`
   - `mkdir test1 && cd test1`
   - `tar -zxvf /poems.tar.gz`
   - `ls`
   - `tree`
   - `cd ~/classfiles`
   - `find Poems | cpio -vocBL -O /poems.cpio`
   - `cpio -Bitv -I /poems.cpio`
   - `mkdir test2 && cd test2`
   - `cpio -vicdumB -I /poems.cpio`
   - `dd if=/dev/sda1 of=/sda1.dd`
   - `df -hT`
   - `ll /sda1.dd` (note the size of the backup = the size of the filesystem)
  
# Software
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `dnf install @development-tools`   
   - `git clone https://github.com/bartobri/no-more-secrets.git`
   - `cd no-more-secrets` 
   - `less README.md`
   - `make nms` 
   - `make sneakers` 
   - `make install`
   - `which sneakers`
   - `which nms`
   - `sneakers`
   - `ls -l | nms` 
   - `dnf install nmap`
   - `nmap -sT localhost`
   - `rpm -qi nmap`
   - `rpm -V nmap`
   - `rpm -ql nmap`
   - `dnf info nmap`
   - `dnf install cowsay sl`
   - `cowsay "Linux Rocks!"`
   - `sl`
   - `dnf list --installed`
   - `dnf list --available`
   * Open a Terminal on your Ubuntu Server virtual machine as root
   - `apt install nmap`
   - `dpkg -l`
   - `dpkg -l '*nmap*'`
   - `dpkg -L nmap | less`
   - `apt install hollywood`
   - `hollywood` (press [Ctrl]+c to quit)
   - `snap install powershell --classic`
   - `snap info powershell`
   - `snap list`
   - `powershell`
     - `Get-Host`
     - `exit`
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `flatpak remote-ls | less` 
   - `flatpak search vlc`
   - `flatpak install flathub org.videolan.VLC` (search for VLC in the GNOME desktop to run it)
   * In Firefox on your Fedora Workstation (as woot), download the Conky .appimage package from https://appimagehub.com/ for your architecture. 
   * Next, open a Terminal app as woot and run the following commands to run your AppImage package:
   - `cd Downloads`
   - `chmod 755 conky*.AppImage`
   - `./conky*.AppImage` 
