# Creating VMs with Labels for Auto Start/Stop (GCP Console)

Here's a step-by-step guide with console commands to create VMs with labels for auto start/stop functionality:

## 1. Create VMs:

- Go to the GCP Console and navigate to the Compute Engine section.
- Click on the "Create instance" button.

### Console Command (Optional):

gcloud compute instances create <INSTANCE-NAME> \
    --machine-type e2-micro \
    --zone <ZONE>

Use code with caution.
Replace <INSTANCE-NAME> with a desired name for your VM.
Replace <ZONE> with the zone where you want to create the VM (e.g., us-central1-a).

- **Labeling VMs:**
  - In the "Labels" section of the VM creation wizard, click on "Add item."
  - Enter a key-value pair to identify your VM group (e.g., "env=dev"). You can create multiple VMs with the same label for group management.
  - Repeat the process to add additional labels if needed.
  - Click "Create" to launch your VM instance.

## 2. Create a Cloud Function:

- Go to the Cloud Functions section in the GCP Console.
- Click on the "Create function" button.

### Console Command (Optional):

gcloud functions create vm-control \
    --runtime python38 \
    --trigger-topic=<PROJECT_ID>-vm-control-topic \
    --allow-unauthenticated

Use code with caution.
Replace <PROJECT_ID> with your actual project ID.

This command creates a Cloud Function named "vm-control" with Python 38 runtime and sets the trigger to a Pub/Sub topic named "<PROJECT_ID>-vm-control-topic".
The --allow-unauthenticated flag is used for simplicity in this example, but consider removing it in production for security reasons (we'll address security in a later step).

## 3. Cloud Function Code:

Paste the following code into the Cloud Function editor, replacing the placeholders:
```bash
def vm_control(message):
    """Stops or starts VMs based on the message content."""
    
    import json

    data = json.loads(message.data)
    action = data.get("action")
    filter_label = data.get("label")

    # Replace with your service account details (get from IAM)
    service_account = "<SERVICE_ACCOUNT_EMAIL>"
    credentials = GoogleCredentials.get_application_default()
    credentials.refresh(Request())

    compute = compute_v1.compute.ComputeEngine(credentials=credentials)

    # Get all VMs in the project
    project = f"projects/{<PROJECT_ID>}"
    instances = compute.instances().list(project=project).execute()

    for instance in instances["items"]:
        # Check if the label matches the filter
        if instance.get("labels", {}).get("env") == filter_label:
            # Perform action (stop or start) based on the message
            if action == "stop":
                compute.instances().stop(project, zone=instance["zone"], instance=instance["name"]).execute()
            elif action == "start":
                compute.instances().start(project, zone=instance["zone"], instance=instance["name"]).execute()
```


Use code with caution.
Replace <PROJECT_ID> with your actual project ID.
Important: Replace <SERVICE_ACCOUNT_EMAIL> with the email address of a service account with the necessary permissions to stop/start VMs in Compute Engine. You'll need to create and grant appropriate IAM permissions to this service account (security will be covered in a separate step).

## 4. Deploy the Cloud Function:

Click the "Deploy" button in the Cloud Functions console.

## 5. Next Steps:

This creates VMs with labels and a Cloud Function to manage them. In the following steps, we'll configure Cloud Scheduler to trigger the Cloud Function for automated VM start/stop and implement security best practices.








Create VMs:
Define a Terraform configuration file (typically with a .tf extension) for creating VM instances. Use the google_compute_instance resource to define the VM instances. Specify the necessary parameters such as name, machine_type, zone, and labels. Here's a simplified example:
```hcl
Copy code
resource "google_compute_instance" "example_instance" {
  name         = "example-instance"
  machine_type = "e2-micro"
  zone         = "us-central1-a"

  labels = {
    env = "dev"
  }
}
```
Create a Cloud Function:
Define another Terraform configuration file to create the Cloud Function. Use the google_cloudfunctions_function resource to define the Cloud Function. Specify the necessary parameters such as name, runtime, event_trigger, and source_archive_bucket. Here's a simplified example:
```hcl
Copy code
resource "google_cloudfunctions_function" "vm_control" {
  name        = "vm-control"
  runtime     = "python38"
  source_archive_bucket = google_storage_bucket.cloud_function_bucket.name

  event_trigger {
    event_type = "google.pubsub.topic.publish"
    resource   = "projects/${var.project_id}/topics/vm-control-topic"
  }
}
```
Cloud Function Code:
You can store the Cloud Function code in a separate file and use Terraform's archive_file data source to include it in your Terraform configuration. Here's an example:
```hcl
Copy code
data "archive_file" "vm_control_code" {
  type        = "zip"
  source_dir  = "${path.module}/cloud_function_code"
  output_path = "${path.module}/vm_control_code.zip"
}

resource "google_storage_bucket_object" "cloud_function_code" {
  name   = "vm_control_code.zip"
  bucket = google_storage_bucket.cloud_function_bucket.name
  source = data.archive_file.vm_control_code.output_path
}
```
Deploy the Cloud Function:
After defining the Cloud Function configuration, run terraform apply to deploy it along with the VMs.

Next Steps:
After deploying the VMs and Cloud Function, you can proceed to configure Cloud Scheduler to trigger the Cloud Function for automated VM start/stop, similar to the steps outlined in the previous guide.

Remember to authenticate Terraform with your Google Cloud account using service account credentials and provide the necessary IAM permissions for Terraform to create and manage resources in your GCP project. Additionally, ensure that you follow security best practices and handle sensitive information securely, such as service account credentials and API keys.






Here are specific instructions for managing the state file securely using Google Cloud Storage (GCS) as the remote backend:

*Note: managing the Terraform state file securely using a remote backend like Google Cloud Storage (GCS) should ideally be set up at the beginning of the project. By establishing this infrastructure early on, you ensure that:

1. **Create a Google Cloud Storage Bucket:**
   - Go to the Google Cloud Console: https://console.cloud.google.com/.
   - Navigate to the Cloud Storage section.
   - Click on "Create bucket."
   - Specify a globally unique bucket name and choose the location where you want to store the bucket.
   - Click "Create" to create the bucket.

2. **Enable Versioning for the Bucket (Optional but Recommended):**
   - Select the bucket you created.
   - Go to the "Settings" tab.
   - Click on "Edit bucket details."
   - Enable versioning for the bucket.
   - Click "Save" to apply the changes.

3. **Configure Terraform to Use Google Cloud Storage as the Backend:**
   - In your Terraform configuration directory, create a file named `backend.tf`.
   - Add the following configuration to `backend.tf` to specify Google Cloud Storage as the backend:

```hcl
terraform {
  backend "gcs" {
    bucket  = "<YOUR_BUCKET_NAME>"
    prefix  = "terraform/state"
  }
}












