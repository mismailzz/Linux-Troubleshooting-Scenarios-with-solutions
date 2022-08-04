# Linux-Troubleshooting-Scenarios-with-solutions
Linux Troubleshooting Scenarios based question with solution approach 

Its crucial to understand the problem statement before proceeding for any action. The first action should not be to rush out to server side and try to troubleshoot the issue. The first approach would be is to try to understand the issue and ask the user/client follow up questions to understand the problem properly. Its often happen that the issues are on the client/user side who reported it and lot of the time get spend into understand or even become impossible to get it resolved. So don't preassume that your client/user would be technical and whatever the shared information is 100% right. We have to ask the questions to build the facts for the formation of right direction. 

In the below cases, we tried to expand and understand the problem statement. As it would provide us the clear crystal clear image to brain that whats happening and in which direction we have to move forward. 

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
<summary>Can't cd to the directory? Even if the user has the sudo privileges.</summary>
<!--All you need is a blank line-->

    . 
    ├── Reasons and Resolution
    │   ├── Directory does not exist
    │   ├── Pathname conflict: relative vs absolute path
    │   ├── Parent directory permission/ownership
    │   ├── Doesn't have executable permission on target directory
    │   ├── Hidden directory
    └── ...

</details>


<details>
<summary>Can't openfile or run script?</summary>
<!--All you need is a blank line-->

    . 
    ├── Reasons and Resolution
    │   ├── Target directory/File does not exist
    │   ├── Pathname conflict: relative vs absolute path
    │   ├── Parent directory permission/ownership
    │   ├── Target file permission/ownership and have the executable
    │   ├── Hidden directory/file
    └── ...

</details>
   

<details>
<summary>Can't create links?</summary>
<!--All you need is a blank line-->

    . 
    ├── Reasons and Resolution
    │   ├── Target directory/File does not exist
    │   ├── Pathname conflict: relative vs absolute path - (should be complete path)
    │   ├── Parent directory permission/ownership 
    │   ├── Target file permission/ownership - (as there should be read permission)
    │   ├── Hidden directory/file
    └── ...

</details>

These above reasons and resolution can be mapped on other issues like can't execute command, can't view/write file etc.


<details>
<summary>Running Out of Memory?</summary>
<!--All you need is a blank line-->

    . 
    ├── Types
    │   ├── Cache (L1, L2, L3)
    │   ├── RAM
    │   │   ├── Usage
    │   │   │   ├── #free -h
    │   │   │   │   ├── Total (Total assigned memory)
    │   │   │   │   ├── Used (Total actual used memory)
    │   │   │   │   ├── Free (Actual free memory)
    │   │   │   │   ├── Shared (Shared Memory)
    │   │   │   │   ├── Buff/Cache (Pages cache memory)
    │   │   │   │   ├── Available (Memory can be freed)
    │   │   │   ├── /proc/meminfo
    │   │   │   │   ├── file active
    │   │   │   │   ├── file inactive
    │   │   │   │   ├── anon active 
    │   │   │   │   ├── anon inactive
    │   ├── Swap (Virtual Memory)
    ├── Resolution
    │   ├── Identify the processes that are using high memory using top, htop, ps etc.
    │   ├── Check the OOM in logs and also check if there is a memory commitment in sysctl.conf
    │   ├── Kill or restart the process/service
    │   ├── prioritize the process using nice 
    │   ├── Add/Extend the swap space 
    │   ├── Add more physical more RAM
    └── ...

</details>


<details>
<summary>Add/Extend the Swap space</summary>
<!--All you need is a blank line-->

    . 
    ├── Due to running out of memory, we would need to add more swap space
    │   ├── Create a file with #dd, as it will reserve the blocks of disk for swap file
    │   ├── Set permission 600 and give root ownership
    │   ├── #mkswap
    │   ├── Now Turned swap on #swapon
    │   ├── fstab entry for persistent
    └── ...

</details>


<details>
<summary>Can't run certains commands</summary>
<!--All you need is a blank line-->

    . 
    ├── Troubleshooting and Resolution
    │   ├── command
    │   │   ├── Could be the system related command which non root user does not have the access
    │   │   ├── Could be the user defined script/command
    │   ├── Troubleshooting
    │   │   ├── permission/ownership of the command/script
    │   │   ├── sudo permission
    │   │   ├── absolute/relative path of command/script
    │   │   ├── not defined in user $PATH variable
    │   │   ├── command is not installed 
    │   │   ├── command library is missing or deleted
    └── ...

</details>

    
<details>
<summary>System unexpectedly reboot and process restart?</summary>
<!--All you need is a blank line-->

    . 
    ├── Troubleshooting and Resolution
    │   ├── System reboot/crash reasons
    │   │   ├── CPU stress
    │   │   ├── RAM stress
    │   │   ├── Kernel fault
    │   │   ├── Hardware fault
    │   ├── Process restart
    │   │   ├── System reboot
    │   │   ├── Restart itself
    │   │   ├── Watchdog application 
    │   │   │   ├── To prevent high stress on system resources 
    │   │   │   ├── If application causing stress, so it will restart or terminate
    │   ├── Troubleshooting
    │   │   ├── After logged in, check the status by using commands like uptime, top, dmesg, journalctl, iostat -xz 1
    │   │   ├── syslog.log, boot.log, dmesg, messages.log etc 
    │   │   ├── custom log path of applicatoin
    │   │   ├── if not completely accessible, so take the virutal console like from ILO, IDRAC etc
    │   │   ├── open a case and reach out a vendor
    └── ...

</details>

 
<details>
<summary>Unable to get the IP Address</summary>
<!--All you need is a blank line-->

    . 
    ├── IP Assignment Methods
    │   ├── DHCP
    │   │   ├── Fixed Allocation
    │   │   ├── Dynamic Allocation
    │   ├── Static
    ├── Troubleshooting
    │   ├── check network setting from virtualization environment like VMware, VirtualBox or etc
    │   ├── check the IP address is assigned or not
    │   ├── check the NIC status from host side using #lspci, #nmcli etc
    │   ├── restart network service 
    └── ...

</details>


<details>
<summary>IP Address is assigned but not reachable</summary>
<!--All you need is a blank line-->

    . 
    ├── Troubleshooting
    │   ├── check the physical network connectivity, configured switch interface
    │   ├── check the NIC status from software/hardware level using #lspci, #nmcli, #mii-tool etc
    │   ├── check gateway, netmask, and ping the gateway to verify outbound traffic
    │   ├── check the network routes and service 
    │   ├── check the IP Address duplication 
    │   ├── check the firewall access on the host and from the Network Team
    └── ...

</details>


<details>
<summary>How to back up and restore file permissions on Linux</summary>
<!--All you need is a blank line-->

    . 
    ├── Troubleshooting
    │   ├── The best option is to create the ACL file of Dir/Files before changing the permissions in bulk
    │   │   ├── Create the acl file before changing the permission (or backup the file permission): ~$ getfacl -R <dir> > permissions.acl    
    │   │   ├── Restore File Permissions: ~$ setfacl --restore=permissions.acl
    │   ├── Restore from the VM Snapshot (But not always a good option for production)
    │   ├── Rebuild the VM (this option is safe for future)
    └── ...

</details>
    
    
<details>
<summary>HTTP error 403: forbidden yum occurs when we try to install a package using yum</summary>
<i>The HTTP 403 Forbidden response status code indicates that the server understands the request but refuses to authorize it. The access is permanently forbidden and tied to the application logic, such as insufficient rights to a resource. [Ref](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/403 , https://bobcares.com/blog/http-error-403-forbidden-yum/)</i>
<!--All you need is a blank line-->

    . 
    ├── There could be some major causes during installing pkg from yum
    │   ├── Network configuration
    │   │   ├── If you are using proxy server
    │   │   │   ├── check the proxy setting in the /etc/yum.conf as its effective rather than using env variable #export https_proxy=https://<ip/hostname>:<port>
    │   │   │   │   ├── If the proxy server is valid
    │   │   │   │   │   ├── Check the ACL on the proxy server
    │   │   ├── Other
    │   ├── A corrupt repo
    │   ├── Permission of packages
    │   │   ├── This option most ofently not used because we linux manages by itself so avoid to proceed for this option. The use of this option irresponsibly could make the system unstable
    │   ├── Selinux issue
    │   ├── firewalld rules
    │   ├── Other
    └── ...

</details>
 

<details>
<summary>Useful tips related disk partition</summary>
<!--All you need is a blank line-->

    . 
    ├── Tips
    │   ├── After adding/attaching a new disk to a VM, we can get its status from lsblk command by doing ~$echo 1 > /sys/block/sda/device/rescan
    │   ├── If we increase disk size of existing disk than the additional space get appended to the existing disk without affecting the already existed FileSystem and Partition    
    │   ├── We can also recreate the filesystem on block device as it will automatically format the old one
    │   ├── If we have a disk(with created partition/FS) we can share the .vmdk to other VM. So after mounting we would have a same data as it was on previous one.
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
$free -h
$cat /proc/meminfo
$env    
```

</details>
 
 
