# Ansible Playbooks examples

This is a public example of an Ansible Playbook I have created. The examples in this Repo are generic and do not represent real system information.

Designed for systems running `Ubuntu 24.04` or `Debian 12.9.0` and later.


## Setup

* Ensure Ansible is installed, follow: https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html

* On Mac, Python may be externally managed with Brew. So install with `brew install ansible`

* Check your Ansible version with: `ansible --version`

* Install additional dependencies:
  * Install `sshpass` on Ubuntu 16.04 or similar: `apt-get install sshpass`
  * Install `sshpass` on Mac OS: `brew install hudochenkov/sshpass/sshpass`


## Pre-requisites

* Ensure all details/file paths etc are correct in `ansible.cfg`

* SSH Key files, some secret variables and template files are encrypted with Ansible vault. Ensure you have the correct password stored in `vault/vault.pass` to decrypt these variables.

* A custom SSH port of `2222` will be used and configured on the nodes, instead of the default port `22`.

* You will need to decrypt the Ansible management user SSH private key, ensure your password to decrypt is specified in `vault/vault.pass` as this will always be used throughout the Playbooks: `ansible-vault decrypt "ssh_keys/ansible/id_rsa"`

* The password to decrypt the Ansible Vault file is: `3KUv7GPFB9e@!j`. Use this to replace `add your password here` in `vault/vault.pass`.

* You may need to adjust the defualt user password, and root password in `group_vars/all.yaml`.



## Encrypting files with Ansible vault

If your files have not already been encrypted with Ansible vault, run these commands below:

Add your password to the vault password file in `vault/vault.pass`

### Encrypt the SSH Private keys which will be used to communicate with the servers:

* `ansible-vault encrypt "ssh_keys/ansible/id_rsa" "ssh_keys/system/id_rsa"`

### Encrypt individual passwords to a password file vault:

* `ansible-vault encrypt_string 'qn4mDx7U7jU7' --name 'management_user_password'`
* `ansible-vault encrypt_string 'kVR91uFaf0' --name 'system_password'`



# Misc Ansible commands

* `ansible-inventory -i inventory.yaml --list`
* `ansible -m ping all -i inventory.yaml`
* `ansible-inventory -i inventory.yaml --graph`



# Ansible playbooks

## General Commissioning
This targets: `all` hosts.

And also runs some Virtual Machine specific tasks for hosts grouped as `virtualmachines`.

All:
* `ansible-playbook playbooks/commissioning/all.yaml -i inventory.yaml`

If you wanted to commission a specific group or node listed in `inventory.yaml` use:
* Node: `ansible-playbook playbooks/commissioning/all.yaml -i inventory.yaml --limit 'proxmox-host01'`
* Group: `ansible-playbook playbooks/commissioning/all.yaml -i inventory.yaml --limit 'WebServers'`


## Notes

## Commissioning Notes

* Create SSH keys, store these into the folders, each with a user's name: `ssh-keygen -b 4096 -t rsa`

* Move the private and public keys into a folder which matches the username on the system, e.g. `ssh_keys/your_username_here/`


You can these login with these keys, you will need to decrypt the SSH key first:

```
ansible-vault decrypt "ssh_keys/ansible/id_rsa"
ssh -i ssh_keys/ansible/id_rsa ansible@10.0.10.103 -p2222
```

```
ansible-vault decrypt "ssh_keys/system/id_rsa"
ssh -i ssh_keys/system/id_rsa system@10.0.10.103 -p2222
```
