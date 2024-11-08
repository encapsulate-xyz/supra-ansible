# Ansible Playbook to deploy Supra RPC/Fullnode and Validator Node 

This Ansible playbook automates the deployment and configuration of Supra Fullnode/RPC and validator node. It ensures that the necessary dependencies, configuration files, and services are properly set up and running.

## Table of Contents

- [Requirements](#requirements)
- [Setup](#setup)
- [Variables](#variables)
- [Usage](#usage)

## Requirements

Before using this playbook, ensure the following requirements are met:

1. **Ansible version**: Make sure you have Ansible 2.15+ installed.
2. **SSH Access**: Passwordless SSH access to all target servers.
3. **Python**: Python 3.x installed on the control node and all target hosts.
4. **Privileges**: The user running the playbook must have sudo privileges on the target machines.

**Note**: The following ansible playbook dynamically fetches private validator and node keys from hashicorp vault. 

## Setup

### 1. Install Ansible

If Ansible is not installed, visit the official documentation for detailed instructions on how to install Ansible on various Linux distributions:

[Ansible Installation Guide](https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html)


### 2. Clone the repository

Clone this repository to your Ansible control node:

```bash
git clone https://github.com/encapsulate-xyz/supra-ansible.git
cd Supra-ansible
```

### 3. Inventory

Define your target servers' IP address or DNS in the inventory folder, and select either `mainnet.yml` or `testnet.yml` to update.

Example for testnet.yml

```yaml
---
all:
  vars:
    env: testnet
  children:
    Supra:
      hosts:
        validator.Supra.testnet.encapsulate.xyz:
          type: validator
        fullnode.Supra.testnet.encapsulate.xyz:
          type: fullnode
```

## Variables

This playbook allows customization through several variables. You can define these variables in the following locations:

- **`group_vars/all.yml`**: Contains all the port configurations.
- **`group_vars/mainnet.yml`** or **`group_vars/testnet.yml`**: Contains version-specific variables.
- **`group_vars/vault.yml`**: Store secret variables, such as `jwtsecret`, in this file.



### Usage

1. First, install the dependencies:

   ```bash
   ansible-galaxy install -r requirements.yml
   ```

2. Create a `ansible_vault_password` file containing ansible-vault password

3. Then run the playbook:

    ```bash
    ansible-playbook main.yml -l validator.supra.testnet.encapsulate.xyz
    ```

After you run the playbook, it will ask for confirmation, displaying all the variables and the IP address or DNS of the server you are going to deploy.

Example output:

```bash
TASK [Display environment being deployed to] ***************************************************************************************************
ok: [validator.supra.testnet.encapsulate.xyz] => {
    "msg": [
        "Deploying to Host: validator.supra.testnet.encapsulate.xyz",
        "Groups: ['supra']",
        "Project: supra",
        "Environment: testnet",
        "Type: validator",
        "Version: v6.3.0",
        "Username: supra",
        "Service Name: supra",
        "Operating System: linux",
        "System Architecture: amd64"
    ]
}

TASK [Confirm deployment details] **************************************************************************************************************
[Confirm deployment details]
Press 'Enter' to continue with the deployment or Ctrl+C to cancel:
ok: [validator.supra.testnet.encapsulate.xyz]

TASK [Please confirm again] ********************************************************************************************************************
ok: [validator.supra.testnet.encapsulate.xyz] => {
    "msg": [
        "Deploying to Host: validator.supra.testnet.encapsulate.xyz",
        "Project: supra",
        "Environment: testnet",
        "Type: validator"
    ]
}

TASK [Confirm deployment details] **************************************************************************************************************
[Confirm deployment details]
Press 'Enter' to continue with the deployment or Ctrl+C to cancel:
ok: [validator.supra.testnet.encapsulate.xyz]
```
