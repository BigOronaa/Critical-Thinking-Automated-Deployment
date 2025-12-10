# Critical Thinking Automated Deployment

**Project:** Automated deployment of AWS infrastructure using Ansible and GitHub Actions

## Overview
This repository contains Ansible playbooks and GitHub Actions workflows to automate provisioning of EC2 instances, creation and mounting of EFS, installation of MySQL and Apache, and related configuration on AWS. GitHub Actions runs Ansible playbooks on push to the `main` branch.

## Repository layout
```
critical_thinking_automated_deployment/
├── .github/
│   └── workflows/
│       └── deploy.yml
├── playbooks/
│   ├── ec2.yml
│   ├── efs.yml
│   ├── mysql.yml
│   └── apache.yml
├── roles/
│   ├── ec2/README.md
│   ├── efs/README.md
│   ├── mysql/README.md
│   └── apache/README.md
├── .gitignore
└── README.md
```

## Quick start (local testing)

1. Clone the repository:
   ```bash
   git clone <your-repo-url>
   cd critical_thinking_automated_deployment
   ```
2. Create a Python virtualenv and install dependencies:
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   pip install --upgrade pip
   pip install ansible boto3 botocore
   ansible-galaxy collection install amazon.aws community.aws
   ```
3. Configure AWS credentials locally (for testing only):
   ```bash
   export AWS_ACCESS_KEY_ID=YOUR_KEY
   export AWS_SECRET_ACCESS_KEY=YOUR_SECRET
   export AWS_REGION=us-east-1
   ```
4. Run the provisioning playbook locally:
   ```bash
   ansible-playbook playbooks/ec2.yml
   ```

## GitHub Actions (CI) setup
1. Create a GitHub repository and push this project.
2. Add the following GitHub Secrets in your repository settings:
   - `AWS_ACCESS_KEY_ID`
   - `AWS_SECRET_ACCESS_KEY`
   - `AWS_REGION` (optional, e.g., `us-east-1`)
   - `SLACK_WEBHOOK_URL` (optional, for notifications)
3. The workflow `.github/workflows/deploy.yml` will trigger on pushes to `main` and run the Ansible playbooks.

## Notes & Recommendations
- The playbooks that manage EC2/EFS run with `hosts: localhost` and `connection: local` so Ansible's AWS modules execute from the control runner (GitHub Actions runner or your local machine).
- Be cautious with credentials and AWS permissions—use least-privilege IAM roles/policies.
- Test iteratively: start with EC2 provisioning, verify instances, then proceed to EFS and software installation.

## Next steps (we will perform these together step-by-step):
1. Create the GitHub repo and push this skeleton.
2. Configure GitHub Secrets.
3. Improve and customize the `playbooks/` files (we'll edit the EC2 playbook next).
4. Run CI and validate on AWS.

