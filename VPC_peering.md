- **VPC Communication and Peering**:
  - **Scenario**: 
    - Two VPCs (VPC-1 and VPC-2) with resources.
    - They can be part of the same project, different projects, or even different organizations.
  - **Communication Requirement**:
    - Internal IP addresses within a VPC are used for communication between resources within the same VPC.
    - However, internal IP addresses cannot be directly used for communication between resources in different VPCs.
  - **Solution: Peering**:
    - Peering allows communication between resources in different VPCs without going through the public internet.
    - It establishes a direct connection between the VPC networks.
  - **Key Points**:
    - Peering does not require external IP addresses.
    - There's no concept of central management, unlike in Shared VPC.
    - It facilitates communication between VPCs using internal IP addresses, ensuring private network communication.

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/0afd1d40-a593-4c5f-854c-901cbe4d4080)

- **Scenario**:
  - Organization has two projects:
    - Project 1: Contains VPCs 1 and 2.
    - Project 2: Contains VPCs 3 and 4.
   
           Organization
             /   \
        Project1  Project2
       /   \         /   \
    VPC1  VPC2    VPC3  VPC4

  - Separate organizational node 2 with projects 3 and 4:
    - Project 3: Contains VPCs 5 and 6.
    - Project 4: Contains VPCs 7 and 8.

         Organization
            /      \
   Project 3        Project 4
    /     \           /     \
  VPC5   VPC6     VPC7   VPC8

  
- **Subscenarios**:
  1. **VPCs within the same project communicate using VPC peering**:
     - VPCs 1 and 2 within Project 1 can communicate using VPC peering.
     - VPCs 3 and 4 within Project 2 can communicate using VPC peering.

 
         Project1
       /        \
     VPC1 ----- VPC2

         Project2
       /        \
     VPC3 ----- VPC4

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/6e6de7cb-69de-44b2-bb74-d5faa8752188)

  2. **VPCs from different projects within the same organizational node communicate using VPC peering**:
     - VPCs 5 and 7 within Organization2 can communicate using VPC peering.
    
  ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/00621909-bf41-4e84-995a-669f288f5a79)


  3. **VPCs from different organizational nodes communicate using VPC peering**:
     - VPCs 4 and 6, belonging to different organizational nodes (Project 2 and Project 3 respectively), can communicate using VPC peering.
     - 
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/9c98e421-1176-4eb6-86ec-05fd6829cfb0)





## Lab Setup Guide

### 1. Setup Projects:
- Go to the cloud platform console.
- Create a new project named "myfirstproject".
- Create another project named "network-pse".

### 2. Enable Compute Engine:
- In both projects, navigate to the Compute Engine section.
- If not already enabled, click "Enable" to activate Compute Engine services.

### 3. Create Custom VPCs and Subnets:
- In "myfirstproject":
- ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/91d91334-1888-4b36-ac08-69aaee949cb5)

  - Navigate to the VPC network section.
  - ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/68123a52-257c-4ab7-8062-f04a33eb147b)

### Create VPC Network
Click on 'CREATE VPC NETWORK' near the top of the page. You will go to the creation page.
Name your VPC and give it a description (optional)
Under 'VPC network ULA internal IPv6 range' section, leave that 'disabled'
Under 'Subnets' section, leave it checked on custom.

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/2b0e951e-9acd-4c1e-a111-143573ecc134)

  -![image](https://github.com/mindmotivate/GCP_private/assets/130941970/5deca9df-b8e4-4e09-a01f-036eef0f07db)
.
  - ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/507d1ae7-95bb-46ec-b11b-4413d01fc368)

  - Create a subnet named "asia-northeast1" with the range "10.133.1.0/24".

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/c93da024-6986-47a7-94b3-9dcfe5a8c3dc)

Under 'Firewall rules' section, there is a list of rules as depicted below. Let's check the 'allow-icmp' to allow your network to return pings. Check the 'allow-ssh' to allow an SSH connection to your network.
Under 'Dynamic routing mode', leave it checked on 'Regional' since we are using a single region for our VPC and VM.
Click on 'Create'
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/8d153956-850a-4d30-b9ec-49b5ce6a3c69)


Do not touch the 'Create Secondary IPV4 Range' button.
Under 'Private Google Access', click 'On'.
Under 'Flow logs', click 'On'. We don't have to configure logs
Under 'Hybrid subnet'. leave that checked at 'Off'.

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/a2c6ad49-dff8-48b3-bfe8-38f90b66617f)




### Creating Custom VPCs and Subnets:

1. **Navigate to the VPC Network Section:**
    - Access the "network-pse" project.
    - Go to the VPC network section.

2### Creating Custom VPCs and Subnets

1. **Navigate to the VPC Network Section:**
    - Access the "network-pse" project.
    - Go to the VPC network section.

2. **Create VPC Network:**
    - Locate and click on 'CREATE VPC NETWORK' near the top of the page to initiate VPC creation.
    - Name your VPC as "custom-vpc" and provide an optional description.
    - Ensure the 'VPC network ULA internal IPv6 range' section remains disabled.
    - Keep the 'Subnets' section set to custom.

3. **Create Subnets:**
    - Create a subnet named "us-central1" with the range "10.136.1.0/24".

4. **Set Firewall Rules:**
    - Configure firewall rules according to your network security requirements.

5. **Set Dynamic Routing Mode:**
    - Choose the appropriate dynamic routing mode based on your network setup.

6. **Final Configuration:**
    - Review the settings and click on 'Create'.
    - Avoid interactions with the 'Create Secondary IPV4 Range' button unless necessary.
    - Ensure 'Private Google Access' is turned 'On' if required.
    - Activate 'Flow logs' if needed for monitoring purposes.
    - Adjust 'Hybrid subnet' settings as necessary.

Ensure that each step is performed accurately to create the "custom-vpc" within the "network-pse" project.



### 4. Create VMs:
- In "myfirstproject":
  - Go to Compute Engine > VM Instances.
  - Click "Create Instance".
  - Name the VM "dns-public".
  - Choose the custom VPC "my-new-vpc".
  - Configure other settings as needed.
- In "network-pse":
  - Repeat the process to create a custom VM.

### 5. SSH into VMs:
- Open a terminal window or use Cloud Shell.
- SSH into the "dns-public" VM using its external IP address.

- SSH into the custom VM in "network-pse" project using its external IP.

### 6. Test Connectivity:
- Clear screens for both SSH sessions.
- Obtain the IP addresses of both VMs using `ifconfig` or `ip addr show`.
- Ping each other's VM using their IP addresses.

### 7. Create Network Peering Connection:
- In "myfirstproject":
- Go to VPC network > VPC network peering.
- Click "Create Peering".
- Name the peering connection "P1-TO-P2".
- Enter the correct network name of "custom-vpc" VPC.
- In "network-pse":
- Go to VPC network > VPC network peering.
- Click "Create Peering".
- Name the peering connection "P2-TO-P1".
- Enter the correct network name of "my-new-vpc" VPC.

### 8. Verify Connectivity:
- Return to SSH sessions.
- Retry the ping command between VMs.
- Ensure that the connection works fine.

### 9. Check Peering Connection Status:
- Go to the cloud platform console.
- Navigate to VPC network > VPC network peering in both projects.
- Verify that the peering connections are active.

https://www.udemy.com/course/google-cloud-gcp-professional-cloud-security-engineer-certification/learn/lecture/27623582#overview
