
```hcl
# Define the provider and required variables
provider "google" {
  project = "webapp-load-balancer"
  region  = "us-central1"
}

variable "machine_type" {
  description = "The machine type for instances"
  default     = "n1-standard-1"
}

variable "startup_script_url" {
  description = "URL to the startup script"
}

variable "health_check_url" {
  description = "URL for health checks"
}

# Create a VPC network
resource "google_compute_network" "application_vpc" {
  name = "application-vpc"
}

# Create subnets
resource "google_compute_subnetwork" "us_central1_subnet" {
  name          = "us-central1-subnet"
  network       = google_compute_network.application_vpc.self_link
  region        = "us-central1"
  ip_cidr_range = "10.1.0.0/24"
}

resource "google_compute_subnetwork" "us_west1_subnet" {
  name          = "us-west1-subnet"
  network       = google_compute_network.application_vpc.self_link
  region        = "us-west1"
  ip_cidr_range = "10.2.0.0/24"
}

# Create firewall rule
resource "google_compute_firewall" "allow_http_https_ssh" {
  name    = "allow-http-https-ssh"
  network = google_compute_network.application_vpc.self_link

  allow {
    protocol = "tcp"
    ports    = ["80", "443", "22"]
  }

  source_ranges = ["0.0.0.0/0"]
}

# Create instance template
resource "google_compute_instance_template" "webapp_instance_template" {
  name        = "webapp-instance-template"
  machine_type = var.machine_type
  tags        = ["http-server", "https-server"]
  metadata_startup_script_url = var.startup_script_url
  network_interface {
    subnetwork = google_compute_subnetwork.us_central1_subnet.self_link
  }
}

# Create managed instance groups
resource "google_compute_instance_group_manager" "mig_web_central1" {
  name               = "mig-web-central1"
  base_instance_name = "mig-web-central1"
  instance_template  = google_compute_instance_template.webapp_instance_template.self_link
  target_size        = 2
  region             = "us-central1"
}

resource "google_compute_instance_group_manager" "mig_web_west1" {
  name               = "mig-web-west1"
  base_instance_name = "mig-web-west1"
  instance_template  = google_compute_instance_template.webapp_instance_template.self_link
  target_size        = 2
  region             = "us-west1"
}

# Create backend services
resource "google_compute_backend_service" "mig_backend" {
  name             = "mig-backend"
  load_balancing_scheme = "INTERNAL"
  protocol         = "HTTP"
  health_checks    = [var.health_check_url]
  backend {
    group = google_compute_instance_group_manager.mig_web_central1.self_link
  }
}

resource "google_compute_backend_service" "mig_backend_west1" {
  name             = "mig-backend-west1"
  load_balancing_scheme = "INTERNAL"
  protocol         = "HTTP"
  health_checks    = [var.health_check_url]
  backend {
    group = google_compute_instance_group_manager.mig_web_west1.self_link
  }
}

# Create global HTTP(S) load balancer
resource "google_compute_url_map" "webapp_load_balancer_map" {
  name            = "webapp-load-balancer-map"
  default_service = google_compute_backend_service.mig_backend.self_link
}

resource "google_compute_target_http_proxy" "webapp_load_balancer_proxy" {
  name    = "webapp-load-balancer-proxy"
  url_map = google_compute_url_map.webapp_load_balancer_map.self_link
}

resource "google_compute_global_forwarding_rule" "webapp_load_balancer_forwarding_rule" {
  name       = "webapp-load-balancer-forwarding-rule"
  target     = google_compute_target_http_proxy.webapp_load_balancer_proxy.self_link
  port_range = "80"
}

# Output clickable URL for load balancer
output "load_balancer_url" {
  value = google_compute_global_forwarding_rule.webapp_load_balancer_forwarding_rule.ip_address
  description = "Click to access the load balancer"
}

# Output other useful information
output "instance_template_id" {
  value = google_compute_instance_template.webapp_instance_template.id
  description = "ID of the instance template"
}

output "managed_instance_group_ids" {
  value = [
    google_compute_instance_group_manager.mig_web_central1.id,
    google_compute_instance_group_manager.mig_web_west1.id
  ]
  description = "IDs of the managed instance groups"
}
```
