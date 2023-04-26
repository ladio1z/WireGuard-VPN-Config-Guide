
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
        
        
        
        
