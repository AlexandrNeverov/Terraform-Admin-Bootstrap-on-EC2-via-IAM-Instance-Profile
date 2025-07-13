# Admin Terraform Bootstrap EC2 🛠️

[![Admin Terraform Bootstrap EC2 Demo](https://raw.githubusercontent.com/AlexandrNeverov/Terraform-Admin-Bootstrap-on-EC2-via-IAM-Instance-Profile/main/image.png)](https://www.youtube.com/watch?v=XXXXXXXXXXX)

## 🚀 Why This Matters

Getting started with Terraform on AWS from a fresh EC2 instance can be tedious — especially when needing admin permissions, a secure remote backend (S3 + DynamoDB), and full CLI-based automation.

**`admin-terraform-bootstrap-ec2` automates the entire bootstrap pipeline** with preconfigured IAM role, Terraform tools, and remote state backend. All this — without manually creating users or assigning policies!

Perfect for:

- Secure, minimal EC2-based Terraform environments
- Local-free infrastructure setups (pure CLI / instance-based)
- Teaching and showcasing DevOps practices

---

## ✅ Features

- EC2 launch with admin-level IAM role via **Instance Profile**
- Terraform, AWS CLI, Git, Python, jq, htop, etc. installed automatically
- Fully provisioned remote backend:
  - S3 bucket (with versioning)
  - DynamoDB table (for state locking)
- Shows currently attached IAM policies via script
- Infrastructure as code mindset from the first step

---

## 🛠️ Scripts Overview

| Script                                        | Purpose                                                        |
|-----------------------------------------------|----------------------------------------------------------------|
| `infra-bootstrap/aws-zero-node-bootstrap.sh` | Launch EC2 with IAM role and security group                    |
| `bootstrap-zero-node-tools.sh`               | Install Terraform, AWS CLI, Git, etc.                          |
| `setup-terraform-s3-dynamodb.sh`             | Create S3 bucket and DynamoDB table for backend +  Check if the EC2 IAM role has AdministratorAccess attached              |

---

## 📜 Script: check-iam-role-policies.sh

```bash
#!/bin/bash

ROLE_NAME="TerraformRunnerRole"

echo "Checking attached policies for IAM role: $ROLE_NAME"

aws iam list-attached-role-policies \
  --role-name "$ROLE_NAME" \
  --query "AttachedPolicies[*].PolicyName" \
  --output table
```

---

## 🚀 Quick Start

1. ✅ Launch EC2 with admin IAM role via AWS CLI:
```bash
bash infra-bootstrap/aws-zero-node-bootstrap.sh
```

2. 🔐 Connect via SSH:
```bash
ssh -i ~/.ssh/My_mac.pem ubuntu@<PUBLIC_IP>
```

3. 🧰 Install DevOps toolchain:
```bash
bash bootstrap-zero-node-tools.sh
```

4. 🗄️ Create S3 + DynamoDB Terraform backend:
```bash
bash setup-terraform-s3-dynamodb.sh
```

5. 🔍 Check IAM role policies (optional):
```bash
bash aws iam list-attached-role-policies \
  --role-name "$ROLE_NAME"
```

---

## 📦 Terraform Remote Backend Sample

```hcl
terraform {
  backend "s3" {
    bucket         = "terraform-backend-zero-<timestamp>"
    key            = "terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-locks-zero-<timestamp>"
    encrypt        = true
  }
}
```

---

## 🧪 Requirements

- AWS CLI configured and authenticated
- SSH Key Pair (e.g., `My_mac.pem`)
- IAM permission to create:
  - EC2 instances
  - IAM roles / instance profiles
  - S3 buckets and DynamoDB tables

---

## 📄 License

MIT – free to use, modify, share.

---

## 👨‍💻 Author

**Alex Neverov**  
DevOps Engineer · Cloud & Infrastructure Automation · Industry PhD

- **GitHub:** [AlexandrNeverov](https://github.com/AlexandrNeverov)  
- **LinkedIn:** [linkedin.com/in/alexneverov](https://www.linkedin.com/in/alexneverov)  
- **Upwork:** [upwork.com/freelancers/~01c616035669bbf379](https://www.upwork.com/freelancers/~01c616035669bbf379)  
- **Website:** [neverov-it.com](https://neverov-it.com) · [neverov-science.com](https://neverov-science.com)  
- **Email:** [alex@neverov-it.com](mailto:alex@neverov-it.com)  
- **Phone:** +1 (754) 236‑5715
