# 🚀 Three-Tier Application Deployment with AWS, Terraform & Jenkins

## 🔍 Overview

This project showcases a complete DevOps pipeline for deploying a **three-tier web application** (React frontend, Node.js backend, MongoDB database) on **AWS** using **Terraform for IaC** and **Jenkins for CI/CD automation**.

---

## 🏗️ Architecture

The application architecture includes:

1. **Frontend (React)**
   Hosted on EC2 behind an **Application Load Balancer**.
2. **Backend (Node.js API)**
   Runs on a separate EC2 instance.
3. **Database (MongoDB)**
   Installed on a dedicated EC2 instance.

Each tier is isolated using AWS **Security Groups** and deployed within the **default VPC**.

---

## 📁 Project Structure

```
Three-Tier-Deployment/
├── terraform/
│   ├── main.tf                # AWS provider and VPC data sources
│   ├── compute.tf             # EC2 instances & Load Balancer
│   ├── security.tf            # Security groups
│   ├── variables.tf           # Variable definitions
│   ├── outputs.tf             # Output values
│   ├── terraform.tfvars       # Deployment-specific variables
│   └── scripts/               # Bootstrap scripts
│       ├── mongodb_setup.sh
│       ├── backend_setup.sh
│       └── frontend_setup.sh
├── jenkins/
│   └── Jenkinsfile            # Jenkins pipeline
└── README.md
```

---

## ⚙️ Jenkins CI/CD Pipeline

Defined in the `Jenkinsfile`, the pipeline includes:

* **Checkout**: Pull code from the repo
* **Terraform Init & Validate**
* **Deploy Infrastructure** with `terraform apply`
* **Smoke Tests** for backend and frontend
* **Security Scans** (optional stage)

---

## 🔐 Security Highlights

- Security groups are configured to restrict access between tiers
- Sensitive information is stored in AWS Parameter Store
- SSH access is limited (should be further restricted in production)
- Network isolation is implemented between application tiers

---
## 🚀 How to Deploy

### Prerequisites

- AWS account with appropriate permissions
- Terraform installed (v1.0.0 or later)
- Jenkins server with AWS and Terraform plugins
- SSH key pair for EC2 instances

### Manual Deployment

1. Clone the repository
2. Navigate to the terraform directory
3. Create a `terraform.tfvars` file with your specific values
4. Initialize Terraform: `terraform init`
5. Plan the deployment: `terraform plan -out=tfplan`
6. Apply the configuration: `terraform apply tfplan`

### Automated Deployment with Jenkins

1. Set up a Jenkins server with necessary plugins
2. Configure AWS credentials in Jenkins
3. Create a new pipeline job pointing to the repository
4. Run the pipeline

---

## 🌐 Accessing the App

* **Frontend**: `http://<ALB-DNS>` or `http://<frontend-ec2-ip>:3000`
* **Backend API**: `http://<backend-ec2-ip>:3001`

---

## 🧹 Cleanup

Destroy all AWS resources:

```bash
cd terraform
terraform destroy -auto-approve
```

---

## 🔭 Future Enhancements

* Enable **Auto Scaling** on frontend/backend
* Add **HTTPS with ACM + ALB**
* **Secure MongoDB** with Secrets Manager
* Implement **CloudWatch Monitoring & Alerts**
* Use a **custom VPC** with public/private subnets

## Assumptions and Decisions

- Used the default VPC for simplicity, but in a production environment, a custom VPC would be preferred
- Stored MongoDB credentials in plaintext for demonstration; in production, use AWS Secrets Manager
- Used t2.micro instances for cost-effectiveness; adjust based on workload requirements
- Frontend and backend are deployed as systemd services for automatic restart