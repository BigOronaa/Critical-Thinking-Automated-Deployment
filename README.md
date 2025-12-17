# Critical Thinking Automated Deployment of Ansible Playbooks using GitHub Actions

## Project Overview
This project demonstrates an automated infrastructure deployment workflow on **AWS** using **Ansible** and **GitHub Actions**. Every time code is pushed to the `main` branch, a GitHub Actions pipeline is triggered to provision and configure infrastructure resources automatically.

The project simulates the responsibilities of a **DevOps Engineer**, focusing on Infrastructure as Code (IaC), CI/CD automation, and secure cloud integration.

---

## Objectives
The main goals of this project are:

- Automate AWS infrastructure deployment using Ansible
- Trigger deployments automatically via GitHub Actions
- Provision and configure EC2 instances
- Create and mount an Amazon EFS file system
- Install and configure Apache and MySQL (MariaDB)
- Securely integrate AWS credentials using GitHub Secrets
- Implement error handling and failure notifications

---

## Architecture Overview

- **GitHub Actions**: CI/CD automation
- **Ansible**: Configuration management and provisioning
- **AWS EC2**: Compute instances
- **Amazon EFS**: Shared file system
- **Apache HTTP Server**: Web server
- **MariaDB (MySQL-compatible)**: Database server

---

## Repository Structure

```
.
├── ansible/
│   ├── deploy.yml
│   ├── inventory.ini
│   └── roles/
│       ├── configure-ec2/
│       │   └── tasks/main.yml
│       └── efs/
│           └── tasks/main.yml
├── .github/
│   └── workflows/
│       └── deploy.yml
├── README.md
```

---

## Prerequisites

Before using this project, ensure you have:

- An AWS account
- A GitHub repository
- An EC2 key pair (private key stored in GitHub Secrets)

### Required GitHub Secrets

The following secrets must be configured in your GitHub repository:

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `EC2_SSH_PRIVATE_KEY`

---

## GitHub Actions Workflow

The workflow is located at:

```
.github/workflows/deploy.yml
```

### Workflow Features

- Triggers automatically on `push` to the `main` branch
- Sets up Python and Ansible environment
- Installs required Ansible collections
- Authenticates securely with AWS
- Uses SSH agent to connect to EC2
- Executes Ansible playbooks
- Fails fast on errors
- Includes failure notification handling

---

## Ansible Playbooks

### `deploy.yml`

This is the main orchestration playbook that:

1. Provisions AWS infrastructure (EFS, EC2 if applicable)
2. Configures EC2 instances
3. Calls role-based tasks for modularity

---

### Roles

#### `configure-ec2`
Handles EC2 instance configuration:

- Updates system packages
- Installs required packages (Docker, Apache, MariaDB, EFS utils)
- Starts and enables services
- Mounts Amazon EFS
- Configures Apache web server
- Installs and starts MariaDB (MySQL-compatible)

#### `efs`
Handles EFS-related tasks:

- Creates EFS file system
- Outputs EFS identifiers for use in EC2 configuration

---

## Validation Steps

After a successful GitHub Actions run, verify the following:

### EC2 Validation

```bash
ssh ec2-user@<EC2_PUBLIC_IP>
```

### EFS Validation

```bash
df -h | grep efs
```

### Apache Validation

```bash
sudo systemctl status httpd
curl http://localhost
```

### MariaDB Validation

```bash
sudo systemctl status mariadb
mysql -u root -e "SELECT VERSION();"
```

---

## Error Handling & Notifications

- The GitHub Actions workflow stops execution immediately on failure
- Failure steps are logged clearly in the Actions console
- Notification logic can be extended (Slack, Email, etc.)

---

## CI/CD Flow Summary

1. Developer pushes code to `main`
2. GitHub Actions workflow triggers automatically
3. AWS credentials are securely injected
4. Ansible playbooks execute
5. Infrastructure is provisioned and configured
6. Errors are caught and reported

---

## Conclusion

This project demonstrates a complete DevOps automation pipeline using **Ansible** and **GitHub Actions**. It covers infrastructure provisioning, configuration management, secure cloud integration, and CI/CD best practices.

It is suitable for showcasing real-world DevOps skills in:

- Infrastructure as Code
- Cloud automation
- CI/CD pipelines
- AWS resource management

---



