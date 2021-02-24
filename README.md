# Ansible Terraform

This role creates a dynamic inventory with groups that can be used by Ansible. 
The groups in this inventory are coming from the Terraform output file.
See this example for the Terraform code: [`outputs.tf`](https://github.com/akutschi/terraform-digitalocean-cloudflare-droplet-firewall/blob/master/outputs.tf).

## Requirements

- This version requires Ubuntu 20.04 LTS
- Ansible 2.9.x, this role is **not tested with Ansible 2.10** yet.
- Terraform 0.14.x

## Role Variables

This role does have two variables:

| Variable | Default | Description
|-|-|-|
| `ansible_terraform_path` | `./../terraform/dev/vpn` | Path to your root Terraform root module
| `ansible_terraform_tfvars` | `../secrets.tfvars` | Path to the file that contains the secrets for Terraform. This path is relative to the one from `ansible_terraform_path`.

Following, simplified folder structure is assumed:

```
IaC
├── ansible
│   ├── playbook.yml
│   └── roles
│       ├── ansible-ssh
│       ├── ansible-terraform
│       └── ansible-update
└── terraform
    ├── dev
    │   ├── secrets.tfvars
    │   └── vpn
    │       ├── main.tf
    │       ├── output.tf
    │       └── variables.tf
    ├── module
    └── prod
```

## Dependencies

This role does not have any dependencies.

## Example Playbook

The setup is quite simple.
Just clone or download this role into your `roles` folder and set up the playbook similar to this example:

```yml
---
- name: Run Terraform and build inventory dynamically
  hosts: localhost
  roles:
    - ansible-terraform

- name: Wait for all machines to become ready
  hosts: all
  gather_facts: no
  tasks:
  - name: Wait for machine to become reachable
    wait_for_connection:
      delay: 10
      sleep: 10
```

After `ansible-terraform` is finished it is recommended to make a short break and to wait until all machines become available.

# License

GPLv3, see [license](./LICENSE).

# Contributing

Feel free to open issues or merge requests if you find problems or have ideas for improvements. Thank you.
