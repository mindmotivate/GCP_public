



## Instance Templates in Google Cloud Platform (GCP)

Instance Templates in GCP are predefined configurations for VM instances, specifying properties like machine type, boot disk image, and network settings. 
They ensure consistency and simplify the creation of multiple VMs with identical configurations. Instance Templates are crucial for Managed Instance Groups (MIGs), 
enabling automated instance creation, scaling, and updates. They also facilitate integration with load balancers, ensuring efficient traffic distribution and reliable application performance.

Let's create an instance template!
(GCP has some ready made templplates?)
name: gcp-instance-template

e2-medium


## Managed Instance Groups (MIGs)
Managed Instance Groups are collections of virtual machines (VMs) that are managed as a single entity, allowing for automated instance creation, deletion, and updates.
Integration with Load Balancers: **MIGs can be integrated with load balancers to automatically manage the instances based on the traffic load and health checks.**

Let's create an instance group!

name: gcp-instance-group
(copy this instance group name into the description)

instance template: 
(select the instance template that you previoulsy created)

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


