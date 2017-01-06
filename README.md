# ANSIBLE ROLE: GRAFANA

An Ansible role to install Grafana metrics dashboards on various platforms.

_**Note:** At the moment, Docker is the only platform supported by this role._

## REQUIREMENTS

* Docker >= 1.12.4

## INSTALLATION

#### USING _ANSIBLE GALAXY_:

```
ansible-galaxy install erjac77.grafana
```

#### USING _GIT CLONE_:

Clone this repository inside the `roles/` subdirectory of your playbook or inside one of the additional directories specified by the `roles_path` setting in `ansible.cfg`.

```
git clone https://github.com/erjac77/ansible-role-grafana.git erjac77.grafana
```

## ROLE VARIABLES

```
---

# Grafana host platform
grafana_platform: Docker
#grafana_platform: "{{ ansible_os_family }}"

# Grafana connection timeout settings
grafana_connection_retries: 60
grafana_connection_delay: 5

########## Grafana configuration ##########
grafana_config:
  instance_name: "{{ grafana_config_server.domain }}"

# Server
grafana_config_server:
  http_port: 3000
  domain: "{{ hostvars[inventory_hostname]['ansible_default_ipv4']['address'] }}"

# Security
grafana_config_security:
  admin_user: grafana
  admin_password: grafana
#########/ Grafana configuration ##########

# Grafana users
grafana_users:
  - { username: "{{ grafana_config_security.admin_user }}", fullname: Grafana Administrator, email: 'grafana.admin@grafana.io' }

# Grafana datasources
grafana_datasources: []
```

### DOCKER VARIABLES

```
---

# Grafana Docker container settings
grafana_container_name: grafana
grafana_container_image: grafana/grafana
grafana_container_port: 3000
grafana_container_data_volume: grafana-data
```

## DEPENDENCIES

* [ansible-role-docker](https://github.com/erjac77/ansible-role-docker)

## EXAMPLE PLAYBOOK

```
---

- name: Install Grafana on Docker with Prometheus Datasource
  hosts: localhost
  become: yes

  vars:
    grafana_platform: Docker
    grafana_users:
      - { username: "{{ grafana_config_security.admin_user }}", fullname: Grafana Administrator, email: 'grafana.admin@grafana.io' }
      - { username: jodoe, password: jodoe, fullname: John Doe, email: 'john.doe@grafana.io', admin: false }
      - { username: jadoe, password: jadoe }
    grafana_datasources:
      - { name: prometheus, type: prometheus, url: 'http://prometheus.localhost:9090', access: direct, basic_auth: false, is_default: true }

  roles:
    - erjac77.grafana
```

## LICENSE

Apache 2.0

## AUTHOR INFORMATION

Eric Jacob ([@erjac77](https://github.com/erjac77))