```bash
# Create a new GCP project
gcloud projects create webapp-load-balancer --name="Web App Load Balancer Project"

# Set up billing for the project
gcloud alpha billing projects link webapp-load-balancer --billing-account=<billing_account_id>

# Enable necessary APIs
gcloud services enable compute.googleapis.com \
    --project=webapp-load-balancer
gcloud services enable compute.googleapis.com \
    --project=webapp-load-balancer
gcloud services enable dns.googleapis.com \
    --project=webapp-load-balancer

# Create VPC network and subnets
gcloud compute networks create application-vpc --subnet-mode=custom \
    --project=webapp-load-balancer
gcloud compute networks subnets create us-central1-subnet --network=application-vpc --region=us-central1 --range=10.1.0.0/24
gcloud compute networks subnets create us-west1-subnet --network=application-vpc --region=us-west1 --range=10.2.0.0/24

# Create firewall rules
gcloud compute firewall-rules create allow-http-https-ssh --direction=INGRESS --action=ALLOW --rules=tcp:80,tcp:443,tcp:22 --source-ranges=0.0.0.0/0 --network=application-vpc

# Create instance template
gcloud compute instance-templates create webapp-instance-template \
    --machine-type=<machine_type> \
    --tags=http-server,https-server \
    --metadata=startup-script-url=<URL_to_startup_script> \
    --subnet=us-central1-subnet

# Create managed instance groups
gcloud compute instance-groups managed create mig-web-central1 \
    --template=webapp-instance-template \
    --base-instance-name=mig-web-central1 \
    --size=2 \
    --region=us-central1

gcloud compute instance-groups managed create mig-web-west1 \
    --template=webapp-instance-template \
    --base-instance-name=mig-web-west1 \
    --size=2 \
    --region=us-west1

# Create backend services
gcloud compute backend-services create mig-backend \
    --global \
    --load-balancing-scheme=INTERNAL \
    --protocol=HTTP \
    --health-checks=<health_check_url> \
    --backend-group=mig-web-central1

gcloud compute backend-services create mig-backend-west1 \
    --global \
    --load-balancing-scheme=INTERNAL \
    --protocol=HTTP \
    --health-checks=<health_check_url> \
    --backend-group=mig-web-west1

# Create global HTTP(S) load balancer
gcloud compute url-maps create webapp-load-balancer-map --default-service=mig-backend
gcloud compute target-http-proxies create webapp-load-balancer-proxy --url-map=webapp-load-balancer-map
gcloud compute forwarding-rules create webapp-load-balancer-forwarding-rule --global --target-http-proxy=webapp-load-balancer-proxy --port-range=80

# Test the load balancer
# (Test steps would depend on the application and its specific requirements)
```
