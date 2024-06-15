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
git clone https://github.com/KlementXV/KubeWizard.git
```

2. Use the role in your Ansible playbook:
```yaml
- name: Install Kubernetes with KubeWizard
  hosts: all
  roles:
    - KubeWizard
```

3. Retrieve and modify the `group_vars` as needed. Example `group_vars` can be found in the `inventory` directory:
```yaml
kubernetes_version: "v1.28"
crio_project_path: "prerelease:/main"
loadbalancer_apiserver_port: 6443

kubeadm_init_options:
  #pod-network-cidr: "192.168.0.0/16"
  #control-plane-endpoint: "kubeapi.local"
  #apiserver-advertise-address: "192.168.0.1"
  #apiserver-cert-extra-sans: "kubeapi.local"
  #kubernetes-version: "{{ kubernetes_version | regex_replace('^v', '') }}"
  #apiserver-bind-port: "6443"
  #cert-dir: "/etc/kubernetes/pki"
  #certificate-renewal: "true"
  #config: "/path/to/config.yaml"
  #cri-socket: "/var/run/dockershim.sock"
  #dry-run: "false"
  #feature-gates: "AllAlpha=false"
  #ignore-preflight-errors: "all"
  #image-repository: "k8s.gcr.io"
  #node-name: "my-node"
  #service-cidr: "10.96.0.0/12"
  #service-dns-domain: "cluster.local"
  #skip-certificate-key-print: "false"
  #skip-phases: "preflight"
  #token: "abcdef.0123456789abcdef"
  #token-ttl: "24h0m0s"
  #upload-certs: "true"

# Kube VIP
#kube_vip_enabled: false
#kube_vip_arp_enabled: false
#kube_vip_controlplane_enabled: false
#kube_vip_address: 192.168.1.2
#loadbalancer_apiserver:
#  address: "{{ kube_vip_address }}"
#  port: "{{ loadbalancer_apiserver_port }}"
#kube_vip_interface: eth0
#kube_vip_services_enabled: false
#kube_vip_dns_mode: first
#kube_vip_cp_detect: false
#kube_vip_leasename: plndr-cp-lock
#kube_vip_enable_node_labeling: false

helm_enabled: false

calico_enabled: false
# calico_repo: "https://docs.tigera.io/calico/charts"
## The following namespaces cannot be used: [calico-system calico-apiserver tigera-system tigera-elasticsearch tigera-compliance tigera-intrusion-detection tigera-dpi tigera-eck-operator tigera-fluentd calico-system tigera-manager]
# calico_namespace: "tigera-operator"
# calico_version: "v3.28.0"

longhorn_enabled: false
# longhorn_repo: "https://charts.longhorn.io"
# longhorn_namespace: "longhorn-system"
# longhorn_version: "1.6.2"
```

4. Modify the inventory file as needed:
```ini
[all]
master1 ansible_host=xxx.xxx.xxx.xxx ansible_ssh_private_key_file=~/.ssh/id_rsa
master2 ansible_host=xxx.xxx.xxx.xxx ansible_ssh_private_key_file=~/.ssh/id_rsa
master3 ansible_host=xxx.xxx.xxx.xxx ansible_ssh_private_key_file=~/.ssh/id_rsa
worker1 ansible_host=xxx.xxx.xxx.xxx ansible_ssh_private_key_file=~/.ssh/id_rsa
worker2 ansible_host=xxx.xxx.xxx.xxx ansible_ssh_private_key_file=~/.ssh/id_rsa

[kube_control_plane]
master1
master2
master3

[kube_node]
worker1
worker2

[k8s_cluster:children]
kube_control_plane
kube_node
```

5. Run your Ansible playbook:
```bash
ansible-playbook -i inventory playbook.yml
```
Make sure to replace `inventory` with the path to your inventory file and `playbook.yml` with the name of your playbook.

## Variables

The following variables can be defined to customize the installation:
- `kubernetes_version`: The version of Kubernetes to install.
- `crio_project_path`: The CRI-O project path.
- `loadbalancer_apiserver_port`: The port for the API server load balancer.
- `kubeadm_init_options`: Additional options to pass to the `kubeadm init` command.

## License

This Ansible role is distributed under the MIT License. Refer to the LICENSE file for more details.