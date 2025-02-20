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

## Directory Structure
```sh
ansible/
├── inventory/
│   └── hosts.yml               # Ansible inventory (groups & hosts)
├── roles/
│   ├── basics/
│   │   └── tasks/
│   │       └── main.yml        # Tasks for the "basics" role under this we are executing apt update command on hosts
│   ├── ceph/
│   │   ├── handlers/
│   │   │   └── main.yml        # Handlers for the "ceph" role
│   │   ├── tasks/
│   │   │   ├── configure_ceph.yml  # Additional Ceph configuration tasks
│   │   │   └── main.yml            # Main tasks entry point for "ceph" role
│   │   └── vars/
│   │       └── main.yml        # Default or role-specific variables for Ceph
│   ├── helm/
│   │   ├── tasks/
│   │   │   ├── helm_install.yml    # Task file to install/configure Helm
│   │   │   └── main.yml           # Main tasks entry point for "helm" role
│   │   └── vars/
│   │       └── main.yml        # Variables for Helm setup
│   └── k3s/
│       ├── tasks/
│       │   ├── k3s_install.yml     # Task file for installing K3s
│       │   ├── kubectl_configure.yml # Task file for configuring kubectl
│       │   └── main.yml            # Main tasks entry point for "k3s" role
│       └── vars/
│           └── main.yml        # Variables for the K3s role
├── vault/
│   ├── host_password.yml       # Encrypted or sensitive host credentials
│   └── secrets.yml             # Other sensitive variables/secrets
├── ansible.cfg                 # Project-level Ansible configuration
├── playbook.yml                # Top-level playbook (could import or include role playbooks)
├── README.md                   # Documentation describing your setup
```
---

## Explanation of Each Section

1. **`inventory/`**  
   - **`hosts.yml`**: Defines which servers/hosts belong to each group, e.g. `[anscible_client]` etc.

2. **`roles/`**  
   - Each role is a self-contained collection of Ansible content for a specific component (e.g., Ceph, Helm, K3s):
     - **`tasks/`**: Contains YAML files with step-by-step Ansible tasks.  
       - Usually, `main.yml` is the primary entry point. Additional task files (like `configure_ceph.yml`) can be included or imported from `main.yml`.
     - **`vars/`**: Defines role-specific variables that can be set here or overridden elsewhere.
     - **`handlers/`**: Contains “handler” tasks that are typically triggered by `notify:` statements (e.g., restarting a service after configuration changes).

   ### Example:
   - **`roles/ceph/`**:
     - **`tasks/main.yml`** Entrypoint for Ceph packages and do initial configuration.
     - **`tasks/configure_ceph.yml`** could handle specialized steps like creating pools, managing CRUSH map, etc.
     - **`handlers/main.yml`** might define a handler to restart Ceph services.
     - **`vars/main.yml`** store default Ceph version, repository URLs, or cluster settings.

3. **`vault/`**  
   - Contains encrypted files (using Ansible Vault) or plain text containing sensitive data:
     - **`host_password.yml`**, **`secrets.yml`**: Credentials or tokens used by your roles.

4. **`ansible.cfg`**  
   - Local Ansible configuration overriding defaults (e.g., inventory path, privilege escalation method, vault password file, etc.).

5. **`playbook.yml`**  
   - A top-level playbook that orchestrates multiple roles/hosts. For instance, it might reference all roles in a specific order or delegate tasks to different host groups:
     ```yaml
     - hosts: all
       roles:
         - basics
         - ceph
         - helm
         - k3s
       ...
     ```
   - You can also keep multiple playbooks and choose which one to run (`ansible-playbook <playbook> -i inventory/hosts.yml`).

6. **`README.md`**  
   - Documentation for developers or operators describing how to use your playbooks, any prerequisites, and how to set up the environment.

---

## Using This Structure

1. **Update Inventory**  
   - Add or remove hosts in `inventory/hosts.yml`.
2. **Configure Variables**  
   - Use `vars/main.yml` or separate group/host vars files to store or override defaults.
3. **Run the Playbook**  
   - Execute:  
     ```bash
     ansible-playbook -i inventory/hosts.yml playbook.yml --ask-vault-pass
     ```
4. **Add More Roles or Tasks**  
   - If you need additional roles (for other services), create a new directory under `roles/`.
5. **Use Vault for Secrets**  
   - Keep passwords or tokens in `vault/` and encrypt them with `ansible-vault encrypt <file>`.

This structure gives each role its own clear home for tasks, variables, and handlers, while your single **`playbook.yml`** orchestrates how each role is applied to the environment.
    
 
