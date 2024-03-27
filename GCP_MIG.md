Managed Instance Group (MIG):

A Managed Instance Group in GCP is a collection of virtual machine (VM) instances that GCP manages for you.
You configure the number of VMs, machine type, and other settings.
MIGs provides certain capabilites such as auto-scaling
ASGs work with MIGs to automate VM scaling based on defined policies. An ASG doesn't directly manage the backend pool itself.

MIGs can also provide the VM instances that can be included in a backend pool.

Note: A backend pool can include VMs from one or more MIGs, offering flexibility in defining your backend configuration.

Recap Terms: 
Backend Pool: A logical grouping of backend resources used by an HTTP(S) or Cloud Load Balancing load balancer.
Backends can be VMs from a MIG, Cloud Functions, or other GCP resources.

A load balancer: distributes traffic across healthy backends within a pool.
Autoscaler Group (ASG): An optional feature used in conjunction with MIGs to enable automatic scaling.
You define scaling policies based on metrics like CPU usage or network traffic.
The ASG automatically adds or removes VMs from the MIG based on the defined policies.

***GCP Load Balancing provides a single, globally accessible IP address that routes traffic to your application's instances worldwide.***

GCP Load Balancing: Global Traffic Distribution with a Single IP Address
Within Google Cloud Platform (GCP), Google Cloud Load Balancing (GCLB) is a managed service that acts as a global or regional virtual traffic director 
for your web applications or services. It sits in front of a pool of backend instances (typically virtual machines in Compute Engine or containers in 
Kubernetes Engine) and efficiently distributes incoming traffic across them.





![image](https://github.com/mindmotivate/GCP_private/assets/130941970/68850e6d-19aa-452a-ac32-1d6f26c0c51c)


