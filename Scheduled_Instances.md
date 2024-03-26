# Automating VM Management with Google Cloud Platform

#### The Problem:
Development and test Virtual Machines (VMs) often run outside of business hours when they're not actively used. Manually stopping and starting these VMs can be tedious and inconsistent.

#### The Solution:
Google Cloud Platform (GCP) offers a comprehensive solution to this problem by leveraging several of its services:

1. **Google Cloud Scheduler**: This service allows you to schedule tasks like stopping and starting VMs.
   
2. **Cloud Pub/Sub**: Acting as a messaging service, Cloud Pub/Sub serves as a bridge between Cloud Scheduler and Cloud Functions.

3. **Cloud Functions**: These serverless functions can be triggered by Cloud Scheduler and execute specific actions, such as stopping or starting VMs based on predefined labels.

#### Benefits:

1. **Cost Savings**: With this automated approach, you only pay for the time VMs are actually running, effectively reducing compute costs.
   
2. **Automation**: Cloud Scheduler automates the entire process, eliminating the need for manual intervention and ensuring consistency in VM management.
   
3. **Flexibility**: Different schedules can be set for different groups of VMs identified by labels, providing flexibility in managing various types of VMs.

4. **Security**: Service accounts with the least privilege can be utilized for VM control through IAM (Identity and Access Management), ensuring security best practices.

5. **Clean Shutdown**: VMs can execute shutdown scripts before stopping, ensuring a clean state and preventing data loss or corruption.

6. **Data Persistence**: Persistent disks with auto-delete disabled retain data even when VMs are stopped, ensuring data persistence across instances.

7. **Override Option**: Users have the flexibility to temporarily remove labels or restart stopped VMs for ad-hoc access, providing additional control and convenience.

#### Conclusion:
By leveraging GCP services such as Google Cloud Scheduler, Cloud Pub/Sub, and Cloud Functions, organizations can automate VM management, optimize resource usage, and save costs for development and test environments. This approach not only streamlines operations but also ensures efficient utilization of cloud resources while maintaining security and flexibility.



# Introduction

This lab guides you through automating the start and stop process of Virtual Machines (VMs) in your Google Cloud Platform (GCP) project based on labels and a schedule. This helps optimize costs by shutting down development and test VMs during off-hours when they're not actively used.

## Requirements:

1. **GCP Project:** An active GCP project with billing enabled. You can usually get free tier credits for experimenting ([GCP Free Tier](https://cloud.google.com/free)).

2. **Permissions:** The "Cloud Scheduler Editor" and "Compute Engine Instance Admin" roles assigned to your user account. Your IT admin can help you with this step. ([Understanding Roles](https://cloud.google.com/iam/docs/understanding-roles))

3. **Basic understanding of:**
   - GCP project structure
   - VM creation and labeling in Compute Engine
   - Python programming (optional, for understanding the Cloud Function code)

## GCP Services Used:

- **Compute Engine:** Manages virtual machines in your GCP project. ([Compute Engine Docs](https://cloud.google.com/compute/docs))
- **Cloud Functions:** Serverless functions triggered by events like Cloud Scheduler messages. ([Cloud Functions Docs](https://cloud.google.com/functions))
- **Cloud Pub/Sub:** A messaging service that acts as a bridge between Cloud Scheduler and Cloud Functions. ([Cloud Pub/Sub Docs](https://cloud.google.com/pubsub))
- **Cloud Scheduler:** Schedules tasks like triggering Cloud Functions to manage VMs. ([Cloud Scheduler Docs](https://cloud.google.com/scheduler/docs))
- **Cloud IAM (Identity and Access Management):** Manages permissions for service accounts used in the process. ([Cloud IAM Docs](https://cloud.google.com/iam/docs/overview))

## Documentation References:

Throughout the lab, we'll reference relevant GCP documentation links to provide deeper understanding of each service and its functionalities:
- Stopping and restarting VMs: [Compute Engine Docs - Schedule Instance Start Stop](https://cloud.google.com/compute/docs/instances/schedule-instance-start-stop)
- Creating Cloud Functions: [Cloud Functions Docs - Console Quickstart](https://cloud.google.com/functions/docs/console-quickstart)
- Cloud Scheduler overview: [Cloud Scheduler Docs](https://cloud.google.com/scheduler/docs)

Let's get started!

We'll follow these steps:

1. Create VMs with Labels (Compute Engine)
2. Create a Cloud Function (Cloud Functions)
3. Implement Cloud Function Code (Optional - Python)
4. Create a Cloud Scheduler Job (Cloud Scheduler)
5. Test and Monitor the Setup

# Steps:

1. **Create VMs:**

   - Go to the GCP Console and navigate to the "Compute Engine" section.
   - Click "Create instance" to spin up two VMs (or more if you want to test different groups). Choose a small machine type like "e2-micro" to minimize costs during testing.
   - Labeling VMs:
     - In the "Labels" section, create a key-value pair to differentiate your VMs. For example, label one VM group with "env=dev" and the other with "env=prod". This label will be used for scheduling.
   - Click "Create" to launch your VMs.

2. **Create a Cloud Function:**

   - Go to the "Cloud Functions" section in the GCP Console.
   - Click "Create Function" and give it a descriptive name like "vm-control".
   - **Trigger:**
     - Under "Trigger," choose "Cloud Pub/Sub" as the event type. This service will send messages to trigger the function.
     - Give your Pub/Sub topic a name (e.g., "vm-control-topic"). This topic will receive messages from Cloud Scheduler.
   - **Runtime:**
     - Select "Python 3" as the runtime environment.

3. **Cloud Function Code:**

   - Paste the following code into the function editor, replacing <PROJECT_ID> with your actual project ID.
  
```python
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

# Explanation of the Code:

This code defines a function `vm_control` that receives a message from Cloud Pub/Sub containing instructions (stop or start) and a label for identification.
- It retrieves a list of VMs in your project and iterates through them.
- It checks if the VM's label ("env") matches the filter from the message.
- Based on the action ("stop" or "start"), it uses the Compute Engine API to manage the VMs.

4. **Create a Cloud Scheduler Job:**

   - Go to the "Cloud Scheduler" section in the GCP Console.
   - Click "Create job" and give it a clear name like "vm-scheduler".
   
   **Target:**
   
   - Under "Target," choose "HTTP" for the target type.
   - Enter the URL of your Cloud Function. It should look something like https://<region>-<project-id>.cloudfunctions.net/vm-control.
   
   **Authorization:**
   
   - In the "Authorization" section, select "IAM service account" and choose the same service account you used in the Cloud Function code. This gives the scheduler permission to call your function.
   
   **Schedules:**
   
   - Define two schedules:
     - **Stop VMs (dev):** Set a schedule for stopping VMs with the "env=dev" label (e.g., weekdays at 5:00 PM).
     - **Start VMs (prod):** Set a schedule for starting VMs with the "env=prod" label (e.g., weekdays at 9:00 AM).

# Security Considerations in Automating VM Start/Stop with GCP Services

While automating VM start/stop offers convenience and cost benefits, it's crucial to consider security implications. Here's a breakdown of potential security issues and how this lab setup can address them, aligned with the CIA triad (Confidentiality, Integrity, and Availability):

## Confidentiality:

- **Concern:** Inadvertent exposure of sensitive data on VMs during stop/start cycles.
- **Mitigation:** This lab doesn't involve accessing VM data. However, ensure VMs are properly secured with firewalls and access controls to restrict unauthorized access even during startup.

## Integrity:

- **Concern:** Potential for unauthorized modification of VM configurations or data during stop/start.
- **Mitigation:** Cloud Functions use IAM permissions to restrict actions to authorized services. The Cloud Function code only stops/starts VMs based on labels and doesn't modify configurations.

## Availability:

- **Concern:** Failure of Cloud Scheduler job or Cloud Function code could lead to VM unavailability.
- **Mitigation:** Cloud Scheduler offers monitoring and error reporting capabilities. You can configure alerts to notify IT if a job fails to execute.

## Additional Considerations:

- **Service Account Permissions:** Grant Cloud Functions only the minimum permissions required to stop/start VMs. Avoid overly broad permissions.
- **Code Security:** If modifying the Cloud Function code, follow secure coding practices to prevent vulnerabilities.
- **Monitoring and Alerting:** Set up monitoring for VM health and status. Integrate Cloud Monitoring with alerting services to notify IT if a VM fails to start up after shutdown or becomes unhealthy after a restart.

## Tracking and Monitoring Capabilities:

- **Cloud Monitoring:**
  - Use Cloud Monitoring to track VM health and performance metrics.
  - Set up alerts to notify IT if metrics deviate from expected values, indicating potential issues after a restart.
- **Cloud Logging:**
  - Cloud Functions and Cloud Scheduler log execution details.
  - You can monitor these logs to track successful/failed executions and troubleshoot issues.

### Example: Monitoring for Startup Failures

- In Cloud Monitoring, create an alert policy for the "instance uptime" metric of your VM group.
- Set the alert condition to trigger if the VM doesn't come online within a reasonable timeframe (e.g., 5 minutes) after a scheduled start.
- Configure the alert to send notifications to the IT team via email, SMS, or other channels.

This approach ensures the IT team is alerted if a VM fails to start as expected, allowing them to investigate and take corrective action.

By following these security best practices and incorporating monitoring and alerting mechanisms, you can automate VM start/stop with a good level of confidence in maintaining the CIA triad for your VMs.

# Setting Up Logging and Monitoring for VM Start/Stop Automation (GCP Console)

This guide details configuring logging and monitoring for the VM start/stop automation process we built using GCP services. We'll focus on Cloud Monitoring and Cloud Logging to track VM health and Cloud Function/Scheduler job executions.

## Prerequisites:

- Completed Lab steps for creating VMs, Cloud Functions, and Cloud Scheduler jobs.
- Familiarity with navigating the GCP Console.

## 1. Cloud Monitoring for VM Health:

- Go to the GCP Console and navigate to [Cloud Monitoring](https://support.google.com/cloud/answer/6256558?hl=en).
- **Create a Metric:**
  - Click "Create metric descriptor" and choose "VM uptime" as the metric type.
  - In "Resource type," select "gce_instance."
  - In "Labels," add a label filter based on your VM group (e.g., "env=dev"). This ensures the metric tracks uptime only for VMs with the specified label.
  - Click "Create" to define the metric.
- **Create an Uptime Alert:**
  - Click on "Alerting" in the left menu.
  - Click "Create Alert Policy."
  - Give your policy a descriptive name (e.g., "VM Startup Failure Alert").
  - In "Metric," select the "VM uptime" metric you created.
  - Set the condition to trigger an alert if the uptime is below 1 for a duration exceeding a reasonable timeframe after a scheduled start (e.g., 5 minutes).
  - Under "Notification channels," choose an option to send alerts to your IT team (e.g., email or SMS).
  - Click "Save" to create the alert policy.

## 2. Cloud Logging for Cloud Functions and Scheduler:

- Go to the GCP Console and navigate to [Cloud Logging](https://cloud.google.com/products/operations).
- **Filters:**
  - By default, you'll see logs from all projects and resources. To focus on relevant logs, use filters in the search bar.
  - For Cloud Functions logs, filter by resource type "cloud_function" and function name (e.g., "resource.type=cloud_function AND resource.labels.function_name=vm-control").
  - For Cloud Scheduler job logs, filter by resource type "cloud_scheduler_job" and job name (e.g., "resource.type=cloud_scheduler_job AND resource.labels.job_name=vm-scheduler").
- **Sinks (Optional):**
  - You can optionally configure Sinks to export logs to external destinations like SIEM tools for further analysis.
  - Click on "Sinks" in the left menu and explore available options based on your needs.

## 3. Monitoring the Console:

- The Cloud Monitoring dashboard provides an overview of your VM uptime metrics.
- You can monitor the Logs Explorer for Cloud Function and Scheduler job executions to identify any errors or unexpected behavior.
- The alert policy you created will notify the IT team via the chosen channel if a VM fails to start within the specified timeframe after a scheduled start.

## Additional Tips:

- Explore creating custom dashboards in Cloud Monitoring to visualize relevant metrics for VM health and Cloud Function executions.
- Consider using Cloud Error Reporting for detailed error reporting from Cloud Functions.
- Regularly review logs and monitor alerts to ensure the automation process functions as expected.

By following these steps, you've established a logging and monitoring framework to track the health and activity of your VM start/stop automation using Cloud Monitoring and Cloud Logging within the GCP Console. This allows you to proactively identify potential issues and maintain the overall reliability of your system.

