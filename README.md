# Ansible Role: Terraform

Ansible role for installing [Hashicorp Terraform](https://www.terraform.io/) CLI.

[![Build Status](https://www.travis-ci.org/PyratLabs/ansible-role-terraform-controller.svg?branch=master)](https://www.travis-ci.org/PyratLabs/ansible-role-terraform-controller)

## Requirements

This role has been tested on Ansible 2.7.0+ against the following Linux Distributions:

  - Amazon Linux 2
  - CentOS 8
  - CentOS 7
  - Debian 10
  - Fedora 29
  - Fedora 30
  - Fedora 31
  - Ubuntu 18.04 LTS

## Disclaimer

If you have any problems please create a GitHub issue, I maintain this role in
my spare time so I cannot promise a speedy fix delivery.

## Role Variables


| Variable                            | Description                                                                    | Default Value             |
|-------------------------------------|--------------------------------------------------------------------------------|---------------------------|
| `terraform_version`                 | Use a specific version of Terraform, eg. `0.11.2`. Specify `false` for latest. | `false`                   |
| `terraform_install_os_dependencies` | Allow role to install OS dependencies.                                         | `false`                   |
| `terraform_install_dir`             | Installation directory for Terraform.                                          | `$HOME/bin`               |
| `terraform_projects_dir`            | Directory to put Terraform projects. Specify `false` to skip.                  | `$HOME/projects`          |
| `terraform_projects`                | List of Terraform projects to be cloned with `git`. See notes.                 | _NULL_                    |

## Dependencies

No dependencies on other roles.

## Example Playbook

Example playbook for installing to single user:

```yaml
- hosts: control_hosts
  roles:
     - { role: xanmanning.terraform, terraform_version: 0.11.14 }
```

Example playbook for installing the latest Terraform version globally:

```yaml
---
- hosts: control_hosts
  become: true
  vars:
    terraform_install_os_dependencies: true
    terraform_install_dir: /opt/terraform/bin
    terraform_projects_dir: /opt/terraform/projects
  roles:
    - role: xanmanning.terraform
```

### Note about `terraform_projects`

This is a list of git repositories to be cloned into the projects directory.
If this is empty, no projects will be cloned.

Below is an example of a project:

```yaml
terraform_projects:
    - name: terraform-template                                 # Directory name to clone into
      repo: git@github.com:AustinCloudGuru/terraform-skeleton  # Repository to clone
      update_repo: true                                        # Always update local copy of repo
      version:  master                                         # Check out this version of the repo
      force: false                                             # Discard any existing working copy of the repo
      key_file: "{{ ansible_user_dir }}/.ssh/id_rsa"           # Key file to use to clone the repo
      recursive: true                                          # Include submodules in clone
```

## License

BSD

## Author Information

[Xan Manning](https://xanmanning.co.uk/)
