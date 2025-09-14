# nix-automation

Infrastructure automation repository

## Repository Structure

```
nix-automation/
├── ansible/
│   ├── inventory/
│   │   └── dev/
│   │       ├── group_vars/
│   │       │   └── jenkins/
│   │       │       └── main.yml
│   │       └── hosts.yml
│   ├── playbooks/
│   │   └── configure_jenkins.yml
│   ├── roles/
│   │   └── jenkins/
│   │       ├── defaults/
│   │       │   └── main.yml
│   │       ├── handlers/
│   │       │   └── main.yml
│   │       ├── tasks/
│   │       │   └── main.yml
│   │       └── templates/
│   │           ├── docker/
│   │           │   └── docker-compose.yml.j2
│   │           └── nginx/
│   │               └── nginx.conf.j2
│   └── ansible.cfg
├── .gitignore
└── README.md
```

## Prerequisites

- Ansible 2.18.8+
- Docker and Docker Compose
- Python 3.13.6+
- SSH access to target servers

## Jenkins Role

The Jenkins role automates deployment of Jenkins server and Nginx reverse proxy with SSL support using Docker containers

### Default Variables (ansible/roles/jenkins/defaults/main.yml)

```yaml
---

jenkins_data_dir: /opt/jenkins

jenkins_docker_registry: nix-docker.registry.twcstorage.ru

jenkins_image_repo: ci/jenkins/jenkins

jenkins_image_tag: 2.516.2-lts-jdk21

jenkins_certbot_admin_email: ""

jenkins_certbot_force_renewal: false

jenkins_nginx_image_repo: ci/nginx/nginx

jenkins_nginx_image_tag: 1.29.1-alpine

jenkins_nginx_domain: ""

jenkins_nginx_ssl_protocols:
  - TLSv1.2
  - TLSv1.3

jenkins_nginx_ssl_ciphers: |
  "ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256"
```

### Example Inventory (ansible/inventory/dev/hosts.yml)

```yaml
---

all:
  hosts:
    jenkins01:
      ansible_user: your_user
      ansible_host: your.jenkins.domain.com
      ansible_python_interpreter: /usr/bin/python3.12
  children:
    jenkins:
      hosts:
        jenkins01:
```

## Quick Start

1. Clone the repository:

```bash
git clone <repository-url>
cd nix-automation
```

2. Navigate to the Ansible directory:

```bash
cd ansible
```

3. Configure inventory in `inventory/dev/hosts.yml`

4. Set variables in `inventory/dev/group_vars/jenkins/main.yml`:

```yaml
---

jenkins_certbot_admin_email: "your-email@example.com"

jenkins_nginx_domain: "your.jenkins.domain.com"
```

5. Run the playbook:

```bash
ansible-playbook -i inventory/dev/hosts.yml playbooks/configure_jenkins.yml
```

## Usage

Deploy with specific tags:

```bash
ansible-playbook -i inventory/dev/hosts.yml playbooks/configure_jenkins.yml --tags "jenkins"
ansible-playbook -i inventory/dev/hosts.yml playbooks/configure_jenkins.yml --tags "nginx"
```

Available tags:
- `jenkins` - Jenkins container deployment
- `nginx` - Nginx reverse proxy setup

## Maintenance

Update Jenkins by changing `jenkins_image_tag` and rerunning the playbook \
Update Nginx by changing `jenkins_nginx_image_tag` and rerunning the playbook

## Support

For issues, check:
- Inventory and variable configurations
- Network connectivity to target servers
- Prerequisites are met

## License

Apache 2.0
