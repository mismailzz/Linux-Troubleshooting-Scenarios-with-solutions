# Linux-Troubleshooting-Scenarios-with-solutions
Linux Troubleshooting Scenarios based question with solutions

Its crucial to understand the problem statement before proceeding to any action. 

<details>
<summary>Server is not reachable or cannot connect</summary>
<!--All you need is a blank line-->

    . 
    ├── Ping the server by Hostname and IP Address
    │   ├── Hostname/IP Address is pingable
    │   │   ├── Issue might be on the client side as server is reachable
    │   ├── Hostname is not pingable but IP Address is pingable
    │   │   ├── Could be the DNS issue
    │   │   │   ├── check /etc/hosts 
    │   │   │   ├── check /etc/resolv.conf 
    │   │   │   ├── check /etc/nsswitch.conf
    │   │   │   ├── (Optional) DNS can also be defined in the /etc/sysconfig/network-scripts/ifcfg-<interface>
    │   ├── Hostname/IP Address both are not pingable
    │   │   ├── Check the other server on its same network to see if there is Network side access issue or other overall something bad
    │   │   │   ├── False: Issue is not overall network side but its with that host/server
    │   │   │   ├── True: Might be overall network side issue
    │   │   ├── Logged into server by Virtual Console, if the server is PoweredON. Check the uptime
    │   │   ├── Check if the server has the IP, and has UP status of Network interface
    │   │   │   ├── (Optional) Also check IP related information from /etc/sysconfig/network-scripts/ifcfg-<interface>
    │   │   ├── Ping the gateway, also check routes
    │   │   ├── Check Selinux, Firewall rules
    │   │   ├── Check physical cable conn
    └── ...
   
</details>


<details>
<summary>Cannot connect to a website or application</summary>
<!--All you need is a blank line-->

    . 
    ├── Ping the server by Hostname and IP Address
    │   ├── False: Above Troublshooting Diagram "Server is not reachable or cannot connect"
    │   ├── True: Check the service availabilty by using telnet command with port
    │   │   ├── True: Service is running 
    │   │   ├── False: Service is not reachable or running 
    │   │   │   ├── Check the service status using systemctl or other command 
    │   │   │   ├── Check the firewall/selinux
    │   │   │   ├── Check the service logs
    │   │   │   ├── Check the service configuration
    └── ...

</details>

    
<details>
<summary>Cannot SSH as root/user</summary>
<!--All you need is a blank line-->

    . 
    ├── Ping the server by Hostname and IP Address
    │   ├── False: Above Troublshooting Diagram "Server is not reachable or cannot connect"
    │   ├── True: Check the service availabilty by using telnet command with port
    │   │   ├── True: Service is running 
    │   │   │   ├── Issue migh be on client side
    │   │   │   ├── User might be disabled, nologin shell, disabled root login and other configuration
    │   │   ├── False: Service is not reachable or running 
    │   │   │   ├── Check the service status using systemctl or other command 
    │   │   │   ├── Check the firewall/selinux
    │   │   │   ├── Check the service logs
    │   │   │   ├── Check the service configuration
    └── ...

</details>

 

<details>
<summary>Disk Space is full issue or add/extend disk space</summary>
<!--All you need is a blank line-->

    . 
    ├── System Performance degradation detection
    │   ├── Application getting slow/unresponsive
    │   ├── Commands are not running (For Example: as / disk space is full)
    │   ├── Cannot do logging and other etc
    ├── Analyse the issue
    │   ├── df command to find the problematic filesystem space issue
    ├── Action
    │   ├── After finding the specific filesystem, use du command in that filesystem to get which files/directories are large
    │   ├── Compress/remove big files
    │   ├── Move the items to another partition/server
    │   ├── Check the health status of the disks using badblocks command (For Example: #badblocks -v /dev/sda)
    │   ├── Check which process is IO Bound (using iostat)
    │   ├── Create a link to file/dir
    ├── New disk addition
    │   ├── Simple partition
    │   │   ├── Add disk to VM
    │   │   ├── Check the new disk with df/lsblk command
    │   │   ├── fdisk to create partition. Better to have LVM partition
    │   │   ├── Create filesytem and mount it
    │   │   ├── fstab entry for persistent
    │   ├── LVM Partition
    │   │   ├── Add disk to VM
    │   │   ├── Check the new disk with df/lsblk command
    │   │   ├── fdisk to create LVM partition
    │   │   ├── PV, VG, LV
    │   │   ├── Create filesytem and mount it
    │   │   ├── fstab entry for persistent
    │   ├── Extend LVM partition
    │   │   ├── Add disk, and create LVM partition 
    │   │   ├── Add LVM partition (PV) in existing VG
    │   │   ├── Extend LV and resize filesystem
    └── ...

</details>

    
<details>
<summary>Filesystem get corrupted</summary>
<!--All you need is a blank line-->

    . 
    ├── check /var/log/messages, dmesg and other log files
    ├── if we have a badsector logs, we have to run fsck
    │   ├── True: 
    │   │   ├── run fsck on block device not on the mountpoint and make sure its not mouted. As busy device error would occur
    │   │   ├── if its on / and we can’t umount it as it contain OS
    │   │   ├── reboot the system into resuce mode as booting it from CDROM by applying ISO
    │   │   ├── proceed to continue the shell and run fsck, disk repair without mounting it 
    └── ...

</details>

<details>
<summary>fstab file missing or bad entry</summary>
<!--All you need is a blank line-->

    . 
    ├── One of the error that cause the system unable to BOOT UP 
    ├── Check /var/log/messages, dmesg and other log files
    ├── If we have a badsector logs, we have to run fsck
    │   ├── True: 
    │   │   ├── reboot the system into resuce mode as booting it from CDROM by applying ISO
    │   │   ├── proceed with option 1, which mount the original root filesystem under /mnt/sysimage
    │   │   ├── edit fstab entries or create a new file with the help of blkid and reboot
    └── ...

</details>

    
<details>
<summary>Some Linux Helpful Commands</summary>
<!--All you need is a blank line-->

```bash
Linux Commands 
  
$nslookup #for DNS resolution
$ip #to get ip address information 
$ping #to check the reachability 
$netstat #show network information like port binding, routes etc.
$df #to get the overall disk usage
$du
$badblocks
$iostat
$lsblk
$fdisk
$pvcreate
$vgcreate
$lvcreate
$vgextend
$lvextend
$dmesg    
$blkid
```

</details>
 
 
