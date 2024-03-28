# MIG/LB Outline

## Tutorial: Setting Up Managed Instance Groups with Load Balancer

1. **Create a New Project:**    - Establishes a dedicated environment for managing resources and configurations related to the web application load balancer.

2. **Set Up Billing for the Project (if required):**  - Enables the project to utilize Google Cloud resources and services, ensuring uninterrupted functionality.

3. **Enable Necessary APIs:**  - Activates essential APIs such as Compute Engine, Cloud Load Balancing, and Cloud DNS to support the required functionalities.

4. **Create a VPC Network:**   - Establishes a virtual private cloud (VPC) network to securely isolate the web application resources.

5. **Create Subnet for "us-central1" Region:**   - Defines a subnet within the VPC network for the specified region, ensuring proper network segmentation and resource allocation.

6. **Create Subnet for "us-west1" Region:**   - Allocates a subnet in a different region for redundancy and fault tolerance, enhancing the application's availability.

7. **Create Firewall Rules:**   - Implements firewall rules to control inbound traffic, allowing only essential protocols like HTTP, HTTPS, and SSH while restricting unauthorized access.

8. **Create Instance Template with Startup Script:**   - Sets up a template for creating virtual machine instances with predefined configurations and a startup script to automate instance setup tasks.

9. **Create a Managed Instance Group (MIG) for "us-central1" Region:**   - Forms a managed group of instances in the specified region to ensure high availability and efficient resource management.

10. **Create a Managed Instance Group (MIG) for "us-west1" Region:**    - Establishes another managed instance group in a different region to distribute the application's load geographically and mitigate regional failures.

11. **Create a Backend Service:**    - Configures a backend service to route incoming traffic to the appropriate instances within the managed instance groups, ensuring efficient load distribution.

12. **Create a Backend Service for "us-west1" Region:**  - Sets up a separate backend service for the instances in the "us-west1" region to handle traffic from that region specifically, optimizing performance.

13. **Create a Global HTTP(S) Load Balancer:**   - Deploys a global load balancer to distribute incoming traffic across multiple regions, improving application performance, and resilience.

14. **Testing the Load Balancer:**   - Validates the functionality and performance of the load balancer by simulating traffic and verifying proper distribution across regions and instances.





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

5. **Create Subnet for "us-central1" Region:**
   - Click on "+ ADD SUBNET".
   - Enter a name for the subnet, e.g., "us-central1-subnet".
   - Choose "us-central1" as the region.
   - Specify a subnet IP range that does not overlap with other subnets, such as "10.1.0.0/24".
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/22462d9d-f060-46c8-8528-b1f53359b53b)

6. **Create Subnet for "us-west1" Region:**
   - Click on "+ ADD SUBNET".
   - Enter a name for the subnet, e.g., "us-west1-subnet".
   - Choose "us-west1" as the region.
   - Specify a subnet IP range that does not overlap with other subnets, such as "10.2.0.0/24".
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/2edb99c3-4775-4227-952c-6559126e6122)

7. **Create Firewall Rules:**
   - While still in the "VPC networks" section, click on "Firewall rules".
   - Click on "+ CREATE FIREWALL RULE" at the top.
   - Name the rule as "allow-http-https-ssh".
   - ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/2bb6f1cc-130b-4d8e-85a5-567edd5f6c3a)
   - Set "Direction of traffic" to "Ingress".
   - In a normal production environment, you will NOT use icmp: "PING"

-choose your dynamic routing option
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/1a00a6b1-2e0b-4410-ab51-a1b609d8597a)
-Select "Create"
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/0066b8ca-048b-4257-ada9-9f007918693b)
-CHeck to see that your VPC has been creates
8. **Create Instance Template with Startup Script:**
   - Navigate to "Compute Engine" > "Instance templates" in the Cloud Console.
   - Click on "+ CREATE INSTANCE TEMPLATE" at the top.
   - Name the template as "webapp-instance-template".
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/4dfddeac-0bcd-4b65-b6be-9f0c540672b6)
- Ensure that you select the VPC you created and that you choose the subnet in the correct region
- ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/459d0534-1857-44aa-a104-5f455eb975f2)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/218a83a2-c8af-46ea-ab9d-8451da350951)
-Scroll down to the "Management" section and paste your startuo script in the Automation text box

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/585a6eb6-90b4-4ecd-ba35-68fdcfb4d997)

-Copy  & Paste the following script:
```bash
#Thanks to Remo
#!/bin/bash
# Update and install Apache2
apt update
apt install -y apache2

# Start and enable Apache2
systemctl start apache2
systemctl enable apache2

# GCP Metadata server base URL and header
METADATA_URL="http://metadata.google.internal/computeMetadata/v1"
METADATA_FLAVOR_HEADER="Metadata-Flavor: Google"

# Use curl to fetch instance metadata
local_ipv4=$(curl -H "${METADATA_FLAVOR_HEADER}" -s "${METADATA_URL}/instance/network-interfaces/0/ip")
zone=$(curl -H "${METADATA_FLAVOR_HEADER}" -s "${METADATA_URL}/instance/zone")
project_id=$(curl -H "${METADATA_FLAVOR_HEADER}" -s "${METADATA_URL}/project/project-id")
network_tags=$(curl -H "${METADATA_FLAVOR_HEADER}" -s "${METADATA_URL}/instance/tags")

# Create a simple HTML page and include instance details
cat <<EOF > /var/www/html/index.html
<html><body>
<h2>Welcome to your custom website.</h2>
<h3>Created with a direct input startup script!</h3>
<p><b>Instance Name:</b> $(hostname -f)</p>
<p><b>Instance Private IP Address: </b> $local_ipv4</p>
<p><b>Zone: </b> $zone</p>
<p><b>Project ID:</b> $project_id</p>
<p><b>Network Tags:</b> $network_tags</p>
</body></html>
EOF
```
-Go ahead select "Create"
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/4ff991d1-49ae-4022-94d7-a38a93d63371)


9. **Create a Managed Instance Group (MIG) for "us-central1" Region:**
   - Navigate to "Compute Engine" > "Instance groups" in the Cloud Console.

   - ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/702daa0d-4373-4513-9b4a-3e49e5a4527f)

   - ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/bef4ab78-fb98-448f-b5d9-f4b8cfb6407c)

   - Click on "+ CREATE INSTANCE GROUP" at the top.
  
   - Note: Alternatively you can simply select the following button from the top of instance template screen:
  
   - ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/ca8bec7b-22ef-4c4d-adce-701fdb9145ed)

   - Name the MIG "mig-web-central1" for the "us-central1" region.
  
    Note: Although, not required for this tutorial, you may opt for "Autoscaling" and set the desired minimum and maximum nunmber of instances
   ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/1a18e754-0a46-4bde-a6a4-a29e0253ceae)

   Note: Although, not required for this tutorial, you may opt for "Health Checks" & "Signals" respectively

   -Click "Create" after all selections have been made
   
   ![image](https://github.com/mindmotivate/GCP_private/assets/130941970/93099d81-7dad-42a9-8246-b162e2bb4cdb)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/91ae4024-798e-4df1-8ac5-fb65a503866f)

**Test one of your instances by utlizing its SSH connection (ping 8.8.8.8)***

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/2d01682f-81b1-40e2-b712-941cb7e611bf)

**Test one of your instances by copying its external IP address into your browser (be sure to inlcude "http://" prefix)***


11. **Create a Managed Instance Group (MIG) for "us-west1" Region:**
    - Repeat the process to create another managed instance group named "mig-web-west1" for the "us-west1" region.




12. **Create a Backend Service:**
    - Navigate to "Load balancing" > "Backend services" in the Cloud Console.
    - Click on "+ CREATE BACKEND SERVICE" at the top.
    - Name the backend service as "mig-backend".
    - Choose "Instance group" as the backend type.

13. **Create a Backend Service for "us-west1" Region:**
    - Repeat the process to create another backend service named "mig-backend-west1" for the "us-west1" region.

14. **Create a Global HTTP(S) Load Balancer:**
    - Go to "Load balancing" in the Cloud Console.
    - Click on "+ CREATE LOAD BALANCER" at the top.
    - Choose "HTTP(S) Load Balancing" as the load balancer type.

15. **Testing the Load Balancer:**
    - Once the load balancer is created, obtain the IP address assigned to it.
    - Open a web browser and navigate to the IP address of the load balancer.
    - Continuously refresh the browser to simulate traffic.
    - Observe the instance details to ensure traffic is being distributed across both regions and instances.
