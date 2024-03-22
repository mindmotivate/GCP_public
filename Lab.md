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
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/387a42b2-5a63-4df9-aa6e-4118c0a39eae)

### Name your static IP address
15. Provide a name for your static IP address.

### Choose network service tier
16. Select the network service tier.

### IP version: ipv4
17. Ensure that the IP version is set to IPv4.

### Type: regional
18. If choosing regional type, select a specific region. (In this case we will use "us-central1(iowa)")

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/134a3b0b-0b5c-43c9-b947-9da8f0b19869)


### Click Reserve and Wait for static IP creation
19. Wait for the static IP address to be created.

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/7c3a7dde-5d6d-4634-b47c-9f5302e4e7de)

### Create a second static IP (global this time)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/ecab351f-a90e-431f-83d3-69dc646f7ffd)
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/96b015d0-b6bb-4a5f-8f48-ef7b42e6c6c7)
19. Wait for the static IP address to be created.
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/5ec2fe65-feac-49b4-bf0d-0556ecd2cc16)

### Compute Engine
21. Navigate to Compute Engine.
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/cfd95cbc-ee38-4c5b-876d-54104e723bb3)

### Create a VM instance
22. Name your instance. (we will name it instance-1)
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/de9e8da1-746c-45f6-92c3-35931ddaefa9)
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/2d7971d3-65e4-4a53-9595-a288d45674e7)

### Networking section
23. Under networking settings:
    - Network tags: optional
    - Networking interfaces: default
    - Choose the default subnet.
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/c8aa4800-cdd8-4e8d-b6b8-3f3063e03795)
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/2849c5e6-ab8b-4b65-9461-99f386cc5c8b)

### Choose internal IP type (we will choose ephemral automatic)
24. Select one of the following for primary internal IP:
    - Ephemeral automatic
    - Ephemeral custom
    - Reserve static internal IP address (will cost more)

For External Ip choose "none"

### Click "create" and wait for creation
25. Wait for its creation.
    ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/ec162812-6c92-4f79-a062-4217a4f65c9c)


### Create another instance (call it "instance-2")
26. Choose ephemeral for internal IP type.

### Assign static IP
27. Create another instance and select the static IP created earlier.

### Verify instances
28. You should now have three instances:
    - The first one without an external IP.
    - The second with a dynamically assigned IP.
    - The third with the static IP assigned.



