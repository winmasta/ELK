[![Build Status](https://travis-ci.org/winmasta/ELK.svg?branch=master)](https://travis-ci.org/winmasta/ELK)

Role Name
=========

Ansible role for latest ELK stack in docker containers. UI behind nginx with basic authorization. ELK docker config
will be taken from `https://github.com/deviantony/docker-elk.git` repo. Tested on:
  - Ubuntu 14.04 Trusty
  - Ubuntu 16.04 Xenial

Requirements
------------

Installed OS:
 - Ubuntu 14.04 Trusty
 - Ubuntu 16.04 Xenial

Role Variables
--------------

  - USE_TLS: True # Enables or disables using TLS for logstash
  - LOGSTASH_PORT: 5000 # Logstash external port for beats (filebeat etc).
  - DEST_DIR: /opt/docker-elk # ELK docker files directory.

Dependencies
------------

Depends on:
 - winmasta.docker-latest
 - winmasta.nginx

Example Playbook
----------------

To use this role:

  - create folder (in user $HOME folder in example below) and install role with dependencies from ansible-galaxy

```bash
cd ~/
mkdir ELK
cd ELK
ansible-galaxy install winmasta.ELK --roles-path .
```

  - as soon as ansible-galaxy doesn't install role dependencies yet, you should do it manually

```bash
ansible-galaxy install -r winmasta.ELK/requirements.yml --roles-path .
```

  - create file `hosts`, containing hostname(s) or IP address(es) of host(s), where you want to deploy role

```bash
echo "ENTER HOSTNAME OR IP" > hosts
```

  - create file `ansible.cfg` in current folder, `remote_user` can be changed

```bash
cat > ansible.cfg << EOF
[defaults]
remote_user = root
host_key_checking = False
EOF
```

  - create playbook in current folder `main.yml` with content, `become: yes` may be required if `remote_user` is not
    root

```bash
cat > main.yml << EOF
---
- hosts: all
  gather_facts: no

  pre_tasks:

  - name: Install required packages
    raw: sudo apt-get update -y && sudo apt-get -y install python-simplejson python-pip
    changed_when: False

  - setup:

  roles:
    - winmasta.docker-latest
    - winmasta.nginx
    - winmasta.CA
    - winmasta.ELK
EOF
```

  - execute playbook `main.yml`

```bash
ansible-playbook -i hosts main.yml
```

License
-------

MIT
