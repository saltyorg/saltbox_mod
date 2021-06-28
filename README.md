# saltbox_mod
Blank Template to add custom Ansible roles to saltbox.

## How to use this template

1. Clone this repo:

    ```bash
    git clone https://github.com/saltyorg/saltbox_mod.git ~/saltbox_mod
    ```

1. CD into the `saltbox_mod` folder:

    ```bash
    cd ~/saltbox_mod
    ```

1. If you have an Ansible vault password file (not recommended), add the location to `ansible.cfg`:

    To edit:

    ```bash
    nano ~/saltbox_mod/ansible.cfg
    ```

    Add line (with path to your vault password file):
    ```ini
    vault_password_file = ~/.ansible_vault
    ```

    Final result:
    ```ini
    [defaults]
    inventory = /srv/git/saltbox/inventories/local
    library = /srv/git/saltbox/library
    roles_path = roles:/srv/git/saltbox/roles:/srv/git/saltbox/resources/roles
    filter_plugins = /srv/git/saltbox/filter_plugins
    fact_path = /srv/git/saltbox/ansible_facts.d
    log_path = ./community.log
    callback_whitelist = profile_tasks
    command_warnings = False
    retry_files_enabled = False
    interpreter_python = /usr/bin/python3
    vault_password_file = ~/.ansible_vault
    ```

1. Create folders for the Ansible role:

    ```bash
    mkdir -p ~/saltbox_mod/roles/newrole/tasks/
    ```

1. Place the task file there:

    ```bash
    touch ~/saltbox_mod/roles/newrole/tasks/main.yml
    ```

1. Add custom variables into `settings.yml`:

    ```
    ~/saltbox_mod/settings.yml
    ```


1. Add the Ansible role to `saltbox_mod.yml`:

    To edit:

    ```bash
    nano ~/saltbox_mod/saltbox_mod.yml
    ```

    Add the following line under `roles:`:
    ```yaml
        - { role: newrole, tags: ['newrole'] }
    ```

    Final result:
    ```yaml
    ---
    - hosts: localhost
      vars_files:
        - settings.yml
        - ['~/cloudbox/accounts.yml', '~/cloudbox/accounts.yml.default']
        - ['~/cloudbox/settings.yml', '~/cloudbox/settings.yml.default']
        - ['~/cloudbox/adv_settings.yml', '~/cloudbox/adv_settings.yml.default']
      roles:
        - { role: pre_tasks }
        - { role: myrole, tags: ['myrole'] }
        - { role: newrole, tags: ['newrole'] }
    ```

    Note: The `pre_tasks` role is required and should not be removed.

1. Run the Ansible role:

    ```bash
    sudo ansible-playbook saltbox_mod.yml --tags newrole
    ```
