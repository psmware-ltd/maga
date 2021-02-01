# How to use Ansible from this repo

## Docker
There is a docker container which includes all requirements and dependencies:
[storage-ansible-docker](https://github.wdf.maga.corp/Storage-Automation/storage-readymade)

## Run a single playbook

Go to:

`<repo/ansible>`

Run:

`ansible-playbook -i inventory/<file>  playbook/<vendor|project|..>/playbook.yml <playbook>`

## Useful options
### `--check`

This will run in check mode.  Check mode will not make any changes but will inform
you what changes will be made when the check-mode flag is not included in the command

*Note: Not all ansible provided modules support check mode.

### `-vvv`
Optional level of verbosity corresponding to the number of v's
