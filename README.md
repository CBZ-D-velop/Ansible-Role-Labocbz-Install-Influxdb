# Ansible role: labocbz.install_influxdb

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: InfluxDB](https://img.shields.io/badge/Tech-InfluxDB-orange)

An Ansible role to install and configure an InfluxDb server on your host.

The Ansible role installs InfluxDB, a time series database, on the target system. The role installs a version that includes the InfluxData web interface, providing a user-friendly interface for managing and querying the database.

Administrators have the flexibility to configure the SSL/TLS settings by enabling or disabling it. When SSL/TLS is enabled, the role allows customization of the SSL certificate and key paths, providing the capability to use custom SSL certificates for secure communications.

Furthermore, administrators can define the bind address and port for the InfluxDB HTTP service, allowing them to set the address and port where InfluxDB will listen for incoming HTTP requests.

It is important to note that while the role offers a robust setup for InfluxDB with SSL/TLS and customizable bind address and port, the role does not support a cluster mode. InfluxDB's native clustering functionality is not included in the role, as it is not developed by the creators of the database.

By deploying InfluxDB with this role, administrators can quickly set up a time series database with an integrated web interface for easy management. The ability to configure SSL/TLS and specify the HTTP bind address and port provides a secure and customizable environment for storing and analyzing time series data.

In summary, the InfluxDB role simplifies the installation and configuration of the database, offering a powerful solution with SSL/TLS support and customizable HTTP bind settings. Administrators can leverage this role to deploy a feature-rich time series database for their applications, benefiting from the performance and scalability it provides.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
install_influxdb__group: "influxdb"
install_influxdb__user: "influxdb"

install_influxdb__http_bind_address: "0.0.0.0"
install_influxdb__http_bind_port: 8086

install_influxdb__ssl: true
install_influxdb__ssl_domain: "my.influxdb2-server-2.domain.tld"
install_influxdb__ssl_path: "/etc/influxdb/ssl"
install_influxdb__ssl_key: "{{ install_influxdb__ssl_path }}/{{ install_influxdb__ssl_domain }}/{{ install_influxdb__ssl_domain }}.key"
install_influxdb__ssl_cert: "{{ install_influxdb__ssl_path }}/{{ install_influxdb__ssl_domain }}/{{ install_influxdb__ssl_domain }}.crt"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_install_influxdb__http_bind_address: "0.0.0.0"
inv_install_influxdb__http_bind_port: 8086

inv_install_influxdb__ssl: true
inv_install_influxdb__ssl_domain: "my-influxdb-server.domain.tld"
inv_install_influxdb__ssl_path: "/etc/influxdb/ssl"
inv_install_influxdb__ssl_key: "{{ inv_install_influxdb__ssl_path }}/{{ inv_install_influxdb__ssl_domain }}/{{ inv_install_influxdb__ssl_domain }}.pem.key"
inv_install_influxdb__ssl_cert: "{{ inv_install_influxdb__ssl_path }}/{{ inv_install_influxdb__ssl_domain }}/{{ inv_install_influxdb__ssl_domain }}.pem.crt"

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_influxdb"
  tags:
    - "labocbz.install_influxdb"
  vars:
    install_influxdb__http_bind_address: "{{ inv_install_influxdb__http_bind_address }}"
    install_influxdb__http_bind_port: "{{ inv_install_influxdb__http_bind_port }}"
    install_influxdb__ssl: "{{ inv_install_influxdb__ssl }}"
    install_influxdb__ssl_domain: "{{ inv_install_influxdb__ssl_domain }}"
    install_influxdb__ssl_key: "{{ inv_install_influxdb__ssl_key }}"
    install_influxdb__ssl_cert: "{{ inv_install_influxdb__ssl_cert }}"
  ansible.builtin.include_role:
    name: "labocbz.install_influxdb"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-05-13: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-05-30: Cryptographic update

* SSL/TLS Materials are not handled by the role
* Certs/CA have to be installed previously/after this role use

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

### 2023-12-11: Cert and Ca location

* Role now copy / import cert in /etc/influxdb2-cert.pem.crt
* Role now copy / import key in /etc/influxdb2-key.pem.key
* Because in some system, create a user and after install with package can be broken (SELinux)

### 2023-12-14: System users

* Role can now use system users and address groups
* Role handle the log path
* Role create a service file, in order do not log inside sysout

### 2024-02-22: New CICD and fixes

* Added support for Ubuntu 22
* Added support for Debian 11/22
* Edited vars for linting (role name and __)
* Added generic support for Docker dind (can add used for obscures reasons ... user in use)
* Fix idempotency
* Added informations for UID and GID for user/groups
* Added support for user password creation (on_create)
* New CI, need work on tag and releases
* CI use now Sonarqube

### 2024-05-19: New CI

* Added Markdown lint to the CICD
* Rework all Docker images
* Change CICD vars convention
* New workers
* Removed all automation based on branch

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
