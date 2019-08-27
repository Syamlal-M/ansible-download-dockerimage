# Install the Docker and download the Docker images to the Linux server

Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to
package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package. 
Here I am installing the docker and downloading the docker images to the Linux server using the Playbook.

## Tasks

- Install the Docker
- Download the Docker images

### Install Docker
Here I installing the python, pip,docker-python extension and docker at the remote linux server using playbook.

```bash
---
 - name: "installing docker and pull the docker images"
   hosts: docker
   vars:
    group1: docker
    user1: ec2-user
   tasks:
     - name: "inatlling python3 and pip package manager"
       become: true
       yum: name=python3,python-pip state=present

     - name: "Install docker python extension"
       become: true
       pip: name=docker-py

     - name: "install the docker"
       become: true
       yum: name=docker state=present

     - name: "restarting docker service"
       become: true
       service: name=docker state=restarted enabled=yes

     - name: "Add or check the group docker "
       become: true
       group:
         name: "{{group1}}"
         state: present
     - name: " add the  user ec2-user to the group docker "
       become: true
       user:
          name: "{{user1}}"
          shell: /bin/bash
          groups: "{{group1}}"
          append: yes
```

### Download the Docker images

```bash
  - name: "pull the docker images from docker HUB"
       docker_image:
           name:  "{{item}}"
       with_items:
             - fedora:28
             - httpd:2
             - ubuntu:18.04
```

### Output

```bash

TASK [Gathering Facts] ********************************************************************************************************************************************************************************************
ok: [172.31.42.219]

TASK [inatlling python3 and pip package manager] ******************************************************************************************************************************************************************
ok: [172.31.42.219]

TASK [Install docker python extension] ****************************************************************************************************************************************************************************
ok: [172.31.42.219]

TASK [install the docker] *****************************************************************************************************************************************************************************************
ok: [172.31.42.219]

TASK [restarting docker service] **********************************************************************************************************************************************************************************
changed: [172.31.42.219]

TASK [Add or check the group docker] ******************************************************************************************************************************************************************************
ok: [172.31.42.219]

TASK [add the  user ec2-user to the group docker] *****************************************************************************************************************************************************************
ok: [172.31.42.219]

TASK [pull the docker images from docker HUB] *********************************************************************************************************************************************************************
changed: [172.31.42.219] => (item=fedora:28)
changed: [172.31.42.219] => (item=httpd:2)
changed: [172.31.42.219] => (item=ubuntu:18.04)

PLAY RECAP ********************************************************************************************************************************************************************************************************
172.31.42.219              : ok=8    changed=2    unreachable=0    failed=0

```
