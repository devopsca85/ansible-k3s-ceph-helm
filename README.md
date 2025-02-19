## Setup and Usage
1. Clone the repository:
```sh
git clone https://github.com/devopsca85/ansible-k3s-ceph-helm.git
cd ansible-k3s-ceph-helm
```
2. Configure inventory and variables in `inventory/hosts.yml` and `group_vars/all.yml`.
3. Encrypt sensitive data using Ansible Vault:
```sh
ansible-vault encrypt vault.yml
```
4. Run the playbook:
```sh
ansible-playbook -i inventory/hosts.yml playbook.yml --ask-vault-pass
```

## Contribution
Feel free to fork and submit PRs for improvements.

## License
This project is licensed under the MIT License.

