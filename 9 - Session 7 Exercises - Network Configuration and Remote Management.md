# Network Interfaces & Services
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `ifconfig` (note the IP address you received from DHCP)
  - `ip addr show`
  - `ethtool -S`
  - `nmcli conn show`  
  - `ls /etc/NetworkManager/system-connections` (note that the folder is empty)
  * Log into GNOME desktop as the woot user and navigate to Activities > Show Applications > Settings > Network 
  * Next, modify your network interface to use a static configuration for your network and Apply your changes 
  - `ls /etc/NetworkManager/system-connections` (note the name.nmconnection file)
  - `cat /etc/NetworkManager/system-connections/name.nmconnection` (use the `[Tab]` completion feature!)
  - `nmcli connection down "name" ; nmcli connection up "name"` (use the `[Tab]` completion feature!)
  - `nmcli` (note the new IP address for future exercises)
  - `ping -c5 google.ca` (note that it will use IPv6 by default if your system has an IPv6 configuration)
  - `ping -4 -c5 google.ca` (forces IPv4 ping) 
  - `dnf install iftop iperf3 -y` 
  - `iftop` (`[Ctrl]+c` to quit) 
  - `iperf3 -s` (this starts an iperf3 server running on port 5201)
  - Open a new terminal as root
  - `iperf3 -c 127.0.0.1` (connects to the local iperf3 server and measures bandwidth) 
  - `exit` 
  - Return to your previous terminal where the iperf3 server is running and press `[Ctrl]+c` to quit 
  - `hostnamectl set-hostname fedoraworkstation` 
  - `hostname`
  - `cat /etc/hostname`
  - `bash` (new shells will indicate the new hostname)
  - `cat /etc/resolv.conf` (note the redirection to Systemd-resolved, which listens on 127.0.0.53)
  - `resolvectl status`
  - `vi /etc/hosts`	(add the line below)
     - `1.2.3.4  fakehost.fakedomain.com  fakehost`
  - `host fakehost.fakedomain.com`
  - `resolvectl query fakehost.fakedomain.com` 
  - `nslookup fakehost.fakedomain.com`
  - `dig fakehost.fakedomain.com`
  - `host google.ca`
  - `resolvectl query google.ca`
  - `nslookup google.ca`
  - `dig google.ca ANY`
  - `ip route` (note the two routes for your network and gateway)
  - `traceroute google.ca` (note that it uses IPv6 by default if your system has an IPv6 configuration)
  - `traceroute -4 google.ca` (forces IPv4 traceroute)
  - `tracepath -4 google.ca`
  - `mtr -4 google.ca` (press `q` to quit)
  - `grep -i telnet /etc/services`
  - `nmap -sT localhost` (note the default services started, and the ports they listen on)
  - `dnf install telnet-server telnet`
  - `systemctl start telnet.socket ; systemctl enable telnet.socket`
  - `nmap -sT localhost` (note the telnet service listening to port 23/tcp, on demand)
  - `telnet localhost` (log in as the root user)
     - `who` (note that you are logged in remotely via a pseudo terminal, pts/0)
     - `ss -t` (note the established telnet socket on your system – after initially connecting on port 23/tcp, communication is redirected to a random port above 32768 for the session duration)
     - `exit` (quits your telnet session to localhost)
  - `who` (note that your pseudo terminal is no longer present)
  - `ss -t` (note that your telnet socket is no longer present)
  - `firewall-cmd --add-service telnet` (this allows telnet in the Fedora Workstation firewall)
  - `firewall-cmd --add-service telnet --permanent` (this allows telnet in the Fedora Workstation firewall at boot)
  * If your host is Windows, install the Putty program (`winget install putty.putty`), open it from the Start menu when finished and start a telnet session to the IP address of your Fedora Workstation. Log in as root, run `who` and then `exit`.
  * If your host OS is macOS, navigate to Applications > Utilities > Terminal to open a shell on your local system. Run the `telnet FedoraWorkstationIPaddress` command, log in as root, run `who` and then `exit`.
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `ip addr show`
  - `networkctl`
  - `cat /etc/netplan/50-cloud-init.yaml`
  - `hostname`
  - `cat /etc/hostname`
  - `ping -c 5 google.ca`
  - `nmap -sT localhost` (we’ll be adding several additional services later)

# SSH
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `ssh woot@UbuntuIPaddress` (accept the SSH host key and supply the woot user’s password when prompted)
     - `su -` (supply the root user’s password when prompted)
     - `who` (note that you are connected via a remote pseudo terminal session) 
     - `exit`
     - `exit`
  - `ssh-keygen` (press `[Enter]` at each prompt to use default settings)
  - `ssh-copy-id woot@UbuntuIPaddress`
  - `ssh woot@UbuntuIPaddress` (note that you are not prompted to supply a password!)
     - `exit`
  - `ssh woot@UbuntuIPaddress cat /etc/hosts > downloadedhosts`
  - `cat downloadedhosts`
  * If your host OS is Windows, open Putty from the start menu and start an SSH session to the IP address of your Ubuntu Server. Log in as woot, run `su -` to switch to root, and then run `who`. Run `exit` to switch back to woot, and `exit` again to close your SSH session.
  * If your host OS is macOS, navigate to Applications > Utilities > Terminal to open a shell on your local system. Run `ssh woot@UbuntuIPaddress` (accept the SSH host key and supply woot’s password when prompted). Next, run `su -` to switch to root, and then run `who`. Run `exit` to switch back to woot, and `exit` again to close your SSH session.
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `systemctl start sshd.service ; systemctl enable sshd.service` 
  - `firewall-cmd --list-all` (note that there is a default exception for SSH already!)
  - `ssh -X woot@localhost` (accept the SSH host key and supply the woot user’s password when prompted)
    - `who` (note that you are connected via a remote pseudo terminal session) 
    - `nm-connection-editor &` (note that the graphics are sent through the SSH tunnel - close the program when finished) 
    - `exit`

# Cockpit
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `dnf install cockpit -y` 
  - `systemctl start cockpit.socket ; systemctl enable cockpit.socket` 
  - `firewall-cmd --add-service cockpit` 
  - `firewall-cmd --add-service cockpit --permanent` 
  * On your host OS, open a web browser and navigate to https://FedoraWorkstationIP:9090. Accept the certificate name warning and log in as woot. Click Limited Access and supply your password to enable sudo priviledges (=root access). Browse the options available and close your web browser when finished.
  * Normally, you would enable Cockpit on servers (e.g., Ubuntu Server), but because we are going to configure many additional networking services on our Ubuntu Server during the next lab - and we only wish to demonstrate Cockpit in this lab - we'll stick to enabling it only on Fedora Workstation.
