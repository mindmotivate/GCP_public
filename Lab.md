# LAB - Static IP

### Select project
1. Open the Google Cloud Platform (GCP) console.
2. Select the desired project.

<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/64bfc3e7-aece-4ffc-8eb2-9ab62d301188" width="50%" height="50%">



### Open menu bar
3. Open the menu bar on the left-hand side of the console.

### Go to networking section
4. Navigate to the "Networking" section.

### Click on VPC network
5. Click on "VPC network" to manage Virtual Private Cloud (VPC) resources.

<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/456d6170-9da3-475a-b20b-b30e857f42ca" width="20%" height="20%">

### View default network details
6. Locate and click on the "default" network to reveal its details.
<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/3082c6d7-3121-42a2-98e4-562293a86a79" width="30%" height="30%">
<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/1cfbc3fd-a80f-4734-a1cf-ccb7b9977e9b" width="50%" height="50%">



### Subnet creation mode: auto-subnets
7. Note the subnet creation mode, which should be set to "auto-subnets".

### Dynamic routing mode - regional
8. Check the dynamic routing mode, which should be set to "regional".

### DNS policy - none
9. Verify the DNS policy, which should be set to "none".

### View provided subnets and their regions
10. Observe the list of provided subnets and their respective regions.
<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/6f2ac4a4-00e1-4c27-aa42-ff8c4203d316" width="50%" height="50%">
<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/353465dd-386d-4c4b-ab4b-dcc3b028a31b" width="100%" height="100%">

### Check for static IP addresses
11. Note that there are currently no static IP addresses assigned.
<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/23bbe377-f58d-4b7c-8b0d-355db46588d6" width="50%" height="50%">

### View Firewall rules, Routes, Network erring, and Private service connection.
<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/e0881e79-bf56-433b-a8f3-9cdccf1389c0" width="50%" height="50%">

### CREATE A STATIC IP
12. Click on "External IP addresses" (none should exist at this time).
13. Choose the option to "reserve static address".
14. Click "reserve external address".
<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/63800aa3-4a5d-49a0-a7eb-a3518ebb478b" width="70%" height="70%">
<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/387a42b2-5a63-4df9-aa6e-4118c0a39eae" width="50%" height="50%">

### Name your static IP address
15. Provide a name for your static IP address.

### Choose network service tier
16. Select the network service tier.

### IP version: ipv4
17. Ensure that the IP version is set to IPv4.

### Type: regional
18. If choosing regional type, select a specific region. (In this case we will use "us-central1(iowa)")

<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/134a3b0b-0b5c-43c9-b947-9da8f0b19869" width="50%" height="50%">

### Click Reserve and Wait for static IP creation
19. Wait for the static IP address to be created.

<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/7c3a7dde-5d6d-4634-b47c-9f5302e4e7de" width="50%" height="50%">

### Create a second static IP (global this time)

<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/ecab351f-a90e-431f-83d3-69dc646f7ffd" width="50%" height="50%">
<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/96b015d0-b6bb-4a5f-8f48-ef7b42e6c6c7" width="50%" height="50%">

### Wait for the static IP address to be created.
<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/5ec2fe65-feac-49b4-bf0d-0556ecd2cc16" width="50%" height="50%">

### Compute Engine
21. Navigate to Compute Engine.

<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/cfd95cbc-ee38-4c5b-876d-54104e723bb3" width="50%" height="50%">

### Create a VM instance
22. Name your instance. (we will name it instance-1)
<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/de9e8da1-746c-45f6-92c3-35931ddaefa9" width="50%" height="50%">
<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/2d7971d3-65e4-4a53-9595-a288d45674e7" width="50%" height="50%">

### Networking section
23. Under networking settings:
    - Network tags: optional
    - Networking interfaces: default
    - Choose the default subnet.
<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/c8aa4800-cdd8-4e8d-b6b8-3f3063e03795" width="50%" height="50%">
<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/2849c5e6-ab8b-4b65-9461-99f386cc5c8b" width="50%" height="50%">

### Choose internal IP type (we will choose ephemral automatic)
24. Select one of the following for primary internal IP:
    - Ephemeral automatic
    - Ephemeral custom
    - Reserve static internal IP address (will cost more)

For External Ip choose "none"

### Click "create" and wait for creation
25. Wait for its creation.
<img src="https://github.com/mindmotivate/GCP_private/assets/130941970/ec162812-6c92-4f79-a062-4217a4f65c9c" width="50%" height="50%">

### Create another instance
26. Name your instance. (we will name it instance-2)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/2980d2a8-4b42-4b4d-8233-e135a4d8b44b)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/13e1bd1a-c8cd-4603-bdba-af6e9eeac2eb)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/927784ef-62c4-476d-9925-c055aac08b61)



### Create another instance

Create VM Instance**: Name the VM instance "instance-3" and select the desired zone (in this case, "us-central1-a").

4. **Networking Settings**: Under networking settings, choose the default subnet and networking interfaces.

5. **Internal IP Assignment**: Select the "Ephemeral automatic" option for the primary internal IP address.

6. **External IP Assignment**: Assign the external IP address named "static-ip-1" (35.208.169.144) to the instance.

7. **Finalization**: Click "Create" and wait for the VM instance to be created.


***You should see the follwing in the console:***

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/ea2f91f4-0a2c-4878-b458-6916e47e083d)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/e08462ee-c7e8-4f57-9ce0-25b76b1e65a2)



| IP Name        | IP Address     | Creation Method   | Selection in Console | Type     | Instance    |
|----------------|----------------|-------------------|----------------------|----------|-------------|
| static-ip-1    | 35.208.169.144 | Manual Reservation| Static               | Static   | instance-3  |
| static-ip-2    | 34.117.134.57  | Manual Reservation| Static               | Static   | Unassigned  |
| unnamed-ip     | 35.209.191.158 | Automatic         | Ephemeral(automatic) | Ephemeral| instance-2  |
| Not assigned   | N/A            | N/A               | N/A                  | N/A      | instance-1  |



# A tale of instances in the Lab...

## instance-1

Initially, the instance did not have any specific IP address assigned. It is running in the us-central1-a zone and currently has an internal IP address of 10.128.0.2. (Internal IP addresses for VM instances in Google Cloud Platform (GCP) are automatically assigned by GCP)


## instance-2

Initially, the instance did not have any specific IP address assigned. It is running in the us-central1-a zone and currently has an internal IP address of 10.128.0.3 and an external IP address of 35.209.191.158. (Internal IP addresses for VM instances in Google Cloud Platform (GCP) are automatically assigned by GCP)

## instance-3

Initially, the instance did not have any specific IP address assigned. It is running in the us-central1-a zone and currently has an internal IP address of 10.128.0.4 and an external IP address of 35.208.169.144. (Internal IP addresses for VM instances in Google Cloud Platform (GCP) are automatically assigned by GCP)

## The Results:
### In simple terms, you are seeing the following:

1. **instance-1**: This instance is running in the us-central1-a zone and has an internal IP address of 10.128.0.2. It doesn't have an external IP address assigned. It is within the same VPC network.

2. **instance-2**: Another instance also running in the us-central1-a zone. It has an internal IP address of 10.128.0.3 and an external IP address of 35.209.191.158. This instance is directly accessible from the internet.

3. **instance-3**: This instance, also in the us-central1-a zone, has an internal IP address of 10.128.0.4 and an external IP address of 35.208.169.144. This instance is directly accessible from the internet.

***Note: External IP addresses are not mandatory for every VM instance because not all instances require direct access from the internet. 
 **Internal Communication**: Many VM instances within a cloud environment need to communicate with each other or with other resources within the same Virtual Private Cloud (VPC) network. In such cases, internal IP addresses are sufficient for communication, and external IP addresses are unnecessary.
**Security Considerations**: Exposing VM instances directly to the internet through external IP addresses can introduce security risks if proper security measures are not implemented. Therefore, in scenarios where direct internet access is not required, it's often recommended to avoid assigning external IP  **Cost Optimization**: External IP addresses may incur additional costs, especially if they are reserved or used for extended periods. By not assigning external IP addresses to every VM instance, organizations can optimize costs, especially for instances that do not require internet connectivity.***

# A tale of two IP addresses.....

## IP Address 1
- Initially, no specific IP address was manually assigned or reserved, indicated by "none."
- A static IP address was manually created and reserved by the user within the Google Cloud Platform (GCP) console.
- This static IP address was then manually assigned to a specific virtual machine (VM) instance within the GCP environment.

## IP Address 2
- Initially, no specific IP address was manually assigned or reserved, indicated by "none."
- A static IP address was manually created and reserved by the user within the Google Cloud Platform (GCP) console.
- This static IP address was not assigned to any specific resource within the GCP environment.


## The Results:
### In simple terms you are seeing the following:

1. **static-ip-1**: This is a fixed address (35.208.169.144) that anyone can use to access something in Google's cloud in the central region. It's like having a permanent phone number for a specific place. Currently, it's being used by a computer named "instance-3."

2. **static-ip-2**: Another fixed address (34.117.134.57) available for anyone to use, but it's not currently assigned to anything. It's reserved, like a phone number not yet connected to a phone.

3. **Unnamed IP**: This address (35.209.191.158) is also available for anyone to use, but it's temporary. It's currently being used by a computer named "instance-2" in the central region. Think of it like a phone number that changes each time you make a call from a specific phone.

Nomenclature reminder:
- **Static**: Fixed address that doesn't change.
- **Ephemeral**: Temporary address that may change over time.



![image](https://github.com/mindmotivate/GCP_private/assets/130941970/098d82d2-68c8-409b-b22d-f7f1017484d4)

# Lab Recap
## Importance of External IP Addresses:
- **Connectivity:** External IP addresses allow devices or services to communicate over the internet, enabling accessibility from anywhere.
- **Identification:** They uniquely identify your system on the internet, facilitating data routing.
- **Scalability:** On-the-fly allocation provides flexibility and scalability, adapting to changing demands.
- **Resource Management:** Pre-allocating IPs ensures availability and efficient resource utilization.

- **Advance Provisioning:** Pre-allocating IPs ensures availability and allows for better planning and resource management.
- **Dynamic On-the-Fly Allocation:** On-the-fly allocation provides flexibility and scalability, adapting to changing demands in real-time.

In summary, whether you're provisioning external IPs in advance or dynamically assigning them on-the-fly, they play a crucial role in enabling connectivity, identification, scalability, and efficient resource management within your network infrastructure.


