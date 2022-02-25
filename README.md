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
<summary>Some Linux Helpful Commands</summary>
<!--All you need is a blank line-->

```bash
Linux Commands 
  
$nslookup #for DNS resolution
$ip #to get ip address information 
$ping #to check the reachability 
$netstat 
```

</details>
 
 
