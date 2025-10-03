# FTP
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `apt install vsftpd -y` 
  - `vi /etc/vsftpd.conf` (view the comments, uncomment the `write_enable=YES` line and save your changes)
  - `systemctl restart vsftpd.service`
  - `nmap -sT localhost`
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `dnf install filezilla -y` 
  - Next, open the Filezilla app in your GNOME desktop and supply the following information and click Quickconnect (accept the SSH keys when prompted):
     - Host = `sftp://UbuntuIP`
     - Username = `woot`
     - Password = `Secret555`
  - Drag some file from your local home directory (left pane) to your remote home directory (right pane) to upload them, and vice versa 
   
# NFS
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `apt install nfs-kernel-server -y` 
  - `nmap -sT localhost` (note rpcbind and nfs) 
  - `vi /etc/exports` (add the following line, saving your changes)
    - `/etc *(rw)` 
  - `exportfs -a` 
  - `showmount –e localhost` 
  - `mount –t nfs localhost:/etc /mnt` 
  - `df -hT`
  - `cat /mnt/hosts` (note that you are viewing /etc/hosts on the remote system via NFS)
  - `umount /mnt` 
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `mount –t nfs IPaddress:/etc /mnt` (where IPaddress is your Ubuntu Server IP address)
  - `df -hT` 
  - `ls /mnt`
  - `cat /mnt/hosts`
  - `umount /mnt` 
  - `sshfs woot@IPaddress:/etc /mnt` (where IPaddress is your Ubuntu Server IP address - SSHFS is an NFS-like implementation using SSH)
  - `df -hT`
  - `cat /mnt/hosts` 
  - `umount /mnt`

# Samba
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `apt install samba smbclient cifs-utils -y`
  - `ps –ef | grep mbd` (note that smbd and nmbd are running)
  - `vi /etc/samba/smb.conf` 
     * View the comments and then add the following line under the `workgroup = WORKGROUP` line
       - `netbios name = ubuntu`
     * Uncomment/modify the section that shares out all home directories
       - `[homes]`
       - `comment = Home Directories`
       - `browseable = yes`
       - `read only = no`
     * Add the following share definition to the bottom of the file, save your changes and quit vi
       - `[etc]`
       - `comment = The etc directory`
       - `path = /etc`
       - `guest ok = yes`
       - `read only = yes`
       - `browseable = yes`
  - `testparm` (if you made an error, re-edit the /etc/samba/smb.conf file to fix it)
  - `systemctl restart smbd.service ; systemctl restart nmbd.service` 
  - `smbpasswd –a root` (supply Secret555 when prompted)
  - `smbpasswd –a woot` (supply Secret555 when prompted)
  - `pdbedit –L` (note that both root and woot have Samba passwords)
  - `nmblookup ubuntu` (note that your NetBIOS name resolves to your IP address)
  - `smbclient –L ubuntu` (note your shares shown, including the root home directory)
  - `ls -a` (note the contents of your home directory)
  - `mount –t cifs //ubuntu/root /mnt`
  - `df -hT`
  - `ls -a /mnt` (note your home directory contents)
  * On your Fedora Workstation virtual machine, open the Files app and enter `smb://ubuntu/woot` in the search bar and choose to connect as a regular user (Username = `woot`, Domain = `WORKGROUP`, Password = `Secret555`) - next, view woot's home directory on the Ubuntu server, and copy some files to and from local directories on your Fedora system (eject your remotely-mounted home directory in the Files app when finished)
  * If your host OS is Windows, open File Explorer and enter `\\ubuntu` in the search bar - next, open PowerShell as Administrator and run `net use z: \\ubuntu\woot /user:woot Secret555` (access your Z:\ mapped drive afterwards in File Explorer)
  * If your host OS is macOS, open the Finder app and navigate to Go > Connect to Server, enter `smb://ubuntu` and click Connect (note the connection in your Finder)
  * Repeat the same connection from your Fedora, Windows, and macOS clients but to the etc share!

# Apache & Nginx
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `apt install apache2 -y`
  - `vi /var/www/html/index.html` (page down until you see the text `Apache2 Default Page` under a `<span` tag and change that text to something else of your choice, saving your changes when finished)
  - `curl http://localhost | less` (note the same HTML webpage)
  * Open Firefox on your Fedora Workstation and navigate to http://UbuntuIPaddress to view your webpage, then return to your Ubuntu Server terminal
  - `ll /var/www/html/index.html` (note the owner/group = root, and the mode = 644)
  - `chmod 640 /var/www/html/index.html` (refresh the webpage in Firefox on your Fedora Workstation and note the Forbidden (HTTP 403) error because the Apache daemon doesn't have enough permission to read the web content)
  - `grep www-data /etc/apache2/*` (note that apache daemons are run as the `www-root` user and group) 
  - `chgrp www-data /var/www/html/index.html` (refresh the webpage in your Web browser and note the webpage displays properly because the Apache daemon has read permission to the web content) 
  - `ab -n 1000 -c 100 http://localhost/ `
  - `apt install nginx -y`
  - `grep sites-enabled /etc/nginx/nginx.conf` (note that on Ubuntu, the Nginx configuration refers to files under the /etc/nginx/sites-enabled/ directory)
  - `vi /etc/nginx/sites-enabled/default` (note the same document root of `/var/www/html`, then change `listen 80` to `listen 81` and `listen [::] :80` to `listen [::] :81` and save your changes)
  - `nginx -t` (if you made a syntax error, it will be displayed here)
  - `systemctl restart nginx.service`
  * In Firefox on your Fedora Workstation, navigate to http://UbuntuIPaddress:81 to view your Nginx webpage.
  
  # CUPS Print Server
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `apt install cups -y`
  - `cupsctl --remote-any --remote-admin` (to allow remote web portal access)
  * Open Firefox on your Fedora Workstation and navigate to https://UbuntuIPaddress:631/admin, accept the certificate to continue and log in as `root` and `Secret555` 
  * View the different functionality provided (adding printers, sharing printers, management, classes = printer pools, etc.)
  * If you have a network printer on your local LAN, optionally add it in the CUPS web administration console, explore the management options, and share it to clients - next, on your Fedora Workstation virtual machine or host OS, add the shared printer and print a test page (alternatively, you can add a fake printer and do the same)

## To demonstrate that you can run server services on any Linux system, we'll configure the following three server services (DNS, DHCP, and NTP) on Fedora Workstation!

# DNS
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `dnf install bind -y`
  - `curl https://triosdevelopers.com/jason.eckert/trios/example.com.dns --output /var/named/example.com.dns` 	
  - `cat /var/named/example.com.dns` 
  - `chmod 644 /var/named/example.com.dns` 
  - `curl https://triosdevelopers.com/jason.eckert/trios/named.conf.additions --output named.conf.additions` 
  - `vi /etc/named.conf`
     * Go to the end of the file and type `:r named.conf.additions` 
     * Ensure that the last line (the `forwarders` option for 8.8.8.8) is added to the options block near the top of the file (remove the word `options` when adding it to this block), and then comment this last line 
     * Save your changes and quit vi
  - `systemctl start named.service ; systemctl enable named.service` 
  * Log into the GNOME desktop environment and set a static DNS server of 127.0.0.1 for your network interface in the Network tool
  - `nmcli connection down "name"` (where name is the name of your network interface)
  - `nmcli connection up "name"` (where name is the name of your network interface)
  - `nslookup server1.example.com` (note the successful name resolution)
  - `nslookup google.ca` (this worked because of the forwarder configured)
  - `vi /var/named/example.com.dns` 	(create several resource records of your choice, saving changes)
  - `systemctl restart named.service` 
  - `resolvectl query server1.example.com` (replace server1.example.com with one of the new records you created)
  - `firewall-cmd --add-service dns` (allows DNS in your firewall so that other systems on the network can use your DNS server)
  - `firewall-cmd --add-service dns --permanent` 

# DHCP
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `dnf install dhcpd -y` 
  - `vi /etc/dhcp/dhcpd.conf` (provide an IPv4 configuration for your network using the syntax shown in the slide deck for Session 8)
  - `systemctl start dhcpd.service` 
  - `ps –ef | grep dhcpd` (if dhcpd is not present, you made a typo in dhcpd.conf)
  - `firewall-cmd --add-service dhcp` 
  - `firewall-cmd --add-service dhcp --permanent` 
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `networkctl renew name` (where name is your interface name - if your Fedora Workstation virtual machine is faster than your home router at providing a DHCPOFFER packet to your Ubuntu Server virtual machine, then Ubuntu Server will accept that offer) 
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `cat /var/lib/dhcpd/dhcpd.leases` (note whether your Fedora Workstation dhcpd.service provided an IP lease to your Ubuntu Server)
  - `journalctl | grep -i dhcpd` (even if your Fedora Workstation dhcpd.service did not provide an IP lease to your Ubuntu Server, you should see the DHCPREQUEST and DHCPOFFER packets for your Ubuntu Server listed here)
  - `systemctl stop dhcpd.service` 

# NTP
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `less /etc/chrony.conf` (note the default pool line for NTP servers)
  - `vi /etc/chrony.conf` (add the `allow subnet` line, where subnet is your local IP subnet – e.g. 192.168.1.0/24)
  - `systemctl restart chronyd.service` 
  - `firewall-cmd --add-service ntp` (allows NTP in your firewall, discussed later)
  - `firewall-cmd --add-service ntp --permanent` 
  - `chronyc sources -v` (note the NTP servers queried for time) 
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `apt install ntp -y` (if you get a conflict with systemd-timesyncd, run `apt remove systemd-timesyncd` first)
  - `vi /etc/ntpsec/ntp.conf` (add this line before other pool entries: `pool FedoraIP iburst`)
  - `systemctl restart ntp.service`
  - `ntpq -p` (note your Fedora time server listed in addition to the others) 

# Docker Containers
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `apt install docker.io -y` 
  - `docker search busybox`
  - `docker pull busybox` (pulls the official busybox Linux container image from Docker Hub)
  - `docker images` (you can remove a downloaded image using `docker rmi imagename`)
  - `docker run busybox echo Hello World` (exits upon completion)
  - `docker ps` (note that there are no running containers)
  - `docker ps -a` (note the container run in the past based on the busybox image)
  - `docker run -it busybox sh` (runs a shell in the container for you to interact with)
     - `ls`
     - `ls /etc`
     - `ls /bin`
     - `ps -ef`
     - `exit`
  - `docker ps`
  - `docker ps -a`
  - `docker container prune` (removes all history of stopped containers)
  - `docker search httpd`  
  - `docker pull httpd`  
  - `docker images`
  - `docker run -d -P --name site1 httpd` (-P exposes a port above 32767 on the underlying OS that is mapped to :80 in the container)  
  - `docker port site1`
  - `docker ps`
  * Open a Web browser on your Windows or macOS PC and navigate to http://UbuntuIP:port (where port is the exposed port of your site1 container)
  - `docker run -d -P --name site2 httpd`   
  - `docker port site2`
  - `docker ps`
  - `docker exec -it site2 sh`  
     - `cd htdocs`
     - `ls`
     - `cat index.html`
     - `echo “<html><body><h1>Site 2 works!</h1></body></html>” > index.html`
     - `exit`
  * Open a Web browser on your Windows or macOS PC and navigate to http://UbuntuIP:port (where port is the exposed port of your site2 container)
  - `docker stop site1`
  - `docker stop site2`
  - `docker ps`
  - `docker ps -a`
  - `docker container prune`
  - `docker ps -a`       

# How Many Containers Can We Run?!?
  * To see how many comtainers can be run on a single cloud host (versus VMs), create and execute the following shell script (use Cockpit to monitor resources)
  * For added fun, do this on a Raspberry Pi (instructions on how to install Docker on the Raspberry Pi OS Linux distro, see https://dev.to/elalemanyo/how-to-install-docker-and-docker-compose-on-raspberry-pi-1mo)
```  
  COUNTER=1
  while true
  do
    docker run -d -P --name site$COUNTER httpd
    COUNTER=$(expr $COUNTER + 1)
  done
```  
  * When you are satisfied with the number of containers run, stop the shell script and create and run the following shell script to stop the containers
```  
  COUNTER=1
  while true
  do
    docker stop site$COUNTER 
    COUNTER=$(expr $COUNTER + 1)
  done
```

# Building Containers
  * Developers often start with a pre-built container image, add their Web app to it, and then push that image to Docker Hub 
  * To illustrate this process without creating an actual Web app, we’ll create another container image based on httpd, but with a different webpage, run it locally, and then push it to Docker Hub 
  * First create a free account on Docker Hub (https://hub.docker.com) and follow the instructions to create a free public repository called webapp 
  * Next, create an access token (password equivalent for uploading to your Docker Hub repository) by navigating to Account Settings > Security > New Access Token (you’ll need to copy this token and use it as a password when connecting to Docker Hub later on)
  - `mkdir webappcontainer && cd webappcontainer` 
  - `vi index.html` (add the following contents, saving your changes)
     - `<html><body><h1>This is a custom app!</h1></body></html>`
  - `vi Dockerfile` (add the following contents, saving your changes)
     - `FROM httpd`
     - `COPY ./index.html htdocs/index.html`
  - `docker build -t jasoneckert/webapp .` (replace jasoneckert with your Docker Hub username) 
  - `docker images`  
  - `docker login -u jasoneckert` (paste your Docker Hub access token when prompted)
  - `docker push jasoneckert/webapp:latest`
  - `docker run -d -p 80:80 --name webapp jasoneckert/webapp` 
  * Open a Web browser on your Windows or macOS PC and navigate to http://UbuntuIP
  - `docker --help`
  - `docker run --help`
  - `docker exec --help`
