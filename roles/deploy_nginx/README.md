```markdown
# Role Name: deploy_nginx

## Description
This role installs Nginx and deploys a simple HTML file to the default web root directory.

## Requirements
Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here.

## Role Variables
A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (i.e., hostvars, group vars, etc.) should be mentioned here as well.

## Dependencies
A list of other roles hosted on Galaxy should go here, plus any details regarding parameters that may need to be set for other roles or variables that are used from other roles.

## Example Playbook
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- hosts: servers
  roles:
     - { role: username.rolename, x: 42 }
```

And here's the content of other files formatted for clarity:

### `main.yml`
```yaml
---
# This file contains the default values for the variables used in the deploy_nginx role
packages:
  - nginx
  - htop
  - neovim
  - neofetch
  - net-tools
dest: /var/www/html
```

### `handlers/main.yml`
```yaml
---
# Handlers used to restart the Nginx service
- name: Restart Nginx Service
  service:
    name: nginx
    state: restarted
```

### `meta/main.yml`
```yaml
galaxy_info:
  author: Valerii Vasianovych
  description: Example role to deploy Nginx and deploy index.j2 to /var/www/html
  license: MIT
  min_ansible_version: '2.4'
dependencies: []
```

### `tasks/01_update_packages.yml`
```yaml
---
# Update and upgrade the package list
- name: Update package list
  apt:
    update_cache: yes

- name: Upgrade all installed packages
  apt:
    upgrade: dist
```

### `tasks/02_nginx_install.yml`
```yaml
---
# Install Nginx package
- name: Install Nginx package
  apt:
    name: nginx
    state: present

- name: Install additional packages
  apt:
    name: '{{ packages }}'
    state: present
```

### `tasks/03_copy_indexj2.yml`
```yaml
---
# This task copies the template file to the remote host

- name: Deploy index.j2
  template:
    src: 'index.j2'
    dest: '{{ dest }}/index.html mode=0555'
  notify:
    - Restart Nginx Service
```

### `templates/index.j2`
```html
<!DOCTYPE html>
<html>
<head>
    <title>Welcome to My Website</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f0f0f0;
        }
        header {
            background-color: #333;
            color: #fff;
            padding: 20px;
            text-align: center;
        }
        h1 {
            color: #333;
            text-align: center;
        }
        .container {
            max-width: 800px;
            margin: 20px auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        p {
            margin-bottom: 10px;
        }
    </style>
</head>
<body>
    <header>
        <h1>Welcome to My Website</h1>
    </header>
    <div class="container">
        <p>This website is owned by {{ owner }}.</p>
        <p>Server IP address: {{ ansible_host }}</p>
        <p>Position: {{ position }}</p>
    </div>
</body>
</html>
```

### `tests/inventory`
```
localhost
```

### `tests/test.yml`
```yaml
---
- hosts: localhost
  remote_user: root
  roles:
    - deploy_nginx
```

### `vars/main.yml`
```yaml
---
# Vars file for deploy_nginx
```

### `install_nginx.yaml`
```yaml
---
- name: Install Nginx and Upload file
  hosts: all
  become: true

  roles:
    - { role: deploy_nginx, when: ansible_system == 'Linux' }
```
