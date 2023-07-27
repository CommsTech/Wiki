---
title: Microsoft NPS
description: 
published: true
date: 2023-07-26T18:23:12.715Z
tags: Server, Radius, Networking, Microsoft
editor: markdown
dateCreated: 2023-05-22T18:23:12.715Z
---
# Configuring Microsoft NPS for MAC-Based RADIUS - MS Switches

1. Last updated8
    
    Oct 5, 2020
    
2. [Save as PDF](https://documentation.meraki.com/@api/deki/pages/916/pdf/Configuring%2bMicrosoft%2bNPS%2bfor%2bMAC-Based%2bRADIUS%2b-%2bMS%2bSwitches.pdf?stylesheet=default "Export page as a PDF")
 


Network Administrators can use port based access control to prevent unauthorized access to the corporate LAN. MAC-Based RADIUS is one method for providing this type of security. This article discusses the benefits of MAC-Based RADIUS and how to configure it in Microsoft NPS and Dashboard. 

## **Benefits of MAC-Based RADIUS**

In some environments it is critical to control which devices can access the wired LAN. Ports in common areas make a network vulnerable to access by guests and other unauthorized users. MAC-Based RADIUS can be used to provide port based access control on your MS series switches. Unauthorized users are prevented from accessing to the wired LAN because each device that connects to a switch port will need to be authenticated before network access is granted.  Devices are authenticated at the port level with MAC-Based RADIUS. When a device connects to a port with an access policy assigned, before network access is granted, the device must be authenticated by the RADIUS server. The switch (RADIUS client) sends a RADIUS Access-Request to the RADIUS server containing the username and password of the connecting device. The username and password combination is always the MAC address of the connecting device, lower case without delimiting characters. If a RADIUS policy exists on the server that specifies the device should be granted access and the credentials are correct, the RADIUS server will respond with an Access-Accept message. Upon receiving this message, the switch will grant network access to the device on that port. If the RADIUS server replies with an Access-Reject because the device does not match a policy, the switch will not grant network access. It is possible however, to configure the switch to drop devices into a Guest VLAN when they fail to authenticate. The Guest VLAN would provide Internet access only. Below is an example of a basic MAC-Based authentication exchange. 

![5f2f8d40-873e-43df-a9cb-222a2a9d9018](https://documentation.meraki.com/@api/deki/files/3976/5f2f8d40-873e-43df-a9cb-222a2a9d9018?revision=1)

## Adding MS Switches as RADIUS clients on the NPS Server 

 All switches that that need to authenticate connecting devices must be added as RADIUS clients on in NPS.  Below are the steps to add the switches as RADIUS clients. 

1) Open the **NPS Server Console** by going to **Start > Programs > Administrative Tools > Network Policy Server.**

2) In the Left pane, expand the **RADIUS Clients and Servers** option.

3) Right click the **RADIUS Clients** option and select **New.**

4) Enter a **Friendly Name** for the MS Switch.

5) Enter the the **IP Address** of your MS Switch.

6) Create and enter a RADIUS **Shared Secret** (note this secret - we will need to add this to the Dashboard).

7) Press **OK** when finished.

8) Repeat these steps b - g for all switches.  See Figure 1 for a sample RADIUS client configuration. 

**Figure 1.**

![9f08a486-2e9e-407c-8524-b13ea24fd9f8](https://documentation.meraki.com/@api/deki/files/6389/9f08a486-2e9e-407c-8524-b13ea24fd9f8?revision=1)

## **Create a user account in Active Directory for a connecting device.**

1) Open Active Directory Users and Computers: **Start > All Programs > Administrative Tools > Active Directory Users and Computers.** 

2) Create a new user account. the username and password should be the MAC address of the connecting device (letters need to be lower case and it should not have any delimiting characters). See Figure 2 for example user account. 

**Figure 2.**

![888915b6-1ec7-49c1-a533-7d37064ff85b](https://documentation.meraki.com/@api/deki/files/5331/888915b6-1ec7-49c1-a533-7d37064ff85b?revision=1)

## Configuring a NPS Connection Request Policy.

1) In the **NPS Server Console**, navigate to **NPS (Local) > Policies > Connection Request Policies**. 

2) Right click on **Connection Request Policies**, and select **New**.

3) Name the policy and select **Next**.

4) On the Specify Conditions page add the following condition: **NAS port type as Ethernet** (Figure 3) followed by clicking **Next**.

5) Click **Next** on the **Specify Connection Request Forwarding** screen.

6) Click **Next** on the **Specify Authentication Methods** screen.

7) Click **Next** on the **Configure Settings** screen.

8) Review settings and click **Finish** on the **Completing Connection Request Policy Wizard** screen.

**Figure. 3**

![d879c9fd-4362-4e2c-86fe-c9e5597fdf44](https://documentation.meraki.com/@api/deki/files/7270/d879c9fd-4362-4e2c-86fe-c9e5597fdf44?revision=1)

## **Configuring a NPS Network Policy.**

1) In the **NPS Server Console**, navigate to **NPS (Local) > Policies > Network Policies**. 

2) Right click on **Network Policies**, and select **New**.

3) Name the policy and select **Next**. (Figure 4)

**Figure 4.** 

![94c2ea61-91d5-4ac9-adc3-2a29e2213197](https://documentation.meraki.com/@api/deki/files/5895/94c2ea61-91d5-4ac9-adc3-2a29e2213197?revision=1)

4) On the Specify Conditions page add the following two conditions Windows Groups, this can be the group containing especially for the user accounts created in Part 3. See KB [Creating a Windows Group For MAC Based Authentication](https://documentation.meraki.com/MR/Encryption_and_Authentication/Creating_a_Windows_Group_For_MAC_Based_Authentication "MR/Encryption_and_Authentication/Creating_a_Windows_Group_For_MAC_Based_Authentication"). For our example we will use DOMAINNAME\Domain Users. Then specify **NAS port type** **Ethernet** followed by clicking **Next**. (Figure 5)

**Figure 5.** 

![0e0db22e-00b9-4064-85e3-dedca86d8f9c](https://documentation.meraki.com/@api/deki/files/1373/0e0db22e-00b9-4064-85e3-dedca86d8f9c?revision=1)

5) Click **Next** on the **Specify Access Permission** screen.

6) On the **Configure Authentication Methods** page, uncheck all options except **Unencrypted authentication (PAP, SPAP).** (Figure 6)

**Figure 6.** 

![ee8dfb23-58e8-488e-bf46-3a4ca3935a8b](https://documentation.meraki.com/@api/deki/files/7648/ee8dfb23-58e8-488e-bf46-3a4ca3935a8b?revision=1)

7) Click **Next** on the **Configure** **Constraints** screen.

8) Click **Next** on the **Configure** **Settings** screen.

9) Review settings and click **Finish** on the **Completing New Network Policy** screen. (Figure 7)

**Figure 7.**  
![14397e84-3bd5-4f7e-87e2-8a444841afc0](https://documentation.meraki.com/@api/deki/files/2334/14397e84-3bd5-4f7e-87e2-8a444841afc0?revision=1)

## Creating a MAC-Based RADIUS Access Policy in Dashboard.

1) On the Dashboard navigate to **Configure > Access Policies.**

2) Click on the link **Add Access Policy** in the main window then click the link to **Add a server.** 

3) Enter the IP address of the RADIUS server, the port (default is 1812 or 1645), and the secret you created above in part 2. (Figure 8)

4) Click **Save changes.**

**Figure 8.**

![0987654321.PNG](https://documentation.meraki.com/@api/deki/files/425/0987654321.PNG?revision=1&size=bestfit&width=984&height=440)

## Apply Access policy to MS Switchports

1) On the Dashboard navigate to **Configure > Switchports.**

2) Select the port(s) that should have the policy applied.

3) Click the **Edit** button, make sure the port type is Access, and from the **Access policy** drop-down select the policy that was created in part 5. 

(Figure 9)

**Figure 9.**   
![2017-07-12 16_20_58-Switch ports.png](https://documentation.meraki.com/@api/deki/files/3193/2017-07-12_16_20_58-Switch_ports.png?revision=1&size=bestfit&width=495&height=578)



# Dynamic Author
# IEEE 802.1X Authentication and Dynamic VLAN Assignment with NPS Radius Server

Published [25th February 2019](https://www.expertnetworkconsultant.com/2019/02/) by [Samuel O](https://www.expertnetworkconsultant.com/author/encusr/)

- [Share](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fwww.expertnetworkconsultant.com%2Fconfiguring%2Fieee-802-1x-authentication-and-dynamic-vlan-assignment-with-nps-radius-server%2F&t=IEEE%20802.1X%20Authentication%20and%20Dynamic%20VLAN%20Assignment%20with%20NPS%20Radius%20Server)
- [Tweet](https://twitter.com/intent/tweet?text=IEEE%20802.1X%20Authentication%20and%20Dynamic%20VLAN%20Assignment%20with%20NPS%20Radius%20Server&url=https%3A%2F%2Fwww.expertnetworkconsultant.com%2Fconfiguring%2Fieee-802-1x-authentication-and-dynamic-vlan-assignment-with-nps-radius-server%2F&via=expertnetworkco)
- [Pin](https://www.expertnetworkconsultant.com/configuring/ieee-802-1x-authentication-and-dynamic-vlan-assignment-with-nps-radius-server/#)

0SHARES

IEEE 802.1X Authentication and Dynamic VLAN Assignment with NPS Radius Server is an important element to networking in the real world. User location cannot be predicted as they may be at and out of a desk and up and about should they need to do so. Tying them to a local VLAN may only be helpful if they are bound to desks in those locations, although the most ideal outcome, it is not the most practical.

It is only wise to incorporate IEEE 802.1X Authentication and Dynamic VLAN Assignment with NPS Radius Server in areas where you expect different teams to come to. Meeting rooms could for a moment have the accounting group or the development group meeting there and based on the intelligent and dynamic vlan assignmnet with 802.1x authentication, users port-access are defined their appropriate vlans for their respective access to resources on the network.

**Open Lounge with IEEE 802.1X Authentication with Dynamic VLAN for all users to function as if they were at their own desks**

**How to Provision 802.1 X Authentication Step By Step With Dynamic VLAN Assignment With Windows Radius Server For 802.1x Clients.**

A typical configuration for a system under IEEE 802.1x Authentication control is shown in the following figure.

[![How to Provision 802.1 X Authentication Step By Step With Dynamic VLAN Assignment With Windows Radius Server For 802.1x Clients](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/How-to-Provision-802.1-X-Authentication-Step-By-Step-With-Dynamic-VLAN-Assignment-With-Windows-Radius-Server-For-802.1x-Clients.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/How-to-Provision-802.1-X-Authentication-Step-By-Step-With-Dynamic-VLAN-Assignment-With-Windows-Radius-Server-For-802.1x-Clients.png)

In this scenario, “Lady Smith” wishes to use services offered by servers on the LAN behind the switch. There are multiple VLANs with resources available based on user vlan membership. Her laptop computer is connected to a port on the Aruba 2920 Edge Switch that has 802.1x port authentication control enabled.

The laptop computer must therefore act in a supplicant role. Message exchanges take place between the supplicant and the authenticator which is the Aruba 2920 Switch, and the authenticator passes the supplicant’s credentials which is her (Windows Active Directory User Account Credentials) to the authentication server for verification. The NPS Server which is the authentication server then informs the authenticator whether or not the authentication attempt succeeded, at which point “Lady Smith” is either granted or denied access to the LAN behind the switch.

[![Dynamically Assign VLANS to Users using 802.1x Wired EAP-TLS](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Dynamically-Assign-VLANS-to-Users-using-802.1x-Wired-EAP-TLS.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Dynamically-Assign-VLANS-to-Users-using-802.1x-Wired-EAP-TLS.png)

**Setup Structure for IEEE 802.1X Authentication and Dynamic VLAN Assignment with NPS Radius Server**

1. Supplicant: Laptop running Microsoft Windows 10 or Windows 7
2. Authenticator: HP Aruba 2920 Edge Switch
3. Authentication Server: Microsoft NPS (Network Policy Server) running on Windows Server 2012 R2.
4. User Database : Active Directory

### For Windows Infrastructure

1. Create NPS Server – Add Role on Windows Server 2012 R2
2. Create DHCP Scopes for VLANS
3. Create RADIUS Client on NAC using Network Policy Server
4. Create Network Policies
5. Configure a Network Policy for VLANs
6. Start Wired Auto-Config Service
7. Enable Network Authentication

**Create NPS Server – Add Role on Windows Server 2012 R2**

[![Add the Network Policy and Access Services Server Role for Dynamic VLAN Assignment with NPS Radius Server](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Add-the-Network-Policy-and-Access-Services-Server-Role.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Add-the-Network-Policy-and-Access-Services-Server-Role.png)  
The Network Policy and Access Services allows you to define and enforce policies for network access authentication, authorisation, and client health using Network Policy Server(NPS), Health registration Authority(HRA), and Host Authorisation Protocol(HCAP).

**Create the DHCP Scopes for VLAN100 and VLAN200 Groups**

- Development Group Scope – VLAN 100

SVI: ip address 172.16.80.254 255.255.255.0  
Scope Subnet: 172.16.80.1/24

- Accounting Group Scope – VLAN 200

SVI:ip address 172.16.70.254 255.255.255.0  
Scope Subnet: 172.16.70.0/24

[![Create the DHCP Scopes for VLAN100 and VLAN200 Groups](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-the-DHCP-Scopes-for-VLAN100-and-VLAN200-Groups.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-the-DHCP-Scopes-for-VLAN100-and-VLAN200-Groups.png)

**Create RADIUS Client on NAC using Network Policy Server**

[![Create New Radius Client to Specify the Network Access to your Network](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-New-Radius-Client-to-Specify-the-Network-Access-to-your-Network.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-New-Radius-Client-to-Specify-the-Network-Access-to-your-Network.png)

**Secret Key:**secret12

**Add Edge Switch Management IP as the RADIUS Client**

[![Add Edge Switch Management IP as the RADIUS Client for Dynamic VLAN Assignment with NPS Radius Server](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Add-Edge-Switch-Management-IP-as-the-RADIUS-Client.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Add-Edge-Switch-Management-IP-as-the-RADIUS-Client.png)

The Shared Secret Key: **secret12** will be used in the Switch Configuration.

**Create Network Policies for the Development Group and Accounting Department** – **Repeat same steps for the Accounting Department**  
[![Create Network Policies for Departments](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policies-for-Departments.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policies-for-Departments.png)

**Create Network Policy for Accounting Group for VLAN 200**  
[![Create Network Policy for Accounting Group for VLAN 200](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policy-for-Accounting-Group-for-VLAN-200.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policy-for-Accounting-Group-for-VLAN-200.png)

**Create Network Policy Conditions for Accounting Group for VLAN 200**  
[![Create Network Policy Conditions for Accounting Group for VLAN 200](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policy-Conditions-for-Accounting-Group-for-VLAN-200.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policy-Conditions-for-Accounting-Group-for-VLAN-200.png)

[![Create Network Policy Conditions for Accounting Group for VLAN 200](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policy-Conditions-for-Accounting-Group-for-VLAN-200-1.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policy-Conditions-for-Accounting-Group-for-VLAN-200-1.png)

**Create Network Policy Constraints for Accounting Group for VLAN 200**  
[![Create Network Policy Constraints for Accounting Group for VLAN 200](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policy-Constraints-for-Accounting-Group-for-VLAN-200.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policy-Constraints-for-Accounting-Group-for-VLAN-200.png)

**Create Network Policy Settings for Accounting Group for VLAN 200**

[![Create Network Policy Settings for Accounting Group for VLAN 200](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policy-Settings-for-Accounting-Group-for-VLAN-200-1.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policy-Settings-for-Accounting-Group-for-VLAN-200-1.png)

**Configuration Example**

Here’s an example of how you might consider when configuring Microsoft NPS Server to assign users to a VLAN based on their user group, using NPS for the authentication and authorization of users. This configuration has worked flawlessly on the HP Aruba 2920 Switch. The key to getting this to work is the use of a RADIUS element called: ‘Tunnel-PVT-Group-ID’. This is a RADIUS attribute that may be passed back to the authenticator (i.e. the Aruba 2920 Switch) by the authentication server (i.e. Microsoft NPS Server) when a successful authentication has been achieved. There are a few other elements which need to accompany it, but this is the key element, as it specifies the VLAN number that the user should be assigned to.

The other elements that need to be returned by the NPS Server are as follows:

- Tunnel-PVT-Group-ID: 200
- Service-Type: Framed
- Tunnel-Type: VLAN
- Tunnel-Medium-Type: 802

**Create Network Policy Settings forTunnel-PVT-Group-ID for VLAN 200**  
[![Create Network Policy Settings forTunnel-PVT-Group-ID for VLAN 200](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policy-Settings-forTunnel-PVT-Group-ID-for-VLAN-200.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policy-Settings-forTunnel-PVT-Group-ID-for-VLAN-200.png)

**Create Network Policy Settings for Tunnel-Medium-Type for VLAN 200**  
[![Create Network Policy Settings for Tunnel-Medium-Type for VLAN 200](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policy-Settings-for-Tunnel-Medium-Type-for-VLAN-200.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policy-Settings-for-Tunnel-Medium-Type-for-VLAN-200.png)

**Create Network Policy Settings for Tunnel-Type for VLAN 200**  
[![Create Network Policy Settings for Tunnel-Type for VLAN 200](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policy-Settings-for-Tunnel-Type-for-VLAN-200.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Create-Network-Policy-Settings-for-Tunnel-Type-for-VLAN-200.png)

### For Client Infrastructure

**On the Supplicant, Windows 7 or 10 configure the following steps on the Ethernet Adapter to enable IEEE 802.1X Authentication**

**Configure Ethernet Authentication on Windows 7 or Windows 10 Operating System**  
[![Enable WLAN AutoConfig](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Enable-WLAN-AutoConfig.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Enable-WLAN-AutoConfig.png)

[![Ethernet Adapter Configuration](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Ethernet-Adapter-Configuration.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Ethernet-Adapter-Configuration.png)

**Enable IEEE 802.1X Authentication**  
[![Ethernet Adapter IEEE 802.1X Authentication](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Ethernet-Adapter-IEEE-802.1X-Authentication.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/Ethernet-Adapter-IEEE-802.1X-Authentication.png)  
**IEEE 802.1X Authentication – Advanced Settings**  
[![IEEE 802.1X Authentication - Advanced Settings](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/IEEE-802.1X-Authentication-Advanced-Settings.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/IEEE-802.1X-Authentication-Advanced-Settings.png)  
**IEEE 802.1X Authentication – Protected EAP Properties**  
[![IEEE 802.1X Authentication - Protected EAP Properties](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/IEEE-802.1X-Authentication-Protected-EAP-Properties.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/IEEE-802.1X-Authentication-Protected-EAP-Properties.png)

**IEEE 802.1X Authentication EAP-MSCHAPv2 Properties**  
[![](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/IEEE-802.1X-Authentication-EAP-MSCHAPv2-Properties.png)](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/IEEE-802.1X-Authentication-EAP-MSCHAPv2-Properties.png)

### For Network Infrastructure

**Connect Server Infrastructure to VLAN 400**

vlan 400
   name "Server Infrastructure"
   untagged 47-48
   ip address 10.10.10.1 255.255.255.0
   exit

**Create VLAN for Accounting Group**

vlan 200
   name "Accounting Group"
   ip address 172.16.70.254 255.255.255.0
   ip helper-address 10.10.10.40
   exit

**Create VLAN for Development Group**

vlan 100
   name "Development Group"
   ip address 172.16.80.254 255.255.255.0
   ip helper-address 10.10.10.40
   exit

**Create AAA Configuration on Switch for Radius Authentication**

hostname "Edge Switch Aruba 2920"
radius-server host 10.10.10.10 key "secret12"
aaa authentication port-access eap-radius
aaa port-access authenticator 1-24
aaa port-access authenticator active

**Download the Switch Configuration:**

**[802.1 x wireless authentication step by step - Download the 802.1 x wired authentication step by step configuration sample](http://www.expertnetworkconsultant.com/wp-content/uploads/2019/02/802.1-x-wireless-authentication-step-by-step.txt)**

### Test the IEEE 802.1X Authentication and Dynamic VLAN Assignment with NPS Radius Server

Verify Port-Access with the following user groups – VLAN 100 and VLAN 200

MacAuth(config)# show port-access authenticator

 Port Access Authenticator Status

  Port-access authenticator activated [No] : Yes
  Allow RADIUS-assigned dynamic (GVRP) VLANs [No] : No

       Auths/  Unauth  Untagged Tagged           % In  RADIUS Cntrl
  Port Guests  Clients VLAN     VLANs  Port COS  Limit ACL    Dir   Port Mode
  ---- ------- ------- -------- ------ --------- ----- ------ ----- ----------
  **1    1/0     0       200      No     No        No    No     both  1000FDx**
  2    0/0     0       None     No     No        No    No     both  1000FDx
  3    0/0     0       None     No     No        No    No     both  1000FDx
  4    0/0     0       None     No     No        No    No     both  1000FDx
  5    0/0     0       None     No     No        No    No     both  1000FDx
  6    0/0     0       None     No     No        No    No     both  1000FDx
  7    0/0     1       None     No     No        No    No     both  1000FDx
  8    0/0     0       None     No     No        No    No     both  1000FDx
  9    0/0     0       None     No     No        No    No     both  1000FDx
  10   0/0     0       None     No     No        No    No     both  1000FDx
  11   0/0     0       None     No     No        No    No     both  1000FDx
  12   0/0     0       None     No     No        No    No     both  1000FDx

MacAuth(config)# show port-access authenticator

 Port Access Authenticator Status

  Port-access authenticator activated [No] : Yes
  Allow RADIUS-assigned dynamic (GVRP) VLANs [No] : No

       Auths/  Unauth  Untagged Tagged           % In  RADIUS Cntrl
  Port Guests  Clients VLAN     VLANs  Port COS  Limit ACL    Dir   Port Mode
  ---- ------- ------- -------- ------ --------- ----- ------ ----- ----------
  1    0/0     0       None     No     No        No    No     both  1000FDx
  2    0/0     0       None     No     No        No    No     both  1000FDx
  3    0/0     0       None     No     No        No    No     both  1000FDx
  4    0/0     0       None     No     No        No    No     both  1000FDx
  5    0/0     0       None     No     No        No    No     both  1000FDx
  6    0/0     0       None     No     No        No    No     both  1000FDx
  **7    1/0     0        100    No        No    No     No     both  1000FDx**
  8    0/0     0       None     No     No        No    No     both  1000FDx
  9    0/0     0       None     No     No        No    No     both  1000FDx
  10   0/0     0       None     No     No        No    No     both  1000FDx
  11   0/0     0       None     No     No        No    No     both  1000FDx
  12   0/0     0       None     No     No        No    No     both  1000FDx

Think of what other clever things you can do from the information below;

**Breakdown of Commands for RADIUS Authentication**

#Define authentication host and pre-shared key.
radius-server host 10.10.10.10 key "SpecifiedSharedSecretKey"

#Enable processing of Disconnect and Change of Authorization messages from authentication server
radius-server host 10.10.10.10 dyn-authorization

#Set selected authentication mode
aaa authentication port-access eap-radius

#Configure specified ports for authentication
aaa port-access authenticator 1-24

#Assign authenticated client VLAN to authenticator ports
aaa port-access authenticator 1-24 auth-vid 200

#Assign unauthenticated client VLAN to authenticator ports
aaa port-access authenticator 1-24 unauth-vid 999

#Activate authentication on assigned ports with configured options
aaa port-access authenticator active

exit

**Verification Commands**

VERIFICATION
A number of CLI commands are available to verify authentication server and port access configuration, including:

show port-access authenticator [port-list] [config | statistics | session-counters | vlan | clients [detailed]]
show authentication
show radius authentication
show radius [host IP]

Thanks for reading. Please share your thoughts in the comment box below;