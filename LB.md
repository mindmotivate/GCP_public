
## Instance Templates in Google Cloud Platform (GCP)
If you dont have a VPC go ahead and create one first

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
   - Enter a name for the subnet, e.g., "us-east1-subnet".
   - Choose "us-central1" as the region.
   - Specify a subnet IP range that does not overlap with other subnets, such as "10.1.0.0/24"
   - Select "On" for Private Google Access
   - Select "On" for Flow Logs
   - Select "Off" for Hybrid subnet


## Firewall Rules:
. **Create Firewall Rules:**
   - While still in the "VPC networks" section, click on "Firewall rules".
   - 
   - You will select "edit" next to "application-vpe-allow-custom"

   - Choose "Specified protocals and ports"
   - Select "TCP"
   - Type Port: "80"


## Instance Templates in Google Cloud Platform (GCP)

Instance Templates in GCP are predefined configurations for VM instances, specifying properties like machine type, boot disk image, and network settings. 
They ensure consistency and simplify the creation of multiple VMs with identical configurations. Instance Templates are crucial for Managed Instance Groups (MIGs), 
enabling automated instance creation, scaling, and updates. They also facilitate integration with load balancers, ensuring efficient traffic distribution and reliable application performance.

Let's create an instance template!
(GCP has some ready made templplates?)
name: gcp-instance-template

Machine Configuration:
e2-medium

Network Interface:
Network: gpc-application-vpc (previously created)
Subnetwork: us-east1-subnet IPV4 10.1.0.0.24 (previously created)

IP Stack: Ipv4

External IP Address: Ephemeral

Network Service Tier: Premium

Managemnet:
Automation: Enter your startup script here

After all selections have been made click "Create":





## Managed Instance Groups (MIGs)
Managed Instance Groups are collections of virtual machines (VMs) that are managed as a single entity, allowing for automated instance creation, deletion, and updates.
Integration with Load Balancers: **MIGs can be integrated with load balancers to automatically manage the instances based on the traffic load and health checks.**

Let's create an instance group!

name: gcp-instance-group
(copy this instance group name into the description)

instance template: 
(select the instance template that you previoulsy created)

Location:
select "single zone"

select a region and a zone for your MIG
region: us-east1
zone: us-east1-b

Autoscaling: On
Minimum # of instances: 3
Maximum # of instances: 10

Autoscaling Signals:CPU Utilization 60% (default)

Note: GCP provides the ability to set autscaling schedules which can be very helpful if you are running a retail site with an expectation of heavy traffic during s certian time of day

Initialization period:  (explain)

VM Instance Lifecycle:
Action on failure: Repair Instances

Auto-Healing:
load-balanced-vms-autohealing-health-check (HTTP)  *default*

Initial Delay: 300 (similar to aws)

Updates during VM Instance Repair
*Keep the same instance configuration

Port Mapping:
Send traffic to a NAMED port 
Port name: http / Port Numbers: 80

After selections have been made click: "Create"

******************
If you need to create a health check from scrtach follow this criteria:

Go to dashboard and type in: "health check"
select create health check

Name: load-balancer-health-check
Scope: regional
Region: 
Protocal: TCP / Port: 80
*****************


## Create Load Balancer (LB)

*Ensure that you are in the correct project"
Select "Create Load Balancer"
Type of Load Balancer: Application
Public Facing or Internal: Public Facing  (for next armageddon we will do "internal" load balancer)
Global or single-region deployment: Single Region
Click Create Load Balancer

Load Balancer Name: gcp-load-balancer
Region: us-east1
Network: select your previously created VPC (or choose default)

Frontend Configuration
Name: lb-frontend
Description
Protacal: HTTP
Port: 80
Ip Address: Ephemral

Reserve Subnet
Name: Proxy-01
Region: us-east1
Ip region: 10.1.0.0/24

Note: In Google Cloud Platform (GCP), a proxy-only subnet is a specialized subnet configuration designed for use with Google Cloud's Internal HTTP(S) Load Balancing. This type of subnet is used to manage and direct internal traffic within a Virtual Private Cloud (VPC) network without exposing it to the internet.
-If you have created a VPC previously and want to attach your internal HTTP(S) load balancer to it, you can certainly create a proxy-only subnet within that existing VPC.
-Existing subnets and their configurations will remain unchanged.
-You can also create a VPCwoth no subnets if you choose and simply use the proxy subnet creation feature
-You may also use the default subnet feature also


Backend Configuration
Name: lb-backend
Description
Protacal: HTTP
Port: 80
Ip Address: Ephemral






