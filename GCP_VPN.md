
**VPN and Interconnect: GCP offers VPN (Virtual Private Network) and Interconnect options for connecting your on-premises network to your VPC securely.** 
**VPN provides an encrypted tunnel over the public internet, while Interconnect offers dedicated, high-bandwidth connections.** 
**These options are useful for extending your on-premises network to GCP or for establishing hybrid cloud architectures.**

### Cloud VPN

**Use Case**: 
Cloud VPN in GCP is suitable for smaller-scale deployments or occasional data transfer needs between your on-premises network and your VPC via **IPsec VPN connection**
Traffic traveling between the two networks is encrypted by one VPN gateway, and then decrypted by the other VPN gateway.
You can connect two instances of Cloud VPN to each other

**HA VPN** High Availability Cloud VPN solution for connecting On-Prem to VPC via Ipsec VPN (in a single region)
-SLA %99.99
-No forwarding rules 
-Uses external VPN gateway resource to provide info to GCP about your peer VPN

**Classic VPN**
-Single interface, single external IP address, and support tunnels using dynamic or static routing
-Much SLOWER than HA VPN

**Advantages**:
- Provides a Virtual Private Network solution for secure communication over the public internet.
- Easy to set up and configure.
- Suitable for small to medium-sized networks or occasional data transfer needs.

**Limitations**:
- Limited bandwidth compared to dedicated connections.
- Subject to potential performance issues and latency due to reliance on the public internet.
- May not be ideal for applications requiring high-speed, consistent connectivity.

### Dedicated Interconnect

**Use Case**:
Dedicated Interconnect offers a dedicated, high-bandwidth connection between your on-premises network and your VPC in GCP, suitable for large-scale data transfer or applications requiring consistent, high-speed connectivity.

**Advantages**:
- Provides a direct, private connection with high reliability and low latency.
- Offers dedicated, high-bandwidth connections (up to multiple 10 Gbps).
- Ideal for large-scale data transfer or applications with stringent performance requirements.

**Limitations**:
- Requires physical infrastructure and higher initial setup costs.
- May involve longer lead times for provisioning compared to Cloud VPN.
- Suitable for larger organizations or enterprises with significant connectivity needs.

### Partner Interconnect/Peering

**Use Case**:
Partner Interconnect and Peering provide alternative options for connecting your on-premises network to your VPC in GCP through external networks or with other organizations.

**Advantages**:
- Partner Interconnect offers connectivity through supported service providers, providing flexibility in bandwidth options and eliminating the need for direct physical infrastructure setup.
- Peering enables direct communication between VPC networks within or across organizations, maintaining private IP addressing.

**Limitations**:
- Partner Interconnect may have slightly higher latency compared to Dedicated Interconnect due to routing through a partner network.
- Peering is limited to communication between VPC networks and may require additional setup for routing and network configurations.


Select your project (or choose an existing one)

Create VPC

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/6f59a811-634b-47c2-8936-302d7400eb7a)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/21b94ef2-04fc-41bc-a8e9-1007c22ed63d)

Name 
Region 
IP Range
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/b9d4175c-ba88-474e-a1c0-dba54d0da61f)

Private Google Access
Flow logs
Dynamic Routing Mode
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/f973007f-05b9-4974-a81a-d0f81119a931)

Create another VPC

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/7852f6fc-59ce-46cf-a5ed-572f17ee506a)

Name 
Region 
IP Range
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/e1778214-39f6-4f03-b2ed-ca159edace71)


Private Google Access
Flow logs
Dynamic Routing Mode
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/77186991-839a-45dd-bfc4-bb808df263fa)

Ensure both VPCs wer created sucessfully
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/1fcd4dfc-5bff-41d4-a8ff-1d6737d70bf7)

➤ Create a set of Firewall Rules (for each of your two networks)
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/b6d75c90-b7a6-4978-96fb-bd74d3088d95)

Name: rule-custom-vpc
Description: rule-custom-vpc 
Logs:On
Network: custom-vpc-1
Direction of traffic: Ingress
Targets: All instances in the network
Source Filter: IP Ranges
Source Ip ranges: 0.0.0.0/0

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/5cfd07cc-a397-4f17-8f03-0348be7b58ba)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/67050075-ecac-4406-9cd3-6befac828ef1)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/05ebf35f-4121-4c0d-9f41-1458f78ccb90)

Ports & Protocals
-Specified protocals and ports
-tcp: 22
-other protocals: icmp

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/e60105d3-41c2-4972-b0f4-de177f908561)



Name: rule-custom-vpc-2
Description: rule-custom-vpc 
Logs:On
Network: custom-vpc-2
Direction of traffic: Ingress
Targets: All instances in the network
Source Filter: IP Ranges
Source Ip ranges: 0.0.0.0/0

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/1c5d2eb5-eb93-4a66-a82d-d7f401bb2043)


Ports & Protocals
-Specified protocals and ports
-tcp: 22
-other protocals: icmp

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/93ccbdeb-40f4-4cdd-9e26-536d02af7d56)

Press Create

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/91853d55-b09c-49ad-9bd0-5809c2021701)

Ensure your created firewall rues were applied correctly

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/02089ea1-23af-443a-b997-dda81db60222)


➤ Create Instance Each using one VPC (custom-ins-1)

Navigate to compute engine and select "VIM-instance"

we will create our custim instance in "us-central-1" where our 1st VPC is located

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/3613bdfb-4933-4f84-b393-8562fd098aed)

In the network interfaces section choose your "custom-vpc-1" and its associated subnet

Click "Create"



➤ Create Another Instance using one VPC (custom-ins-2)

Navigate to compute engine and select "VIM-instance"

we will create another custom instance in "us-east-1" where our 2nd VPC is located

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/3613bdfb-4933-4f84-b393-8562fd098aed)

In the network interfaces section choose your "custom-vpc-2" and its associated subnet

Click "Create"

Ensure both instances are created and ready

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/b8dc9a47-9ca2-406d-b34a-9fbb6d5e23b8)


➤ Verify Connectivity

Use SSH to access the first instance: 

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/2ee96156-8f67-4391-acb4-97e77507d552)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/5b09996a-1862-48e9-8db3-a6965b03c296)

Once accessed, attempt to ping (this means our firewall rule is working)

### ACCESS INSTANCE 2 FROM INSTANCE 1 VIA EXTERNAL IP

use "ping command" along with the external ip of instance 2
ping -c 3 (this command specified 3 pings exactly)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/a54e7c7b-5f93-49dc-977f-4dc8278ba141)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/1fd847e4-2cbc-498d-93e6-7d13190fb09e)

### ACCESS INSTANCE 2 FROM INSTANCE 1 VIA INTERNAL IP

Try to ping the internal Ip address of your second instance:

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/844c5092-2aa1-4171-949c-d6e986bd6fb2)

Note: You should not be sucessful due to our configuration

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/d2d4ef48-90f2-469c-8c2d-6291ee13efcc)



### ACCESS INSTANCE 1 FROM INSTANCE 2 VIA EXTERNAL IP

Now attempt SSH with the second instance via the external Ip addess

You should be sucessful

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/2235d6a0-1a15-4d89-92bd-bf92ec35f244)

### ACCESS INSTANCE 1 FROM INSTANCE 2 VIA INTERNAL IP

However if we try to access the internal IP we should NOT have access

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/9a3d71a6-8156-45c8-8ecf-a1d119317997)




➤ Create Two VPN one for each Network

Use the CLI:

```bash
gcloud compute target-vpc-gateways create <VPN_NAME> --network=<NETWORK_NAME> --region=<REGION>
```

use your vpnname: (myvpn-1)
network name: (custom-vpc-1)
Obtain your subnet region (us-central1)

```bash
gcloud compute target-vpc-gateways create myvpn-1 --network custom-vpc-1 --region us-central1
```

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/baf11be4-02dc-4b57-a8cc-167020808ce4)

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/832f02d7-65df-4775-a098-aacca9abb3ee)

Press Enter

You should recieve a confirmation message:

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/6e50cdac-68ee-43a5-8d59-3785dcd197f2)

Now create a static Ip that can be linked to your VPN

```bash
gcloud compute addresses create <STATIC_IP_NAME> --region=<REGION>
```
static ip name: (myvpn-1-ip)
region: (us-central1)

```bash
gcloud compute addresses create myvpn-1-ip --region us-central1
```
Press Enter

Await your confirmation message:

![image](https://github.com/mindmotivate/GCP_private/assets/130941970/e115a1d0-959d-4a31-8b51-b48ce023ad43)

Use this command to list all of the ip addesses in your project

```bash
gcloud compute addresses list
```
![image](https://github.com/mindmotivate/GCP_private/assets/130941970/640d70e9-3955-47d8-a691-cfd50a242357)



➤ Create Static IP one for each Network

➤ Set Forwarding Rules for each VPN Gateway

➤ Create Tunnel between each Gateway

➤ Create Route for Each Network



