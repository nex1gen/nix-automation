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
├── kubernetes/
│   ├── buildkit.yaml
│   ├── clusterissuer.yaml
│   └── loadbalancer.yaml
├── .gitignore
└── README.md
```

## Jenkins Role

Detailed setup notes for the Jenkins Ansible role - [ansible/roles/jenkins/README.md](ansible/roles/jenkins/README.md).
