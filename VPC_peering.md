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
  - Navigate to the VPC network section.
  - Create a new VPC named "my-new-vpc" with the range "10.133.0.0/16".
  - Create a subnet named "asia-northeast1" with the range "10.133.1.0/24".
- In "network-pse":
  - Create another custom VPC named "custom-vpc" with the range "10.136.0.0/16".
  - Create a subnet named "us-central1" with the range "10.136.1.0/24".

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
