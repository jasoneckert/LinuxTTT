# Ownership
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`			
   - `ll proposal1` (note the user and group owner)
   - `chown nobody proposal1`
   - `chgrp nobody proposal1`
   - `ll proposal1` (note the nobody user and nobody group owner)
   - `chown root:bin proposal1`
   - `ll proposal1` (note the root user and bin group owner)

# Permissions
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `chmod u=r,g=rwx,o=w letter`
   - `ll letter` (note the mode is r--rwx-w-)
   - `chmod g-wx,o-w letter`
   - `ll letter` (note the mode is r--r-----)
   - `chmod 000 letter`
   - `ll letter` (note the mode is ---------)
   - `chmod 777 letter`
   - `ll letter` (note the mode is rwxrwxrwx)
   - `chmod 664 letter`
   - `ll letter` (note the mode is rw-rw-r--)

# Default Permissions
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `umask`	(note the default umask of 0022, which is 022)
   - `mkdir sampledir`
   - `ll -d sampledir` (note the default permissions of rwx-r-xr-x)
   - `touch samplefile`
   - `ll samplefile` (note the default permissions of rw--r--r--)
   - `umask	077`			
   - `mkdir sampledir2`
   - `ll -d sampledir2` (note the default permissions of rwx-------)
   - `touch samplefile2`
   - `ll samplefile2` (note the default permissions of rw--------)

# Special Permissions
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `chmod 4666 /bin/false`	
   - `ll /bin/false` (note the capitalization indicating absence of x)
   - `chmod 4777 /bin/false`	
   - `ll /bin/false` (users who execute false become the owner!)
   - `mkdir /public`
   - `chmod 1777 /public` (sets the sticky bit)
   - `touch /public/rootfile`
   - `ll /public/rootfile` (note that the owner is root)
   - `exit` (log into a new terminal as woot to execute remaining commands)
   - `touch /public/wootfile`
   - `ll /public/wootfile` (note that the owner is woot)
   - `rm -f /public/wootfile`
   - `rm -f /public/rootfile` (note the error message, even though you have w permission on the directory)
   - `su -` (switches to root to execute commands in the next 2 sections)
   
# Modifying the ACL (beyond User, Group, Other)
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `setfacl -m u:woot:rw- letter`	
   - `ll letter` (note the + symbol next to the mode)
   - `getfacl letter` (note the mask and entry for woot)
   - `setfacl -b letter`
   - `ll letter` (note the + symbol is no longer present)
   - `getfacl letter` (note the absence of additional ACL entries)

# File Attributes
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `lsattr letter`
   - `chattr +i letter`
   - `lsattr letter` (note the immutable attribute is set)
   - `vi letter` (make a change and attempt to save the file – when you are denied, exit vi discarding changes)
   - `chattr -i letter`
   - `lsattr letter`
   - `man chattr` (note the description of other attributes)

# Filesystems
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `ll /dev/sda*` (note the block file type, major number and minor numbers)
   - `ll /dev/tty[0-6]` (note the character file type, major number and minor numbers)
   * In the settings for your virtual machine, insert your Fedora ISO image into the virtual DVD drive. 
   * In the GNOME desktop, navigate to the Files app and note that your DVD is shown as mounted. 
   - `df -hT` (note the standard partitions are mounted to /, /home and /boot, and DVD automounted to /run/media/woot/label) 
   - `lsblk` (note that zswap is mounted as SWAP) 
   - `ls /sys/block` 
   - `ls /run/media/woot/label`
   - `umount /run/media/woot/label` (replace `label` with the actual label usign Tab completion) 
   - `df -hT` (verify that the virtual DVD is no longer mounted)
   - `mount /dev/cdrom /mnt` (/dev/cdrom is a symlink to your CD/DVD device file)
   - `ls /mnt` (note the DVD contents)
   - `fdisk /dev/sda` 
      - `p` 
      - `n`
      - If MBR disk, choose `p` for primary partition, otherwise this will not be displayed
      - `[Enter]` (for next available partition number)
      - `[Enter]` (for first available sector)
      - `[Enter]` (for last available sector)
      - `p`
      - `w`
   - `partprobe` 
   - `mkfs -t ext4 /dev/sdan` (where n is the partition number from fdisk, probably 4)
   - `mkdir /data`
   - `mount /dev/sdan /data`
   - `lsblk`
   - `cp -R /root/classfiles /data`
   - `ls /data`
   - `ls /data/classfiles`
   - `du -hs /data/classfiles`
   - `df -hT`
   - `umount /data`
   - `pvcreate /dev/sdan` (choose `y` to remove the existing ext4 filesystem)
   - `vgcreate vg00 /dev/sdan`  
   - `vgdisplay` (note the value next to VG Size in GB for creating LVs)
   - `lvcreate -L size -n data vg00` (where size is the VG Size from the last command)
   - `mkfs -t ext4 /dev/vg00/data`
   - `mount /dev/vg00/data /data`
   - `lvdisplay`
   - `lsblk`
   - `df -hT`
   - `vi /etc/fstab` (add the following line and save your changes)
      - `/dev/vg00/data   /data  ext4  defaults  0  0` 
   - `reboot` (after the system has booted, open a Terminal as root)
   - `lsblk` (verify that the data LV is mounted)
   * In your virtualization software, add a second 10GB virtual hard disk to your Fedora Workstation virtual machine (this will be /dev/sdb). 
   - `lsblk` 
   - `fdisk /dev/sdb`
     - `n`
     - `[Enter]`
     - `[Enter]`
     - `[Enter]`
     - `p`
     - `w`
   - `pvcreate /dev/sdb1`
   - `vgextend vg00 /dev/sdb1`
   - `vgdisplay`  (note the additional space available in the vg00 pool)
   - `lvextend -L +size –r vg00/data` (where size is the additional space)
   - `lsblk`
   - `df -hT`  (note that your /data volume is now much larger!)
   - `umount /data`
   - `fsck -f /dev/vg00/data`
   - `mount /data` (this works because of the line in /etc/fstab)
   * Challenge: In your virtualization software, add a third 10GB virtual hard disk to your Fedora Workstation virtual machine (this will be /dev/sdc). Create a standard partition on it that is formattted with XFS and mounted to /xfs (also at boot time!). Next, copy some files to /xfs and check your XFS filesystem for errors. 

# Advanced (RAID) Filesystems
   * In the Settings of your Ubuntu Server virtual machine, add 4 additional virtual disks (10GB each)
   * Boot your Ubuntu Server virtual machine and open a Terminal as root 
   - `lsblk` (note that your new block devices are detected) 
   - `mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sdb /dev/sdc /dev/sdd` (to create a softare RAID 5)
   - `mkfs –t ext4 /dev/md0`
   - `mkdir /raid5`
   - `mount /dev/md0 /raid5` 
   - `lsblk`
   - `df –hT`
   - `cat /proc/mdstat`
   - `mdadm --detail /dev/md0` 
   - `sync`
   - `mdadm --manage /dev/md0 --fail /dev/sdb` (to simulate a failure on sdb)
   - `mdadm --detail /dev/md0` 
   - `cat /proc/mdstat` 
   - `mdadm --manage /dev/md0 --remove /dev/sdb`
   - `mdadm --manage /dev/md0 --add /dev/sde` 
   - `mdadm --detail /dev/md0` (after a few minutes, you should see it fully synchronized)  
   - `umount /raid5` 
   - `mdadm --stop /dev/md0` 
   - `fdisk /dev/sdb`
     - `w` (this will remove the existing RAID signature from sdb)
   * Repeat this fdisk command for /dev/sdc, /dev/sdd, and /dev/sde
   - `mkfs.btrfs -m raid0 -d raid0 /dev/sdb /dev/sdc /dev/sdd -f`
   - `mkdir /btrfs`
   - `mount /dev/sdb /btrfs`
   - `df –hT`
   - `lsblk`
   - `btrfs device add -f /dev/sde /btrfs`
   - `btrfs filesystem balance /btrfs`
   - `df -hT`
   - `btrfs subvolume list /btrfs` 
   - `btrfs subvolume create /btrfs/stuff` 
   - `btrfs subvolume list /btrfs` 
   - `cp /etc/services /btrfs/stuff`
   - `mkdir /stuff` 
   - `mount -o subvol=stuff,compress=lzo /dev/sdb /stuff` 
   - `df –hT` 
   - `ls –l /stuff` 
   - `umount /stuff` 
   - `umount /btrfs` 
   - `mkfs.btrfs -m raid1 -d raid1 /dev/sdb /dev/sdc /dev/sdd /dev/sde -f` 
   - `btrfs filesystem show /dev/sdb` 
   - `mount /dev/sdb /btrfs`
   - `df -hT` 
   - `lsblk` 
   - `btrfs filesystem df /btrfs`
   - `umount /btrfs`
   - `mkfs.btrfs -m raid5 -d raid5 /dev/sdb /dev/sdc /dev/sdd /dev/sde -f`
   - `btrfs filesystem show /dev/sdb`
   - `mount /dev/sdb /btrfs`
   - `df -hT` 
   -  `lsblk` 
   - `btrfs filesystem df /btrfs` 
   - `umount /btrfs`
   - `btrfs check /dev/sdb`
   - `btrfsck /dev/sdb`
   - `poweroff`
   * In the Settings of your Ubuntu Server virtual machine, remove the 4 additional 10GB virtual disks you added earlier.
