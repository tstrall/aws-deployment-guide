# AWS Deployment Guide

## 🧠 Core Design Principles

This project is guided by a fabric of interwoven ideas that enable scalable, secure, and flexible AWS infrastructure.

Each principle supports the others, forming a composable system where infrastructure, configuration, and services can evolve independently — yet always remain connected.

These principles include:

- **Build Once, Deploy Anywhere** – Reusable Terraform components across all environments.
- **Configuration Is the Source of Truth** – Git-controlled configs drive every deployment.
- **Immutable Infrastructure** – Replace rather than patch, enabling reliable rollouts.
- **Dynamic Dependency Resolution** – Services discover dependencies at runtime via nicknames.
- **Separation of Concerns** – Infra, config, and app code live in independent repos.
- **External System Referencing** – Reference systems you didn’t create as first-class citizens.
- **Git as the Gatekeeper** – Only what’s defined in Git gets deployed.
- **Optional Smart Caching** – Runtime refresh logic when failures occur.
- **LocalStack-Friendly by Default** – Develop and test locally with minimal cost.

➡️ [View the full explanation »](docs/design-principles.md)

## **Overview**  
This repository explains how to implement a **Configuration-Driven AWS Deployment Model**, allowing you to:  
- **Build AWS infrastructure once, then deploy dynamically via configuration updates.**  
- **Separate infrastructure (IaC), configuration (JSON), and application code (Lambdas).**  
- **Resolve dependencies dynamically using AWS Parameter Store, eliminating hardcoded references.**  
- **Ensure security and auditability by managing deployments through Git.**  

## **📂 How the Repositories Work Together**  
This model requires **three Git repositories** that interact to create a fully managed AWS environment:  

1️⃣ **[`aws-iac`](https://github.com/tstrall/aws-iac)** – Terraform-based Infrastructure as Code (VPC, RDS, IAM, etc.).  
2️⃣ **[`aws-config`](https://github.com/tstrall/aws-config)** – JSON-based configuration defining what gets deployed.  
3️⃣ **[`aws-lambda`](https://github.com/tstrall/aws-lambda)** – Independent Lambda functions that dynamically resolve dependencies.  

This repository (**aws-deployment-guide**) serves as the **documentation hub**, providing a step-by-step guide for using these repos together.

---

## **🔗 Quick Links**
- **[IaC Repo: aws-iac](https://github.com/tstrall/aws-iac)** – Terraform modules for AWS infrastructure.  
- **[Config Repo: aws-config](https://github.com/tstrall/aws-config)** – Deployment configurations in JSON.  
- **[Lambda Repo: aws-lambda](https://github.com/tstrall/aws-lambda)** – Independently deployed Lambda functions.  

---

## **📖 How It Works**
1. **Terraform infrastructure modules** in **`aws-iac`** expect input from AWS Parameter Store.  
2. **Configuration files in `aws-config`** define what gets deployed.  
3. **A CI/CD pipeline syncs the config repo with AWS Parameter Store**, ensuring that Terraform only deploys **pre-approved** components.  
4. **Lambda functions in `aws-lambda` resolve dependencies dynamically**, making them **independent of Terraform deployments**.  

---

## **🚀 Why Use This Approach?**
✅ **You only build once – all deployments are managed via config changes.**  
✅ **No Terraform knowledge needed to manage environments.**  
✅ **Fully auditable – everything is version-controlled in Git.**  
✅ **Parallel deployments and feature branches are supported.**  
✅ **Infrastructure, application code, and deployments are fully decoupled.**  

---

## **🔧 Setting Up**
### **1️⃣ Clone the Repositories**
```sh
git clone https://github.com/tstrall/aws-deployment-guide.git
git clone https://github.com/tstrall/aws-iac.git
git clone https://github.com/tstrall/aws-config.git
git clone https://github.com/tstrall/aws-lambda.git
```

### **2️⃣ Deploy the Initial Infrastructure**
Ensure the config repo has the necessary entries before applying Terraform:
```sh
cd aws-iac/components/vpc
terraform init
terraform apply -var="nickname=main-vpc"
```

### **3️⃣ Deploy Lambda Services Independently**
```sh
cd aws-lambda/user-auth-service
./deploy.sh
```

---

## **🔐 Security & Compliance**
- **All deployments are controlled through Git**, ensuring version history and approvals.  
- **IAM policies can restrict who modifies `/aws/.../config` entries**, ensuring **only authorized changes** get deployed.  
- **Secrets are stored in AWS Secrets Manager, keeping sensitive information secure.**  
---

## 🧠 Project Background

This project represents independent work and architectural ideas I’ve developed over the years, some of which originated as conceptual proposals or rejected initiatives in prior roles. The implementations are entirely original, built from scratch outside of any employment context.

---

## **📌 Next Steps**
Want to implement this in your AWS environment? Here’s what to do next:  
1️⃣ **Fork and customize the repos** to fit your organization’s needs.  
2️⃣ **Set up a CI/CD pipeline** to automate syncing config updates to AWS Parameter Store.  
3️⃣ **Define IAM policies** to ensure secure access control.  

📩 **Questions? Reach out or contribute!**  
This is an open-source approach, and improvements are always welcome.  

---

📢 **Like this approach? Star the repo and follow for updates!** 🚀  
