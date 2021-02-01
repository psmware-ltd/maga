# MAGA
This repository contains exmaples of how to work with Molecule, and Ansible with GitHub Actions

## Contribution guide

We have a detailed guide which describes how we work in this repo and how everyone can participate - [CLICK](documentation/contributing.md)

## Tips on how to work with GIT

We have a detailed collection of [git tips](documentation/git_tips.md).
It contains examples on how to fork from our repo, how to keep your fork in sync and some GIT aliases.

## Operation
There is a detailed [operations guide](documentation/OPERATION.md) which can be helpful for the first use of this repo.

## Folder Structure

```
├── ansible
│   ├── inventory <-- inventory and dynamic inventories
│   ├── module <-- our own developed modules
│   ├── module_utils <-- module utilities
│   ├── playbook <-- contains our playbooks
│   ├── plugins <-- tools to enhance Ansible's core
│   │   └── filter <-- filter plugins
│   ├── roles <-- contains our Ansible roles
│   ├── templates <-- Jinja2 templates
│   ├── vars <-- global variables
│   └── vault <-- passwords encrypted in Ansible Vault files
└── documentation
    ├── ansible
```

## README's for every section

* [INVENTORY](documentation/ansible/INVENTORY.md)
* [MODULE](documentation/ansible/MODULE.md)
* [MODULE_UTILS](documentation/ansible/MODULE_UTILS.md)
* [PLAYBOOK](documentation/ansible/PLAYBOOK.md)
* [PLUGINS](https://docs.ansible.com/ansible/latest/dev_guide/developing_plugins.html?highlight=module_utils)
* **ROLES** - we have seperate README for every role
* [TEMPLATES](http://jinja.pocoo.org/docs/dev/)
* [VARS](documentation/ansible/VARS.md)
* [VAULT](documentation/ansible/VAULT.md)

## Coding Standards
Please write your code according to the [PEP8](https://www.python.org/dev/peps/pep-0008/) conventions.
Use [Pylint](https://www.pylint.org/) to check your code.
