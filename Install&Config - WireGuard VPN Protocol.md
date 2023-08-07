
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
      
      ``` 
      sudo apt-get update -y  
      ```
      or
      
      ``` 
      sudo apt-get upgrade -y 
      ```
        
    **Step 2** -:   Install iptables,   net-tools
     
      ```
      sudo apt-get install iptables net-tools  -y 
      ```
        
    **Step 3** -:   Install WireGuard VPN Software on the System
     
      ```
      sudo apt-get install wireguard -y                 # Ubuntu  
      ``` 
        
      ```       
      sudo yum install wireguard-tools -y             # Redhat
      ```
     
    **Step 4a** -:  Switch to root user profile.
      ```  
      sudo  su   -    
      ```
      
    **Step 4b** -:  Go to /etc/wireguard   **WireGurad directory** 
      ```
      cd   /etc/wireguard/       
      ```
      
    **Step 5** -:   Enable IP Forwarding to route packets between VPN Clients and the Internet
                           
      **Check IP forward on ipv4. If result is 0 set it to 1**
      ```
      cat  /proc/sys/net/ipv4/ip_forward       
      ```
      
      ```
     sudo vi  /etc/sysctl.conf                  # View and Edit the System Config. file
     ```
     
      **uncomment this line  …… OR add  this to the file**
                          
      ```
      net.ipv4.ip_forward=1
      ```
      
     ```
     :wq!               # To save and quite
     ```
     ```
     sudo sysctl -p     # View the set value or uncomment value and reset the system.
     ```
     
    **Step 6a** -:  Create a private and public key pair for the WireGuard Server
      **generate private key through which a publickey is created**
      ```
      wg  genkey | tee privatekey | wg pubkey  > publickey      
      ```

    **Step 6b** -:  Show and Copy Private key 
    ```
    cat privatekey       
    ```
      ![image](https://user-images.githubusercontent.com/113307504/234717642-8a72ea22-facf-456d-92ed-6d408131cbe5.png)
    
    **Step 7** -:   Create a new config file called   wgs0.conf    # ls   to list content     
    ```
    touch wgs0.conf       
    ```
    
      ![image](https://user-images.githubusercontent.com/113307504/234725647-82a1ae62-a3c5-4f77-9d0d-8853f182a2f3.png)


    **Step 8** -:   Open wgs0.conf  and configure the Network Interface as below format.
    ```
    vi wgs0.conf       
    ```
    +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    ()()()()()()()()()()()()()()()()()()()()()()()()()()()()()()()()()()()()
    ```		
    [Interface]
           # A privatekey of the Server you copied ( cat /etc/wireguard/privatekey )
      Privatekey = sDMJiu ……. LhWUY=
           # Address: A private IP address for wg0 interface
      Address = 120.0.0.1/32
           # Saving Configuration File
      SaveConfig =  true

           # NOTE the **enp0s9** must be the public Network Interface of the Server. 
           # Some distors are of name  **eth0**

           # The PostUp will run when the WG Server starts the Virtual VPN tunnel
      PostUp = iptables -A FORWARD -i wg0 -j ACCEPT
      PostUP =  iptables -t nat -A POSTROUTING -o enp0s9 -j MASQUERADE
	
           # The PostDown rules run when the WG Server stops the Virtual VPN tunnel
      PostDown = iptables -D FORWARD -i wg0 -j ACCEPT
      PostDown =  iptables -t nat -D POSTROUTING -o enp0s9 -j MASQUERADE

           # Specify the listening port of WireGuard. This is the defaut.
      ListenPort = 51820
      
    ```
      
           # After the Installation and Configuration at the WG Client side 
                 # we update the Network Interface at the WG Server side 
                     # with below details for each peer(WG client)
    ```
    [Peer]
           # A public key of the Client1 ( cat /etc/wireguard/publickey )
      Publickey =   gF98uIOMNe ……..sDMJiu=
           # The assign IP address of Client1 Network Interface   
      AllowedIPs = 120.0.0.2/32
    ```
    
    ```
    [Peer]
           # A public key of the Client2 ( cat /etc/wireguard/publickey )
      Publickey =    IOMNegFsDM ……..Jiu98u=
           # The assign IP address of Client2 Network Interface   
      AllowedIPs = 120.0.0.3/32

    ```
        
    ()()()()()()()()()()()()()()()()()()()()()()()()()()()()()()()()()()()
    +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    
     ```
     :wq!    # To save and quit 
     ```
    
    
      ![image](https://user-images.githubusercontent.com/113307504/234723824-160757e0-b99d-471a-9957-e84b4b0b410b.png)
     ##### The Endpoint of the Peer are updated accordingly after connection is established.
    
    
          # Enable the WG Server to listen on the WG Clients set default port via 51820 UDP
              # You could set any other port to listen on this Server

    **Step 9** -:   Configure UFW firewall to open WireGuard VPN Server 
    ```
    sudo ufw  status                      # Check status of the UFW  
    ````
    
    ```
    sudo   ufw allow 51820/udp        ## Open the port 51820 on UDP
    ```
    
    ```
    sudo ufw enable                        ## To  enable ufw
    ```
    
    ```
    sudo ufw disable                         ## To disable ufw
    ```

  **Step 10a** -:   Bring up  the Network Interface - wgs0 
    ```
    wg-quick  up  wgs0       # Start the wg0 Interface
    ```
    
    ```
    wg     or    sudo wg          # Display the Interface Details
    ```
    
   ![image](https://user-images.githubusercontent.com/113307504/234724657-6ead729e-be03-4c49-a2b0-ddd6a3389e85.png)

   
   **Step 10b** -:  Put down the Network Interface - wgs0               
    ```
    wg-quick  down wgs0         # To put down wg0 Interface
    ```
    
    ```
    ip link  or  sudo  ip a show wgs0      # Show the Network Interfaces on the Host.
    ```
    

   **Step 11** -:   Check the status. Start and Enable WireGuard Sever. 
                      ## NOTE: PUT  DOWN THE NETWORK INTERFACE  BEFORE EDITTING IT 
    ``` 
    systemctl status wg-quick@wgs0                # Check the status of wg-quick on wgs0                 ```
    
    ```
    systemctl start wg-quick@wgs0.service     # Start wg-quick on wgs0  service
    ```
    
    ```
    systemctl enable wg-quick@wgs0.service        # Enable wg-quick on wgs0 service
    ```
    
    ```
    systemctl restart wg-quick@wgs0.service     # Restart the wg-quick on wgs0 service
    ```
    
    ```
    systemctl stop wg-quick@wgs0.service          # Stop the wq-quick on wgs0 service
    ```
  

## At the VPN Client
   ### Installation Requirement
 - Any OS
 - Update or Upgrade the Package Management

   ### Installation Steps:
    **Step 0** -:   Change the hostname to wgclient1
   
      ```
      sudo hostnamectl set-hostname wgclient1
      ```
      
    **Step 1** -:   Update or  Upgrade the package management
      
      ``` 
      sudo apt-get update -y  
      ```
      or
      
      ``` 
      sudo apt-get upgrade -y 
      ```

      **Step 2** -:   Install iptables,   net-tools
   
       ```
       sudo apt-get install iptables net-tools  -y 
       ```

   
      **Step 3** -:   Install WireGuard VPN Software on the System
   
       ``` 
       sudo apt-get install wireguard -y                 # Ubuntu  
       ```
          or
       ```
       sudo yum install wireguard-tools -y             # Redhat
   
       ```
   
      **Step 4a*** -:  Switch to root user profile.
   
       ```
       sudo  su   -    
       ```

      **Step 4b*** -:  Go to /etc/wireguard   WireGurad directory
   
       ```
       cd   /etc/wireguard/       
       ```

      **Step 5a** -:  Create a private and public key pair for the WireGuard Client
                          # generate private key through which a publickey is created    

       ```
       wg  genkey | tee privatekey | wg pubkey  > publickey      # ls   to list content     
       ```

      **Step 5b** -:  Show and Copy Private key 

       ```
       cat privatekey       
       ```

      ![image](https://github.com/ladio1z/WireGuard-VPN-Config-Guide/assets/113307504/0c4266b1-bc44-459b-b938-d49b76b3d593)

   
   
