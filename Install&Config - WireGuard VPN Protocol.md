
# A GUIDE TO THE INSTALLATION AND CONFIGURATION OF WIREGUARD VPN 

#### A VPN is a secure/encrypt tunnel between your device and a secure Server, then sending your traffic through it. 

#### A Tunnel is a medium created between two devices/nodes through which traffic flow to and from. A Secure Tunnel is a Tunnel with only a secure data/traffic exchange between the nodes.

#### The driving force (ie Protocol) creating each encrypted tunnel, which determines that tunnel’s shape and sizes. WireGuard is just one of the several different types of protocols used to create these tunnels.

#### WireGuard is a communication protocol and free and open-source software that implements encrypted virtual private network (VPN), and was designed with the goal of easy use, high speed performance, and low attack surface. WireGuard cryptographic primitives may be faster and it is built into the Linux Kernel.


### The below are the procedure for the installation and configuration of a WireGuard VPN between a VPN Server and a Client.

## At the VPN Server
   ### Installation Requirement
 - Any OS
 - Update or Upgrade the Package Management
 - Open port **51820** (WireGuard VPN default)

   ### Installation Steps:
    **Step 0** -:   Change the hostname to wgserver
      ```
      sudo hostnamectl set-hostname wgserv
      ```
      
    **Step 1** -:   Update or  Upgrade the package management
      
      ``` Update
      sudo apt-get update -y  
      ```
      or
      
      ``` Upgrade
      sudo apt-get upgrade -y 
      ```
        
    **Step 2** -:   Install iptables,   net-tools
     
      ```
      sudo apt-get install iptables net-tools  -y 
      ```
        
    **Step 3** -:   Install WireGuard VPN Software on the System
     
      ``` Ubuntu
      sudo apt-get install wireguard -y                 # Ubuntu  
      ``` 
        
      ``` Redhat       
      sudo yum install wireguard-tools -y             # Redhat
      ```
     
    **Step 4a** -:  Switch to root user profile.
           sudo  su   -    
     
    **Step 4b** -:  Go to /etc/wireguard   WireGurad directory 
                         cd   /etc/wireguard/       

    **Step 5** -:   Enable IP Forwarding to route packets between VPN Clients and the Internet
                           # Check IP forward on ipv4. If result is 0 set it to 1
                                 cat  /proc/sys/net/ipv4/ip_forward       
                       
                       sudo vi  /etc/sysctl.conf                  # View and Edit the System Config. file
                                uncomment this line  ……     net.ipv4.ip_forward=1
                                net.ipv4.ip_forward = 1     #   or add  this to the file
                                        :wq!               # To save and quite
                        sudo sysctl -p     # View the set value or uncomment value and reset the system.

     
        
        
        
