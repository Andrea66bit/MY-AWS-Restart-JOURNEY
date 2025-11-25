AWS Compute Lab: Secure EC2 Instance Launch and Verification

1. Overview and Scope

This hands-on lab provides a step-by-step procedure for provisioning a virtual machine instance using Amazon Elastic Compute Cloud (Amazon EC2) within the AWS Management Console. EC2 is the foundational compute service in AWS, enabling scalable and secure cloud infrastructure deployment.

The lab culminates in verifying secure connectivity and utilizing instance metadata to retrieve runtime configuration details.

Target Audience

Cloud Architects, DevOps Engineers, and System Administrators seeking practical experience with core AWS compute provisioning workflows.

Objectives

Upon completion of this lab, the user will be able to:

Successfully navigate the EC2 Launch Instance wizard in the AWS Management Console.

Select and configure essential components: Amazon Machine Image (AMI), instance type, storage, and networking (VPC/Subnet).

Establish network-level security using Security Groups (SGs).

Securely access the launched instance using a Key Pair and the EC2 Instance Connect service.

Retrieve runtime configuration data from the EC2 Instance Metadata Service (IMDS).

Properly decommission all provisioned resources to prevent accrual of charges.

2. Prerequisites and Required Configuration

Before beginning the procedure, the following resources and access permissions must be verified:

AWS Account Access: An active AWS account with appropriate permissions to provision EC2, VPC, and IAM resources.

Network Setup: An existing Amazon Virtual Private Cloud (VPC) and a public subnet with an attached Internet Gateway (IGW) are required. Default VPC settings are acceptable.

IAM Permissions: The executing IAM user or role must possess the AmazonEC2FullAccess policy (or equivalent least privilege permissions) and permissions to use AWS Systems Manager (SSM) actions, specifically ssm:GetParameters for the advanced verification step.

Key Pair: An existing SSH Key Pair in the target AWS Region, or the ability to create one during the launch process.

3. Procedure: Launching the EC2 Instance

Follow these sequential steps to launch a secure, Free Tier-eligible EC2 instance.

3.1. Initiate Instance Launch

Navigate to the EC2 Dashboard in the AWS Management Console.

In the left navigation pane, select Instances, then click the Launch instances button.

3.2. Select Configuration and Naming

Name and Tags: Provide a descriptive name for the instance (e.g., Lab-Web-Server).

Application and OS Images (AMI): Select the base operating system image. For this lab, choose a Free Tier eligible image, such as Amazon Linux 2023 AMI or Ubuntu Server 20.04 LTS.

Note: Using a standard Amazon Linux 2/2023 AMI ensures the necessary SSM Agent and EC2 Instance Connect components are pre-installed.

Instance Type: Select the t2.micro instance type to ensure Free Tier eligibility and sufficient baseline performance.

3.3. Configure Key Pair and Network Settings

Key Pair (Login): Select an existing Key Pair or choose Create new key pair. This is essential for traditional SSH connectivity.

Network Settings (VPC and Subnet):

Select the desired VPC.

Select the target Subnet (ensure it is a public subnet if a Public IP is required).

Set Auto-assign Public IP to Enable.

Security Group:

Select Create security group.

Name the Security Group (e.g., Lab-SG-Allow-SSH).

Configure the inbound rules to allow secure shell access:

Type: SSH

Protocol: TCP

Port Range: 22

Source: Restrict the source to your local public IP address (My IP) for enhanced security, or use 0.0.0.0/0 (for IPv4) if required.

3.4. Final Configuration and Launch

Configure Storage: Retain the default 8 GiB General Purpose SSD (gp3) EBS volume, or specify a volume size up to 30 GiB for Free Tier eligibility.

Advanced Details (Optional but Recommended): Utilize the User data field to inject a bootstrap script to perform initial setup (e.g., installing a web server).

Review and Launch: Review the summary pane for accuracy, ensuring all security and network configurations are correct. Click Launch instance.

4. Verification and Connection Testing

Once the instance state changes from pending to running and the Status checks pass (typically a few minutes), verify connectivity and functionality.

4.1. Connect via EC2 Instance Connect (Recommended)

EC2 Instance Connect is the preferred secure method for browser-based connectivity, eliminating the need to download and manage SSH key files locally.

On the EC2 Instances page, select the newly launched instance.

Click the Connect button.

Select the EC2 Instance Connect tab.

Specify the default username (ec2-user for Amazon Linux).

Click Connect. A secure, browser-based terminal session will open.

4.2. Verify Configuration using Instance Metadata

Within the Instance Connect session, retrieve configuration details programmatically using the Instance Metadata Service (IMDSv1).

Action 1: Retrieve Availability Zone and Region

Run the following command to retrieve the Availability Zone (AZ) where the instance is running, and extract the Region from the AZ identifier:

# Retrieve the full Availability Zone
AZ=$(curl -s [http://169.254.169.254/latest/meta-data/placement/availability-zone](http://169.254.169.254/latest/meta-data/placement/availability-zone))
echo "Availability Zone: $AZ"

# Extract the Region name by slicing the AZ string
REGION=${AZ::-1}
echo "Region: $REGION"


Action 2: Retrieve AMI ID (Simulating Parameter Store Use)

The user's original lab mentioned retrieving the AMI ID from Parameter Store, which is a common practice for standardized deployments (using Systems Manager). Assuming the base AMI ID used for launch is stored in a public or known Parameter Store key (e.g., /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2), you can simulate this verification:

# NOTE: This step assumes the required IAM role for SSM access is attached to the EC2 instance.
# Fetch the AMI ID used to launch the instance from the instance metadata
AMI_ID_METADATA=$(curl -s [http://169.254.169.254/latest/meta-data/ami-id](http://169.254.169.254/latest/meta-data/ami-id))
echo "Instance AMI ID (from Metadata): $AMI_ID_METADATA"


5. Resource Decommissioning (Cleanup)

To avoid incurring unexpected charges, all resources created during this lab must be properly terminated.

Terminate the Instance:

Navigate back to the EC2 Instances page.

Select the running instance (Lab-Web-Server).

Click the Instance state button, then select Terminate instance. Confirm the action.

Delete Dependent Resources:

After the instance status changes to terminated, delete the following resources manually if they were created specifically for this lab:

Security Group: Delete the Lab-SG-Allow-SSH security group.

Key Pair: Delete the SSH Key Pair if it is no longer required for other resources.

EBS Volume: Although the root volume is usually deleted automatically upon termination, verify that no unattached EBS volumes remain in the Elastic Block Store > Volumes section.
