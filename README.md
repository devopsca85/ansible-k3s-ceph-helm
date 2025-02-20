## Setup and Usage
1. Clone the repository:
```sh
git clone https://github.com/devopsca85/ansible-k3s-ceph-helm.git
cd ansible-k3s-ceph-helm
```
2. Configure inventory`inventory/hosts.yml`.
3. Encrypt sensitive data using Ansible Vault:
```sh
ansible-vault encrypt vault/secrets.yml
ansible-vault encrypt vault/host_password.yml
```
host_password.yml file containes ssh password for host and secrets.yml containes ceph admin password/k3s token

4. Run the playbook:
```sh
ansible-playbook -i inventory/hosts.yml playbook.yml --ask-vault-pass
```

## Contribution
Feel free to fork and submit PRs for improvements.

## License
This project is licensed under the MIT License.

