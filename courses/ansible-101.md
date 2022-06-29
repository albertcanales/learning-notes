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


## Ad-hoc tasks and Inventory

### Intro to ad-hoc commands

Normally we use Ansible through Playbooks, as those commands can be easily reproducible. For debugging or monitoring purposes, however, *ad-hoc* commands are very useful as they are faster to write.

### Multi-VM Vagrant configuration

With Vagrant one can start multiple VMs in a single Vagrant file. Also available in the examples.

### Multi-host inventory

There are two sintaxes that can be used, INI and YAML. In INI:
- Comments with #
- Groups in brackets (for example `[db]`)

We can also make groups of groups with `:children`. Example:
```
[group1]
host1
...

[group2]
host3
...

# Using :children for group of groups
[multigroup:children]
group1
group2
```

We can also give variables for groups with `:vars`. For example:
```
[multigroup:vars]
ansible_ssh_user=myuser
```

### Forks and parallellization

Ansible (by default) runs 5 commands in parallel. Option `-f` can change the number of forks, so 1 would make it sequential.

Ad-hoc commands always report *change*, as without a module, Ansible, is unable to figure out if something has really changed or not.

### Setup module

The `setup` module gives all the information Ansible has about a server. Useful for reference when building a playbook for that host.

### Becoming root

For installing packages, the `-b` option (become) is necessary to have root privileges. If becoming superuser requires a password, the `-K` option will ask for it and proceed.

### ansible-doc

Ansible documentation in the [website](docs.ansible,com) or terminal using `ansible-doc` (offline).

### Targeting inventory groups

A single host can be targeted (or ignore) with `--limit` argument followed by a regexp. Not necessary until debugging with quite advanced stuff.


## Introduction to Playbooks

> As the livestreams don't perfectly match with the chapters on the books, the beggining of this episode continues with ad-hoc commands, that would fit better on the last episode.

### Multi-host ad-hoc orchestration

To let an ad-hoc command run in the background, there are two interesting arguments:
- `-B <seconds>`: Lets the command run for `<seconds>` seconds. If not finished after those, the job is killed.
- `-P <seconds>`: It will poll every `<seconds>` seconds to see if the job is finished. If 0, then no polling is done.

On `-P`, ansible returns a *job_id*, that can be used to check the status of that job with the `async_status` module.

### Using different modules ad-hoc

- The command module does not accept pipes, so if necessary (not recommended due to idempotency) the `shell` module can be used.
- `cron` module for easily create or delete recurrent jobs.
- Managing repositories with `git` module.

As always, playbooks are recommended for most of these cases.

### Moving commands into YAML playbooks

General pattern for naming playbooks:

- *main.yml*: Playbook that automates the whole maintenance of the host
- The rest of the names describe the general purpose

### Making playbooks more Ansible-ish

> For this part, I think it is better to look at the docs, as the course explains the concepts very near to the given examples and its not very translatable to notes.

The `copy` module for copying files with given permissions, etc. It can do multiple files at once by using the `with_items` clause and **Jinja**. Example:

```yaml
- name: Copy some files
  copy:
    src: "{{ item.src }}"
    dest: "{{ item.dest }}"
    owner: root
    group: root
    mode: 0644
  with_items:
    - src: file1
      dest: ~/file1
    - src: file2
      dest: ~/file2
```

Then Ansible runs the task for each of the values of `with_items`, as in a loop. The syntax `{{ item['src'] }}` can also be used, an safer when the the property has special characters.

For boolean values, both of the pairs *true*/*false* or *yes*/*no* can be used.

For running playbooks, the `ansible-playbook` command is used.

### Limiting to specific servers and ansible-inventory

As in ad-hoc, the `--limit` argument can be used to limit the targeted hosts for a playbook to a certain group, host, negation, etc.

The `ansible-inventory` command gives information about the hosts managed on the inventory file.


## A first real-world playbook

### Our first real-world playbook

We can specify variables to be considered before running a play with the following syntax:

```yaml
vars_files:
- vars.yaml
```

For styling purposes, tasks on a playbook can be divided into `pre_tasks`, `tasks` and `post_tasks`

Downloading in apt can have problems due to cache. The following task fixes it:

```yaml
pre_tasks:
  - name: Update apt cache if needed
    apt:
      update_cache: true
      cache_valid_time: 3600
```

### Adding handlers

Sometimes is interesting to trigger a task only when another one applies changes. This is possible with Ansible, and the triggered task is called **handler**.

The triggering task has an attribute `notify: <name of the handler>`. Then the *handlers* section is added like so:

```yaml
handlers:
  - name: Name of my first handler
    module, etc.
```

An example of this could be a service, as changing the configuration (trigger) often requires to restart that service (handler).

### Installing Java and Solr

When downloading files, it is better to specify the whole path to guarantee idempotency.

Some more interesting modules:
- `get_url`: Downloads file from url, similar to wget command.
- `unarchive`: Equivalent to unzip, untar, etc commands.

`creates` is a common parameter to guarantee idempotency. If a certain thing exists (for example a file), there is no need to apply changes. In that case, it sometimes reports *skipped* instead of *ok*, depending on the module.

When working with long attributes, two interesting operators can be used for legibility purposes:

- `>`: All the lines afterwards will be joined with a space in between
- `|`: All the lines afterwards will be joined with newlines in between

### Checking playbook syntax

Syntax can be checked without running the playbook with the `--syntax-check` argument.


## Playbook handlers, environment vars, and variables

### Playbook handlers

By default, handlers are called at the end of the playbook. This can be changed by the `flush_handlers` meta task:

```yaml
- name: This flushes all pending handlers up to this moment.
  meta: flush_handlers
```

There is a module called `fail` that always fails.

If a tasks fails, the pending handlers won't be run. This behavior can be changed by the `--force-handlers` attribute.

As handers are a kind of task, a handler can notify another handler.

For standarisation, it is a good practice to give handlers' name no capital start letter, puntuation, etc.

### Environment variables

Ansible already has the environment variables of the remote host.

For changing an environment variables (or adding a new one), the `lineinfile` module can be used:

```yaml
- name: Adding the ENV_VAR to remote user's shell
  lineinfile:
    dest: "~/.bash_profile"
    regexp: '^ENV_VAR='
    line: 'ENV_VAR = value'
```

An environment variable can be used on the playbook with the `register` attribute.

```yaml
- name: Get value of an environment variable.
  shell: 'source ~/.bash_profile && echo $ENV_VAR'
  register: foo
```

In the example, `foo` now contains a dictionary with *stdin*, *stdout*, and *stderr*. The `debug` mode can be used to echo the value.

```yaml
- debug: "The value of the variable is {{ foo.stdout }}."
```

A variable for a task or a whole play can be declared with the `environment` attribute. 

The second option is to declare them at play level with `vars`, and later on injected into the necessary tasks with `environment`.

### Dynamic variable files for multi-OS

Variables can also be imported on a playbook from a *variables file* with `vars_file`. The *variables file* also follows a YAML structure.

This allows, for example, to have installation playbooks compatible with different package managers, as a different name for the service can be given for each one. To achive this, one could do:

```yaml
pre_tasks:
  - name: Lead variable files.
    include_vars: {{ item }}
    with_first_found:
      - "vars/{{ ansible_os_family }}.yml"
      - "vars/default.yml"
```

- The `with_first_found` item runs a loop looking for file, and stores in `item` the first file that can be read.
- The `ansible_os_family` is part of the *facts* Ansible can gather about a host. The rest can be seen with the module `setup`.

### Ansible facts and setup modules

The default *fact* gathering on a play can be disabled with the attribute `gather_facts`

A *facts script* can be used to only register certain facts.


## Ansible Vault and Roles

### Encrypting a vars file with Vault

A YAML file (normally a vars one) can be encrypted with `ansible-vault encrypt <path-to-file>`.

For running playbooks that use that file, the `--ask-vault-pass` argument can be passed to the `ansible-playbook` command. This approach cannot be easily automated, so it is sometimes convenient to store the password in a file and use the attribute `--vault-password-file`.

### Decrypt, encrypt, edit, rekey, etc.

A file can also be *decrypted* or *edited*. The password can be changed with *rekey*

For best practices, it is better to keep the encypted and non-encrypted variables separated.

### Task features - conditionals and tags

The `when` attribute can be used (together with Jinja) to run a task on a certain condition. There also exists `changed_when` and `failed_when`.

There also exists `ignore_errors` if it is desired to not fail on an error from the task.

Some tasks can also have `tags` assigned, and then running that playbook with `tags=<tag>` will only run those tasks. Important not to use them too much.

### Blocks

Similar to a try and except structure. A group of tasks is run and, on fail, another group of tasks is run. As in tags, not necessary in most cases

### Playbook organization

Playbooks should be kept, in general, quite small. Ansible offers tool to guarantee this.

### Includes and imports

It is common to separate handlers on their own directory, and import them with (analogous for normal tasks):

```yaml
handlers:
  -import_tasks: handlers/<handler_file>.yml
```

Variables can be passed to these imported tasks with `vars`.

### Caution with dynamic tasks

If the task has dynamic content `include_tasks` must be used.

### Playbook includes

Playbooks can be included with `import_playbook`.

### Roles

A *role* is a set of related tasks that can be called from one or multiple playbooks with extra tools that make it easier than importing a playbook.

They are stored in the `roles/<role_name>` folder. They are made of two directories:

- *meta*: There must be *main.yml*. Some more advanced stuff like dependencies.
- *tasks*: Where the tasks are stored. There must be *main.yml*.

Roles are by default stored on some path in the home directory

### Options for including roles

Roles can be specified in the `roles` section of the playbook, and they will be run at the start. If it is desired to call them at a certain moment, it is possible with `include_role`.


### Real-world flexible role usage

Due to its flexibility, there are roles available for installing and deploying specific software on **Ansible Galaxy**.

To create an empty role, `ansible-galaxy role init <role_name>`.

Due to the great amout of roles available, most times setting a service consists on choosing roles and writing few tasks to add some less fundamental functionality.

