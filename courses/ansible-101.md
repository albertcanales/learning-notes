[Ansible 101 - Jeff Geerling](https://youtube.com/playlist?list=PL2_OBreMn7FqZkvMYt6ATmgC0KAGGJNAN)

# Ansible 101

## Introduction to Ansible

Some important links:
- [Examples from the book](https://github.com/geerlingguy/ansible-for-devops)
- [Ansible Documentation](https://docs.ansible.com)

### Connecting to a Server with Ansible

Use **inventory file**, which specifies the hosts (and optionally users) that will be managed and under which group they are. Check connection with the `ping` module.

### Using an ansible.cfg file

This file gives Ansible a general configuration (for example inventory file). Simplifies the rest of commands as values for some parameters are already defined.

### Running ad-hoc commands

If no module is given on the Ansible command (that is, `-m` is not present), the `command` module is run by default, which executes a certain command.

The option `-a` gives arguments to the module. In the module `command`, the arguments are the command that is going to be run.

### Vagrant Intro

Recommended procedure for deploying a service:

1. Create an empty local Docker Container or VM
2. Automate at the local level
3. Delete everything and use the automation to rebuild it
4. Check everything still works

Vagrant allows to configure a *box* (i.e. VM) with `vagrant init <box_path>`. Boxes can be found [here](https://app.vagrantup.com/boxes/search). The corresponding box is downloaded and VM launched with `vagrant up`

Some other commands: ssh, halt, destroy...

### First Ansible Playbooks

Ansible playbooks can be configured on Vagrant as a *provision*, with the code (for the Vagrantfile and the example playbook) available in the examples (ch2). Then `vagrant provision` can be used to run the playbooks

### Idempotence

In Ansible playbooks, we define the state that we want our machines to be. Due to Ansible **idempotency**, it will do nothing if already on that state (*ok*) or it will apply changes so it comes to that state (*changed*).

This behavior can be used to *check* if the state of a machine has been modified, by for example running playbooks periodically and triggering an event on a not *ok*.

For this reason it is always recomended to use built-in modules, as they guarantee idempotency. Alternatively, scripts should be written idempotent.
