# Working with Kubernetes (K3s)
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `curl -sfL https://get.k3s.io | sh -` 
  - `kubectl get node`
  - `kubectl create deployment webapp --image=jasoneckert/x86webapp:latest`	(creates a deployment configuration used to start/manage/scale containers based on a container image that is pulled in from Docker Hub)
  - `kubectl get deployment` (once the container image is downloaded from Docker Hub, you'll see READY 1/1)
  - `kubectl expose deployment webapp --type=NodePort --port=80` (creates a service to expose the deployment (NodePort exposes the port on every cluster node)
  - `kubectl get service` (note that the service is exposed within the internal Kubernetes network - 10.0.0.0)
  - `kubectl get pod` (note that one pod was created for the deployment - the pod represents one or more containers created by a deployment and is the smallest unit of management in Kubernetes)
  - `kubectl edit deployment webapp` (modify `Replicas=3` and save your changes)
  - `kubectl get deployment webapp` (note that 3 containers are now running in the pod based on the same container image - requests sent to the service are load balanced across these containers)
  - `kubectl get pod` (to see these 3 containers)
  - `kubectl logs -f -l app=webapp --prefix=true` (this displays events within the container and is often useful when troubleshooting issues with a deployment such as not being able to pull the container image, or a issue where the webapp can't connect to other required resources)
  - `kubectl get service webapp -o yaml` (note the `nodePort:` line to see which service port is automatically exposed outside of the Kubernetes cluster)
  - Open a Web browser on your Fedora Workstation and navigate to http://UbuntuIP:port (where port is the exposed service port from the previous command) - K3s comes preconfigured with the Traefik ingress controller that allows external traffic to enter your Kubernetes cluster and interact with exposed services
  - `kubectl top nodes` (note that K3s comes with a metrics service that collects statistics - these can be used to perform Horizontal Pod Autoscaling (HPA))
  - `kubectl autoscale deployment webapp --min=3 --max=8 --cpu-percent=50` (create HPA configuration for your Web app that automatically scales from 3 to 8 pods when a consistent trend of more than 50% of the CPU is consumed)
  - `kubectl get hpa webapp` 
  - `kubectl set image deployment/webapp webapp=jasoneckert/webapp` (Kubernetes will immediately start replacing the pods with your new image in sequence until all of them are upgraded - if an upgraded image causes issues, you can revert to the previous image, which you'll need to do since jasoneckert/webapp was compiled for ARM64!)
  - `kubectl get pod` (note that the first container used during the replacement has a CrashLoopBackOff or Error state indicating a serious issue)
  - `kubectl rollout undo deployment webapp` 
  - `kubectl get pod` (note it was rolled back successfully)
  - `/usr/local/bin/k3s-uninstall.sh` (removes K3s)

# Working with Ansible
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `useradd –m ansible` 
  - `passwd ansible` (supply `Secret555`)
  - `usermod –aG sudo ansible` 
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `ssh-copy-id ansible@UbuntuServerIP` 
  - `dnf install ansible -y` 
  - `vi hosts.inventory` (add the following two lines, save and quit)
    - [ubuntu]
    - UbuntuServerIP
  - `wget https://raw.githubusercontent.com/jasoneckert/LinuxTTT/refs/heads/main/files/deploylamp.yml` 
  - `less deploylamp.yml` (Can you understand what this file is going to do? Ask AI if you can't!)
  - `ansible-playbook –i hosts.inventory –u ansible --ask-become-pass deploylamp.yml ` (note the colour that indicates a change has occurred)
  - `ansible-playbook –i hosts.inventory –u ansible --ask-become-pass deploylamp.yml ` (can you tell that no actions were necessary when this was executed a second time?)
  - `ansible –i hosts.inventory ubuntu -a "systemctl restart apache2.service" –u ansible –b --ask-become-pass` 
  - `vi ansible.cfg` (add the following three lines, save and quit)
    - [defaults]
    - inventory = hosts.inventory
    - remote_user = ansible
  - `ansible ubuntu -a "systemctl restart apache2.service" –b --ask-become-pass` 
  - `ansible ubuntu -a "apt install cowsay -y" –b --ask-become-pass` 
  - `ansible ubuntu -a "cowsay Ansible Rocks!"` 
  - `ansible ubuntu -a "hostnamectl"` 

# Security
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `nmap -sT localhost` (note that telnet is not necessary)
  - `systemctl stop telnet.socket ; systemctl disable telnet.socket`
  - `nmap -sT localhost` (note that your attack surface is now lower)
  - `tcpdump -i eth0`    (press `[Ctrl]+c` after a few minutes)
  - `last`
  - `lastb`
  - `gpg --gen-key` (supply your email and Secret555 passphrase)
  - `cd classfiles`
  - `gpg --encrypt --sign -r email letter`
  - `file letter* ; head -1 letter*`
  - `gpg letter.gpg` (choose to overwrite letter)
  - `head -1 letter`
  - `poweroff`
  * In the Settings of your virtual machine, add an additional 10GB virtual hard disk (/dev/sdc) to your Fedora Workstation virtual machine and start it. 
  - `fdisk /dev/sdc` (create a single partition that uses the space on the entire disk)
  - `cryptsetup luksFormat /dev/sdc1` (specify lUkS-555 as the passphrase)
  - `cryptsetup luksOpen /dev/sdc1 private`
  - `lsblk`
  - `mkfs -t ext4 /dev/mapper/private`
  - `mkdir /private`
  - `mount /dev/mapper/private /private` 
  - `lsblk`
  - `cp -R classfiles/* /private ; ls /private`
  - `vi /etc/crypttab (add the line: private /dev/sdc1)`
  - `vi /etc/fstab` (add the line: `/dev/mapper/private /private ext4 defaults 0 0`)
  - `reboot` (supply the lUkS-555 passphrase to mount /dev/sdc1, then obtain a shell as root)
  - `lsblk` (verify that /dev/sdc1 is mounted as a crypt volume)
  - `ls /private`
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `vi /etc/pam.d/common-auth` (add the following line above the `pam_unix.so` line) 
     - `auth required pam_faillock.so authfail deny=3 unlock_time=7200`
  * Log into tty5 at least four times as woot with an incorrect password, and then try to log in with the correct password (it will not let you!) 
  * Next, return to your previous Terminal as root
  - `faillock` (note that woot has been locked out after 3 invalid attempts)
  - `faillock --reset --user woot`
  - `faillock` 
  - `vi /etc/motd` (create a security message of your choice, then log out completely and then log in again to view it)

# Firewalls
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `iptables -L` (note the default policy for each chain)
  - `iptables –P INPUT DROP`  
  - `ping gateway`    (where gateway is your default gateway – this should fail)
  - `iptables -L` (note the default policy for the INPUT chain)
  - `iptables –A INPUT –s subnet -j ACCEPT`  (where subnet is your local IP network)
  - `iptables -L` (note the firewall rule under the INPUT chain)
  - `ping gateway`    (where gateway is your default gateway – this works now because the ping reply is now allowed into your system)
  - `iptables –F ; iptables –P INPUT ACCEPT` (this undoes our configuration - you'd have to instead use `iptables-save` if you wanted to configure them at boot time)
  - `iptables -L` (note that your configuration has been reverted to defaults)
  - `nft add table inet filter`
  - `nft add chain inet filter input`
  - `nft add rule inet filter input drop`
  - `nft add rule inet filter input tcp dport 22 accept`
  - `nft add rule inet filter input tcp dport 80 accept`
  - `nft list table inet filter`
  - `nft list ruleset` (you'd have to redirect this output to /etc/nftables.conf to set the configuration at boot time)
  - `nft delete table inet filter`
  - `ufw enable` 
  - `ufw status verbose`
  - `ufw allow ssh ; ufw allow ftp ; ufw allow nfs` 
  - `ufw allow samba ; ufw allow http ; ufw allow postgres`
  - `ufw status verbose`
  - `ufw disable` 
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `iptables -L`
  - `firewall-cmd --get-zones` 
  - `firewall-cmd --get-services` 
  - `firewall-cmd --list-all-zones` 
  - `firewall-cmd --get-active-zone`
  - `firewall-cmd --list-all`  
  - `firewall-cmd --add-service samba` 
  - `firewall-cmd --add-service samba --permanent`
  - `firewall-cmd --add-port 666/tcp` 
  - `firewall-cmd --add-port 666/tcp --permanent`
  - `firewall-cmd --list-all`  
  - `firewall-cmd --query-service=samba`  

# SELinux 
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `sestatus` (note that SELinux is enabled and enforcing by default)
  - `cat /etc/selinux/config`
  - `setenforce permissive`
  - `sestatus`
  - `setenforce enforcing`
  - `sestatus`
  - `ps -eZ | less`
  - `ll -Z /var/* | less`
  - `ll -Z classfiles` (note the default type of admin_home)
  - `mv classfiles/text.err /var`
  - `ll -Z /var` (note that text.err still has the same type)
  - `restorecon /var/text.err`
  - `ll -Z /var` (note that text.err now has a generic var type)
  - `systemctl start httpd.service` (open Firefox and navigate to `http://localhost`)
  - `vi /etc/httpd/conf.d/userdir.conf` (comment the ***UserDir disabled*** line and uncomment the ***UserDir public_html*** line, saving your changes)
  - `systemctl restart httpd.service`
  - `mkdir /home/woot/public_html`
  - `chmod 755 /home/woot /home/woot/public_html` 
  - `echo "<html><body><h1>Woot!</h1></body></html>" > /home/woot/public_html/index.html`
  - `chmod 644 /home/woot/public_html/index.html` (in Firefox, navigate to `http://localhost/~woot`)
  - `audit2why < /var/log/audit/audit.log` (note that SELinux prevented acccess to /home/woot/public_html/index.html and the command to allow it)
  - `ls -Z /home/woot/public_html/index.html` (note the type classification)
  - `ps -eZ | grep httpd` (note the type classification - it should match the previous step, so that's not the issue)
  - `getsebool -a | grep httpd | less` (note that the default SELinux policy has httpd_enable_homedirs = off)
  - `setsebool -P httpd_enable_homedirs 1` (in Firefox, navigate to `http://localhost/~woot` and it should work!)
