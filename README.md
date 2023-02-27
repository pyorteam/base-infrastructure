# Setting up a remote machine using Ansible

#### This repository contains the necessary files and configurations to set up the Ansible DevOps Automation Tool on a remote machine running Ubuntu. It enables you to automate various tasks related to DevOps on the remote machine.

#### The repository is designed to install the following tools on a remote machine:
- Docker
- Nginx
- Node
- npm
- yarn
- PHP

## 0. Minimum Requirements
To get started with this repository, you need to have the following:

Mimium requirements:
* Ansible installed on your local machine
* sshpass installed on your local machine

### 0.1 Installing sshpass on your local machine
To install `sshpass` on your local machine, you can use the package manager specific to your operating system.

For example, if you are using Ubuntu, you can run the following command:
```
sudo apt-get install sshpass
```

If you are using CentOS, you can run the following command:
```
sudo yum install sshpass
```

If you are using MacOS, you can run the following command:
```
brew tap perkons/sshpass
brew install sshpass
```

## 1. Create ssh key
To create an SSH key, you can use the `ssh-keygen` command in your terminal.

For example, to create a new SSH key with the default settings, you can run the following command:
```
ssh-keygen
```

This will prompt you to enter a file name for the key and a passphrase. You can press Enter to accept the default file name and not set a passphrase.


## 2. Copy your SSH key to the remote server
After the key is generated, you can add it to the remote machine by using the `ssh-copy-id` command. For example:
```
ssh-copy-id root@<remote_host>
```

Alternatively, you can run the following command:
```
cat ~/.ssh/id_rsa.pub | ssh root@<remote_host> "cat >> ~/.ssh/authorized_keys"
```

## 3. Set up the configuration
To set up the configuration, follow the steps below:

### 3.1 Configure the hosts file
To configure the `hosts.example` file in your Ansible setup, rename it to `hosts` and specify the IP address of the remote server or copy the file:
```
cp hosts.example hosts
```
Remember to set up the remote server IP in the group name section.

The `<groupname>` is the same as the file in `group_vars/<groupname>.yml`.

### 3.2 Configure `group_vars/groupname.yml`
```
cp group_vars/groupname.yml.example group_vars/<groupname>.yml
```

Remember to change the name of the file to `groupname.yml`, which is the same as the group name in the hosts file.

### 3.3 Nginx main configuration
```
cp roles/nginx/templates/nginx_main_configuration.conf.j2.example roles/nginx/templates/nginx_main_configuration.conf.j2
```

### 3.4 Configure agent rules
```
cp roles/nginx/templates/agents.rules.j2.example roles/nginx/templates/agents.rules.j2
```

### 3.5 Configure a new domain conf (nginx)
```
cp roles/nginx/templates/conf/domain.conf.j2.example roles/nginx/templates/conf/<domain>.conf.j2
```

### 3.6 Configure task variables

#### 3.6.1 Configure task variables for base setup
```
cp roles/setup/vars/main/task_vars/task_variable_list.yml.example roles/setup/vars/main/task_vars/task_variable_list.yml
```

#### 3.6.2 Configure task variables for nginx setup
```
cp roles/nginx/vars/main/task_vars/task_variable_list.yml.example roles/nginx/vars/main/task_vars/task_variable_list.yml
```

#### 3.6.3 Configure task variables for node setup
```
cp roles/node/vars/main/task_vars/task_variable_list.yml.example roles/node/vars/main/task_vars/task_variable_list.yml
```

#### 3.6.4 Configure task variables for docker setup
```
cp roles/docker/vars/main/task_vars/task_variable_list.yml.example roles/docker/vars/main/task_vars/task_variable_list.yml
```

#### 3.6.5 Configure task variables for php setup
```
cp roles/php/vars/main/task_vars/task_variable_list.yml.example roles/php/vars/main/task_vars/task_variable_list.yml
```

## 4. Commands
Replace <hosts> with the group name in the hosts file.

### 4.1 Setting up or updating the remote machine
To set up or update the remote machine, run the following command:
```
ansible-playbook setup-playbook.yml -e "target_hosts=<hosts>"
```

### 4.2 Setting up or updating Nginx on the remote machine
To set up or update Nginx on the remote machine, run the following command:
```
ansible-playbook setup-nginx-playbook.yml -e "target_hosts=<hosts>"
```

### 4.3 Setting up or updating Node on the remote machine
To set up or update Node on the remote machine, run the following command:
```
ansible-playbook setup-node-playbook.yml -e "target_hosts=<hosts>"
```

### 4.4 Setting up or updating Docker on the remote machine
To set up or update Docker on the remote machine, run the following command:
```
ansible-playbook setup-docker-playbook.yml -e "target_hosts=<hosts>"
```

### 4.5 Setting up or updating PHP on the remote machine
To set up or update PHP on the remote machine, run the following command:
```
ansible-playbook setup-php-playbook.yml -e "target_hosts=<hosts>"
```
