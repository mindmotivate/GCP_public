## Shared VPC's
### Scenario:
- **Organization Structure**: 
  - One organization containing two projects, each with its own resources and requirements.
- **Resource Sharing Requirement**:
  - Resources provisioned in one project may need to access resources in the other project.
- **Shared VPC Approach**:
  - Implement a shared VPC architecture within the organization.
  - Designate one project as the "host" project, containing the shared VPC.
  - Configure both projects to utilize resources from this shared VPC.
- **Benefits**:
  - Centralized management and control over network resources.
  - Simplifies inter-project resource access and communication.
  - Facilitates consistent network policies and security controls across projects.
  - Commonly adopted in large companies for efficient resource utilization and management.
- **Considerations**:
  - Requires coordination between network engineers and project teams.
  - Ensure compliance with organizational policies and access controls.
  - Evaluate scalability and performance implications of shared infrastructure.
  - Limitations include organizational node capacity and restrictions such as the 100-project limit in platforms like GCP.
  - Shared VPC is only available within an organization's project hierarchy.
  - There are limitations on the number of host projects allowed, typically with a maximum service project limit as well.
 
# Lab Setup Shared VPC

## 1. Establish Organization Node
   - Ensure you have an organization established (e.g., `malgusclan`).

## 2. First Tab: Create Projects and Custom VPC
   - Go to the cloud platform console.
   - Navigate to the dashboard.
   - Create three projects:
     - Host Project: `my-vpc`
     - Service Project 1: `serviceP1`
     - Service Project 2: `serviceP2`
   - Within the `my-vpc` project:
     - Navigate to VPC network > VPC networks.
     - Click "Create VPC network".
     - Name it `my-hvpc`.
     - Configure the VPC settings:
       - Specify IP range for the VPC.
       - Create subnets:
         - Subnet 1 with specific range.
         - Subnet 2 in `us-central-1` with another range.
     - Ensure Compute Engine API is enabled.
     - Click "Create".

## 3. Second Tab: Initialize Compute Engine in Service Project P1
   - Open a new tab in the cloud platform console.
   - Switch to the `serviceP1` project.
   - Navigate to Compute Engine.
   - Create and initialize a Compute Engine instance as needed.

## 4. Return to Host Project and Configure Shared VPC
   - Go back to the `my-vpc` (Host) project.
   - Go to IAM & admin > IAM.
   - Search for the permission error mentioned.
   - Create a role for the Host Project:
     - The required permission is `compute.organizations.enableXPNHost`.
     - This permission provides three host roles: "Compute Shared VPC Admin Role", etc.
   - Go to IAM & admin > IAM for the organization level.
   - Select "Edit Member" under the host project.
   - Choose "Compute Shared VPC Admin" from the role dropdown.
   - Save changes.
   - Verify that the new role has been assigned properly.
   - Re-check permissions to ensure the new role is applied correctly.

## 5. Configure Shared VPC Set
   - Go to IAM & admin > Shared VPC.
   - Enable the Host Project if not already enabled.
   - Select the desired subnets from other projects to share.
   - Grant necessary permissions for the shared subnets.
   - Return to Shared VPC setup.
   - Ensure everything works fine without any error messages.
