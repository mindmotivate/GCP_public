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
  ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/608fcefc-7dbc-4421-84a9-48d7b86b1413)
* Notice how this custom rule goes to both subnets
   56:34
  - select "application-vpe-allow-custom"
  - - Note: In a normal production environment, you will NOT use icmp: "PING"
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

   - 
     51:45
