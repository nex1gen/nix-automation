# nix-automation

Infrastructure automation repository

## Repository Structure

```
nix-automation/
├── ansible/ # Root Ansible automation directory
│   ├── inventory/
│   │   └── dev/
│   │       ├── group_vars/
│   │       │   └── jenkins/
│   │       │       └── main.yml
│   │       └── hosts.yml
│   ├── playbooks/
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
│   │           │   ├── docker-compose.yml.j2
│   │           │   └── Dockerfile.j2
│   │           └── nginx/
│   │               └── nginx.conf.j2
│   ├── configure_jenkins.yml # Jenkins playbook
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

jenkins_image_tag: 2.516.2-lts-jdk21

jenkins_certbot_admin_email: ""

jenkins_nginx_image_tag: 1.29.1-alpine-perl

jenkins_nginx_domain: ""
```

### Example Inventory (ansible/inventory/dev/hosts.yml)

```yaml
---

all:
  children:
    jenkins:
      hosts:
        jenkins01:
          ansible_user: user
          ansible_host: your.jenkins.domain.com
          ansible_python_interpreter: /usr/bin/python3.12
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
ansible-playbook -i inventory/dev/hosts.yml configure_jenkins.yml
```

## Usage

Deploy with specific tags:

```bash
ansible-playbook -i inventory/dev/hosts.yml configure_jenkins.yml --tags "jenkins"
ansible-playbook -i inventory/dev/hosts.yml configure_jenkins.yml --tags "nginx"
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
