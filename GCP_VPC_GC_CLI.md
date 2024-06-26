```bash
Setting Up a Custom VPC Network in GCP

Create a Custom VPC Network: Start by creating a Virtual Private Cloud (VPC) network that allows for custom subnet creation. Open your Google Cloud Console, go to the Cloud Shell, and enter the following command:

gcloud compute networks create revanvpc --subnet-mode=custom

Create a Subnet within the VPC: Now, create a subnet within your newly created VPC. This subnet will have a specific IP range. Use the following command:

gcloud compute networks subnets create revan-subnet \
    --network=revanvpc \
    --range=192.200.100.0/24 \
    --region=europe-west2

Allow SSH Connections: To securely access your instances, set up a firewall rule to allow SSH connections. Run:

gcloud compute firewall-rules create allow-ssh --network revanvpc --allow tcp:22 --source-ranges 0.0.0.0/0

Allow ICMP Traffic: To enable ping requests, create a firewall rule that allows ICMP traffic. This is useful for testing connectivity:

gcloud compute firewall-rules create allow-icmp --network revanvpc --allow icmp --direction INGRESS --priority 1000 --source-ranges 0.0.0.0/0

Create a Compute Instance: With the network and firewall configured, create a compute instance within your subnet. This instance will use a small machine type and a Debian image. Run:

gcloud compute instances create revaninstance \
    --zone=europe-west2-b \
    --machine-type=e2-micro \
    --subnet=revan-subnet \
    --image-family=debian-10 \
    --image-project=debian-cloud \
    --tags=allow-ssh

Establishing VPC Peering

Create a Peering Connection: To allow private network connectivity between two VPC networks across different projects, establish a VPC peering connection. Run the following command, adjusting the --peer-network URL to match the target VPC's full URL, so get your target VPC's network name, and project ID for --peer-network argument below:

  gcloud compute networks peerings create revan-to-malgus-peering-v2 \
    --network=revanvpc \
    --peer-network=https://www.googleapis.com/compute/v1/projects/flemming-friday/global/networks/malgusvpc \
    --project=inner-replica-417201

Accessing Your Instance via SSH: To test your setup, SSH into your newly created instance:

gcloud compute ssh revaninstance --zone=europe-west2-b

Once logged in, you can test network connectivity with tools like ping.

List VPC Peering Connections: To view active peering connections for a network, use:

gcloud compute networks peerings list --network=revanvpc --project=inner-replica-417201

Similarly, to view peering for the other network, replace --network and --project values accordingly.
```
