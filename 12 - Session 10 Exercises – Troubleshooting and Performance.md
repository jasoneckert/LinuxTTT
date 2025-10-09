# Troubleshooting
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `grep -i bluetooth /var/log/messages` 
   - `journalctl --since "08:00" | grep -i bluetooth` (Is the output the same? Experiment pasting some of these events into Google/AI)
   - `journalctl -xe --unit bluetooth` (note the output is more readable, scroll right to read long lines)
   - `top` (press Shift+`M` to sort by memory to detect memory leaks, Shift+`P` to sort by CPU to detect rogues)
   - `systemd-analyze blame`
   - `df -i` (is the inode table near its limit for the root filesystem?)
   - `umount /private ; fsck -f /dev/mapper/private ; mount /private`
   - `ping -c 4 localhost`
   - `ping -c 4 gateway` (where gateway is your default gateway)
   - `ping -c 4 8.8.8.8`
   - `ping -c 4 -4 dns.google`
   - `systemctl start httpd.service`
   - `ps -ef | grep httpd`
   - `nmap -sT localhost` 
   - `curl http://localhost`
   - `firewall-cmd --list-all`
   - `firewall-cmd --add-service http`
   - `firewall-cmd --add-service http --permanent` (in a web browser on your host OS, navigate to `http://FedoraWorkstationIP`)

# Performance
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `dnf install sysstat iotop ioping`
   - `systemctl enable sysstat.service --now` (this starts it now as well as enables it at boot time)
   - `systemctl start sysstat-collect.service`
   - `systemctl enable sysstat-collect.timer --now`
   - `systemctl start sysstat-summary.service`
   - `systemctl enable sysstat-summary.timer --now`
   - `systemctl start sysstat-rotate.service`
   - `systemctl enable sysstat-rotate.timer --now`
   - `mpstat 2 5`
   - `pidstat`
   - `iostat`
   - `vmstat`
   - `free`
   - `sar -u 2 5`
   - `sar -q 2 5`
   - `sar -d 2 5`
   - `sar -f /var/log/sa/saX` (where X is the day of the month – this will work once time passes and events are collected!)
   - `top`
   - `iotop`
   - `ioping /dev/sda`
   - `iftop`
   - `uptime`
   - `tload` (use `[Ctrl]+c` to quit)
   - `w` 
