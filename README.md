![KubeWizard Logo](https://github.com/KlementXV/KubeWizard/blob/main/logo.jpg?raw=true)

# KubeWizard
This Ansible role is designed to **install Kubernetes** using KubeWizard on specified hosts.

## Prerequisites

Make sure the following prerequisites are met before using this role:
- Target hosts must be accessible via SSH.
- Ansible must be installed on the system from which you execute this role.
- Superuser (root) privileges on target hosts are required to install Kubernetes.

## Usage

1. Clone this repository into your Ansible project:

```bash
git clone <repository_url>
```

2. Use the role in your Ansible playbook by specifying the necessary variables:
```yaml
- name: Install Kubernetes with KubeWizard
  hosts: all
  become: yes
  vars:
    VERSION: "1.28"
    POD_CIDR: "192.168.0.0/16"
    kubeadm_init_options:
      pod-network-cidr: "{{ POD_CIDR }}"
  roles:
    - KubeWizard
```

Ensure to adjust the variables according to your needs. For example, you can specify a different version of Kubernetes by modifying the value of the VERSION variable.

3. Run your Ansible playbook:
```bash
ansible-playbook -i inventory playbook.yml
```
Make sure to replace `inventory` with the path to your inventory file and `playbook.yml` with the name of your playbook.

## Variables

The following variables can be defined to customize the installation:
- VERSION: The version of Kubernetes to install.
- POD_CIDR: The CIDR of the network for Kubernetes pods.
- kubeadm_init_options: Additional options to pass to the `kubeadm init` command.

## License
This Ansible role is distributed under the MIT License. Refer to the LICENSE file for more details.