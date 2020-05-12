# Getty/IO challenge

## Ansible playbook that provisions a simple HTTP server

<img alt="Github" src="https://miro.medium.com/max/690/1*AoK4pqVAILB1twMF3E98FQ.png" width="400" height="140">

## About the playbook

### First PLAY (Create container)
This play is responsable for providing a container to setup http server. The container is built based on [treasureboat/ssh](https://hub.docker.com/r/treasureboat/ssh/) and listens on port 22 for ssh connection. Last task ensure that the container is started

### Second PLAY (Provision HTTP server)
Ensure that the system has all requirements for provisioning http server. First, a single pre task installs python using the raw module. After that apache is installed and started. Then index.html is copied to the remote machine and apache service is restarted to apply changes

## Steps
Give your sudo/dzdo password
```yaml
ansible-playbook provision.yml --ask-become-pass
```

Go to your browser and type:
```text
http://172.17.0.2:80
```

## Updating
Feel free to update index.html but don't forget to run the bellow command after changes:
```yaml
ansible-playbook provision.yml --ask-pass --tags update
```

## Obs
* raw module is useful and should only be done in a few cases. A common case is installing python on a system without python installed by default. [From ansible docs](https://docs.ansible.com/ansible/latest/modules/raw_module.html)

*  In this case facts are not used, so gather_facts: no

* Running with --tags docker executes only *Create container play*

* Running with --tags provision executes only *Provision HTTP server play*