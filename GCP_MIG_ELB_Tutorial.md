# MIG/LB Outline

## Tutorial: Setting Up Managed Instance Groups with Load Balancer

1. **Create a New Project:**
   - Establishes a dedicated environment for managing resources and configurations related to the web application load balancer.

2. **Set Up Billing for the Project (if required):**
   - Enables the project to utilize Google Cloud resources and services, ensuring uninterrupted functionality.

3. **Enable Necessary APIs:**
   - Activates essential APIs such as Compute Engine, Cloud Load Balancing, and Cloud DNS to support the required functionalities.

4. **Create a VPC Network:**
   - Establishes a virtual private cloud (VPC) network to securely isolate the web application resources.

5. **Create Subnet for "us-central1" Region:**
   - Defines a subnet within the VPC network for the specified region, ensuring proper network segmentation and resource allocation.

6. **Create Subnet for "us-west1" Region:**
   - Allocates a subnet in a different region for redundancy and fault tolerance, enhancing the application's availability.

7. **Create Firewall Rules:**
   - Implements firewall rules to control inbound traffic, allowing only essential protocols like HTTP, HTTPS, and SSH while restricting unauthorized access.

8. **Create Instance Template with Startup Script:**
   - Sets up a template for creating virtual machine instances with predefined configurations and a startup script to automate instance setup tasks.

9. **Create a Managed Instance Group (MIG) for "us-central1" Region:**
   - Forms a managed group of instances in the specified region to ensure high availability and efficient resource management.

10. **Create a Managed Instance Group (MIG) for "us-west1" Region:**
    - Establishes another managed instance group in a different region to distribute the application's load geographically and mitigate regional failures.

11. **Create a Backend Service:**
    - Configures a backend service to route incoming traffic to the appropriate instances within the managed instance groups, ensuring efficient load distribution.

12. **Create a Backend Service for "us-west1" Region:**
    - Sets up a separate backend service for the instances in the "us-west1" region to handle traffic from that region specifically, optimizing performance.

13. **Create a Global HTTP(S) Load Balancer:**
    - Deploys a global load balancer to distribute incoming traffic across multiple regions, improving application performance, and resilience.

14. **Testing the Load Balancer:**
    - Validates the functionality and performance of the load balancer by simulating traffic and verifying proper distribution across regions and instances.





# Tutorial: Setting Up Managed Instance Groups with Load Balancer

1. **Create a New Project:**
   - Go to Google Cloud Console (console.cloud.google.com).
   - Click on the project dropdown menu at the top.
   - Select "New Project" from the dropdown.
   - Enter the project name as "webapp-load-balancer" and click "Create".

2. **Set Up Billing for the Project (if required):**
   - If prompted, set up billing for the newly created project to enable usage of Google Cloud resources.

3. **Enable Necessary APIs:**
   - Go to "APIs & Services" > "Dashboard" in the Cloud Console.
   - Click on "+ ENABLE APIS AND SERVICES" at the top.
   - Search for "Compute Engine API", "Cloud Load Balancing API", and "Cloud DNS API".
   - Click on each API and then click "Enable" to enable them for the project.

4. **Create a VPC Network:**
   - Navigate to "VPC network" > "VPC networks" in the Cloud Console.
   - Click on "+ CREATE VPC NETWORK" at the top.
   - Enter the name as "application-vpc".
   - Select "Custom" for subnet creation mode.

5. **Create Subnet for "us-central1" Region:**
   - Click on "+ ADD SUBNET".
   - Enter a name for the subnet, e.g., "us-central1-subnet".
   - Choose "us-central1" as the region.
   - Specify a subnet IP range that does not overlap with other subnets, such as "10.1.0.0/24".

6. **Create Subnet for "us-west1" Region:**
   - Click on "+ ADD SUBNET".
   - Enter a name for the subnet, e.g., "us-west1-subnet".
   - Choose "us-west1" as the region.
   - Specify a subnet IP range that does not overlap with other subnets, such as "10.2.0.0/24".

7. **Create Firewall Rules:**
   - While still in the "VPC networks" section, click on "Firewall rules".
   - Click on "+ CREATE FIREWALL RULE" at the top.
   - Name the rule as "allow-http-https-ssh".
   - Set "Direction of traffic" to "Ingress".

8. **Create Instance Template with Startup Script:**
   - Navigate to "Compute Engine" > "Instance templates" in the Cloud Console.
   - Click on "+ CREATE INSTANCE TEMPLATE" at the top.
   - Name the template as "webapp-instance-template".

9. **Create a Managed Instance Group (MIG) for "us-central1" Region:**
   - Navigate to "Compute Engine" > "Instance groups" in the Cloud Console.
   - Click on "+ CREATE INSTANCE GROUP" at the top.
   - Name the MIG as "mig-web-central1" for the "us-central1" region.

10. **Create a Managed Instance Group (MIG) for "us-west1" Region:**
    - Repeat the process to create another managed instance group named "mig-web-west1" for the "us-west1" region.

11. **Create a Backend Service:**
    - Navigate to "Load balancing" > "Backend services" in the Cloud Console.
    - Click on "+ CREATE BACKEND SERVICE" at the top.
    - Name the backend service as "mig-backend".
    - Choose "Instance group" as the backend type.

12. **Create a Backend Service for "us-west1" Region:**
    - Repeat the process to create another backend service named "mig-backend-west1" for the "us-west1" region.

13. **Create a Global HTTP(S) Load Balancer:**
    - Go to "Load balancing" in the Cloud Console.
    - Click on "+ CREATE LOAD BALANCER" at the top.
    - Choose "HTTP(S) Load Balancing" as the load balancer type.

14. **Testing the Load Balancer:**
    - Once the load balancer is created, obtain the IP address assigned to it.
    - Open a web browser and navigate to the IP address of the load balancer.
    - Continuously refresh the browser to simulate traffic.
    - Observe the instance details to ensure traffic is being distributed across both regions and instances.
