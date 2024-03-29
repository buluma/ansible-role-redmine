# Ansible role [redmine](https://galaxy.ansible.com/ui/standalone/roles/buluma/redmine/documentation)

Install Redmine on CentOS

|GitHub|Version|Issues|Pull Requests|Downloads|
|------|-------|------|-------------|---------|
|[![github](https://github.com/buluma/ansible-role-redmine/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-redmine/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-redmine.svg)](https://github.com/buluma/ansible-role-redmine/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-redmine.svg)](https://github.com/buluma/ansible-role-redmine/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-redmine.svg)](https://github.com/buluma/ansible-role-redmine/pulls/)|[![Ansible Role](https://img.shields.io/ansible/role/d/buluma/redmine)](https://galaxy.ansible.com/ui/standalone/roles/buluma/redmine/documentation)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-redmine/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  vars:
    - centos_base_enable_epel: true
    - centos_base_fail2ban_configuration: false
    - centos_base_install_selinux_packages: true
    - centos_base_basic_packages: true
    - centos_base_security_packages: true
    - centos_base_firewalld: true
    - ruby_version: 2.3
    - ruby_install_globally: true
  roles:
    - bngsudheer.centos_base
    - bngsudheer.ruby

- name: Converge
  hosts: all
  vars:
    - redmine_sql_database_name: "redmine"
    - redmine_sql_database_host: "localhost"
    - redmine_sql_username: "redmine"
    - redmine_sql_password: "password"
    - redmine_unicorn_port: 5777
    - redmine_configure_selinux: false
    - redmine_plugins:
        - name: scrum
          base_name: scrum
          url: https://redmine.ociotec.com/attachments/download/481/scrum-v0.18.1.tar.gz
          create_base_directory: false
        - name: timesheet
          base_name: timesheet
          url: https://github.com/Contargo/redmine-timesheet-plugin/archive/master.zip
          create_base_directory: false
    - mysql_databases:
        - name: redmine
    - mysql_users:
        - name: redmine
          password: password
          priv: "redmine.*:ALL"
    - redmine_additional_configuration: true
    - redmine_enable_smtp_email: true
    - redmine_smtp_settings_address: localhost
    - redmine_smtp_settings_port: 25
    - redmine_smtp_settings_authentication: plain
    - redmine_smtp_settings_domain: redmine.example.com
    - redmine_smtp_settings_user_name: redmine@redmine.example.com
    - redmine_smtp_settings_password: password
    - redmine_smtp_settings_enable_starttls_auto: true
  roles:
    - geerlingguy.mysql
    - buluma.redmine
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-redmine/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  gather_facts: false
  tasks: []
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-redmine/blob/master/defaults/main.yml):

```yaml
---
# defaults file for ansible-role-redmine
redmine_version: "3.4.11"
# redmine_runtime_directory: "/home/redmine/redmine-{{ redmine_version }}"
redmine_runtime_directory: "/srv/redmine/redmine"

redmine_sql_driver: mysql2
redmine_sql_database_name: "redmine"
redmine_sql_database_host: "localhost"
redmine_sql_username: "redmine"
redmine_sql_password: "password"

redmine_unicorn_worker_processes: 2

redmine_domain_name: redmine.example.com

redmine_configure_nginx: true
redmine_nginx_config_template: plain
redmine_nginx_custom_config_path:
redmine_configure_unicorn: true
redmine_configure_firewalld: true

redmine_unicorn_port: 5000
redmine_nginx_bind_ip:
redmine_plugins: []

redmine_configure_selinux: false
redmine_bundler_version: 1.16.1

redmine_additional_configuration: false
redmine_enable_smtp_email: false
redmine_smtp_settings_address: localhost
redmine_smtp_settings_port: 25
redmine_smtp_settings_authentication: plain
redmine_smtp_settings_domain: redmine.example.com
redmine_smtp_settings_user_name:
redmine_smtp_settings_password:
redmine_smtp_settings_enable_starttls_auto: false

redmine_default_data: false
redmine_language: en
redmine_ssl_certificate_path: "/etc/letsencrypt/live/{{ redmine_domain_name }}/fullchain.pem"
redmine_ssl_certificate_key_path: "/etc/letsencrypt/live/{{ redmine_domain_name }}/privkey.pem"
redmine_nginx_allowlist: false
redmine_nginx_allowlist_path: ''
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-redmine/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | Version |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Ansible Molecule](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-bootstrap.svg)](https://github.com/shadowwalker/ansible-role-bootstrap)|
|[bngsudheer.centos_base](https://galaxy.ansible.com/buluma/bngsudheer.centos_base)|[![Ansible Molecule](https://github.com/buluma/bngsudheer.centos_base/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/bngsudheer.centos_base/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/bngsudheer.centos_base.svg)](https://github.com/shadowwalker/bngsudheer.centos_base)|
|[bngsudheer.ruby](https://galaxy.ansible.com/buluma/bngsudheer.ruby)|[![Ansible Molecule](https://github.com/buluma/bngsudheer.ruby/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/bngsudheer.ruby/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/bngsudheer.ruby.svg)](https://github.com/shadowwalker/bngsudheer.ruby)|
|[geerlingguy.mysql](https://galaxy.ansible.com/buluma/geerlingguy.mysql)|[![Ansible Molecule](https://github.com/buluma/geerlingguy.mysql/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/geerlingguy.mysql/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/geerlingguy.mysql.svg)](https://github.com/shadowwalker/geerlingguy.mysql)|
|[geerlingguy.postgresql](https://galaxy.ansible.com/buluma/geerlingguy.postgresql)|[![Ansible Molecule](https://github.com/buluma/geerlingguy.postgresql/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/geerlingguy.postgresql/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/geerlingguy.postgresql.svg)](https://github.com/shadowwalker/geerlingguy.postgresql)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-redmine/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/repository/docker/buluma/enterpriselinux/general)|7|

The minimum version of Ansible required is 2.4, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-redmine/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-redmine/blob/master/CHANGELOG.md)

## [License](#license)

[license (Apache-2.0)](https://github.com/buluma/ansible-role-redmine/blob/master/LICENSE)

## [Author Information](#author-information)

[Shadow Walker](https://buluma.github.io/)

