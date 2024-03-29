# Tutorial: Setting Up Managed Instance Groups with Load Balancer

1. **Create a New Project:**
   - Go to Google Cloud Console (console.cloud.google.com).
   - Click on the project dropdown menu at the top.
   - Select "New Project" from the dropdown.
   - Enter the project name as "webapp-load-balancer" and click "Create".
   - 
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/9541751b-0639-4ae3-b07f-3e2ac0ca9a4e)


2. **Set Up Billing for the Project (if required):** OPTIONAL STEP
   - If prompted, set up billing for the newly created project to enable usage of Google Cloud resources.

3. **Enable Necessary APIs:**
   - Go to "APIs & Services" > "Dashboard" in the Cloud Console.
   - Click on "+ ENABLE APIS AND SERVICES" at the top.
   - Search for "Compute Engine API", "Cloud Load Balancing API", and "Cloud DNS API".
   - Click on each API and then click "Enable" to enable them for the project.
   - 
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/6a9f65b5-d907-44e3-bef8-f5b40a861fa3)

4. **Create a VPC Network:**
   - Navigate to "VPC network" > "VPC networks" in the Cloud Console.
   - 
   - ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/1f516bb8-5973-463a-af6b-e94253ef93c3)

   - Click on "+ CREATE VPC NETWORK" at the top.
   - 
  ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/1421f44c-46e1-49b2-9348-03cc1fae7402)

   - Enter the name as "application-vpc".
   - Select "Custom" for subnet creation mode.
  
### Quick Note On Subnets:
RFC 1918 defines the following private IP address ranges for internal networks:
10.0.0.0 to 10.255.255.255
172.16.0.0 to 172.31.255.255
192.168.0.0 to 192.168.255.255
These address blocks are not routable on the public internet, meaning devices with these addresses cannot be directly accessed from the internet. 
This is a key benefit for network security and allows for efficient use of IP addresses within private networks.

Use ipconfig (bash) to find your IP address for your machine (It should start with 192 for private network)
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/8a213795-2a5f-4d6d-8478-6276bf458684)

In GCP we will use: 10.101-254.0.0/24


5. **Create Subnet for "us-west1" Region:**
   - Click on "+ ADD SUBNET".
   - Enter a name for the subnet, e.g., "public-webapp1".
   - Choose "us-central1" as the region.
   - Specify a subnet IP range that does not overlap with other subnets, such as "10.237.1.0/24"
   - Select "On" for Private Google Access
   - Select "On" for Flow Logs
   - Select "Off" for Hybrid subnet
   - 
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/cfb702a4-6052-43d5-8e22-dd01d7e0d177)


5. **Create Subnet for "europe-west1" Region:**
   - Click on "+ ADD SUBNET".
   - Enter a name for the subnet, e.g., "public-webapp2".
   - Choose "us-central1" as the region.
   - Specify a subnet IP range that does not overlap with other subnets, such as "10.237.2.0/24"
   - Select "On" for Private Google Access
   - Select "On" for Flow Logs
   - Select "Off" for Hybrid subnet

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/c1e454ba-fa61-4456-8384-0d194b8c8e87)

### You now have established 2 subnets in TWO DIFFERENT regions (very unique to GCP!)


## Firewall Rules:
7. **Create Firewall Rules:**
   - While still in the "VPC networks" section, click on "Firewall rules".
   - 
   - You will select "edit" next to "application-vpe-allow-custom"
    ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/8757c039-c86a-4c11-86df-91e7e9f811be)

   - Choose "Specified protocals and ports"
   - Select "TCP"
   - Type Port: "80"
  ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/608fcefc-7dbc-4421-84a9-48d7b86b1413)
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/792a9a2f-8892-41cb-9796-ebe86721eb20)
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/7cc8dfb7-4c3a-4863-81d6-f65a9ad7f855)

* Notice how this custom rule goes to both subnets
   56:34
  - select "application-vpe-allow-custom"
  - - Note: In a normal production environment, you will NOT use icmp: "PING" (Ping can be used by hackers to discover hosts on a network)
   - select "application-vpe-allow-rdp"
   - select "application-vpe-allow-ssh"
 
   - Note: Your deny rule will block all other incoming traffic (you cannot edit this rule)
   - Note: Your allow rule will allow all egress traffic (you cannot edit this rule) DON'T TOUCH OUTBOUND RULES
   - Note: Typically Theo does his DENYS first (but google is setup differently)
59.57
57:40

   - Choose your dynamic routing option: select "Regional"
   - PRAY. then press "create"
  ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/e9fbc611-89e4-469f-a558-316d2874f4a5)

   - After you have created your VPC, naviage to Firewall Rules and click "Create Firewall Rules"
   - ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/c48aee9f-575e-4320-a2fa-7cbe0ea979dc)
   - We will create a seperate rule for 443
   - Name; "secure-443"
   - Description: "Secure Connection"
   - Logs: "On"
   - for "Direction of Traffic" choose: "Ingress"
   - NEVER TOUCH EGRESS
   - Targets: "Specified target tags"
   - Target tags: "default"
   - Add Source IPV4 ranges: 10.237.1.0/24
   - ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/46787fe4-948d-478a-9122-5c1e8c592d48)

   - Note: refer to your document to ensure youhave the correct Ip ranges
   - Note: for the purposes of this turorial we are only applying the "secure-443" rule to one of our Ip ranges
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/9b0974d0-b755-4d9d-a4f4-760f63a6ad0f)
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/79b7f342-8a49-4542-88a8-d93b9b05843b)
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/c4385494-61b0-4a26-ba2e-f6b893f73e29)

   -Click "Create"

  Now that our rule has been createed, lets say you decide you ant to remove "icmp"
  You can simply click on the icmp rule and choose "delete firewall rule"
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/d1cf4fee-5077-48e2-92ca-38628cac6232)
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/edd033a7-6dcf-48b5-8224-4b555e563735)

### Next, we will create a VM with a user startup script

8. **Create VM Instance with Startup Script:**
   - Navigate to "Compute Engine" > "VM Instances" in the Cloud Console.
   - Click on "+ CREATE INSTANCE" at the top.
  ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/12d4e15a-95dd-4cd2-be54-67418242b4c8)

   - 
   - Name the instance as "webapp-instance1".

- Ensure that you select the VPC you created and that you choose the subnet in the correct region

-DO NOT CHECK FIREWALL OPTIONS
-REMOVE THE DEFAULT NETWORK INTERFACES IF THEY ARE SELECTED (THIS WILL CAUSE YOU TO HAVE A NIC CREATE DNINTHE DEFAULT VPC)
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/89b428b0-92d8-4c17-8544-e19a5d7b85e3)
-ENSURE THAT YOU ARE SELCTINH THE CORRECT VPC AN P HERE:
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/093ffa99-d28a-4074-a481-d3f322c31986)



-Scroll down to the "Management" section and paste your startup script in the Automation text box
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/e2680e03-043a-447f-8158-d5ac72bcb480)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/ffbb143d-4853-4fee-928f-e0bff465a5ff)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/79eb1039-23bb-4ac9-940a-cc27d4e7a1dc)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/bd0b1301-08b4-4c55-a8c6-250b0b7f590d)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/5a1e8e7d-d711-421c-875b-482749af4161)

Note: This instance will use the firewall from the VPC
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/f0f5fb47-b57d-4a54-b0b1-8886dee65089)

-Create

