# jenkins-ansible-pipeline

This project demonstrates a Jenkins Pipeline that integrates with Ansible for automated deployment over SSH.

## Structure

- `Jenkinsfile`: Defines the Jenkins Pipeline with stages for checkout, shell practice, and running Ansible.
- `inventory/`: Contains the Ansible inventory file.
- `playbooks/`: Ansible playbooks (legacy, now using roles).
- `roles/common/`: Ansible role for common tasks like system update, installing tools.

## Usage

1. Set up Jenkins with Ansible and SSH configured.
2. Configure the inventory with your target hosts.
3. Run the pipeline.

## Requirements

- Ansible
- SSH access to target machines
- Jenkins with Pipeline plugin