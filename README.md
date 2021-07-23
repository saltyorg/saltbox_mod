# saltbox_mod
Blank Template to add custom Ansible roles to Saltbox.

## How to use this template

1. Clone this repo:

    ```bash
    git clone https://github.com/saltyorg/saltbox_mod.git /opt/saltbox_mod
    ```

1. CD into the `saltbox_mod` folder:

    ```bash
    cd /opt/saltbox_mod
    ```

1. Create folders for the Ansible role:

    ```bash
    mkdir -p /opt/saltbox_mod/roles/newrole/tasks/
    ```

1. Place the task file there:

    ```bash
    touch /opt/saltbox_mod/roles/newrole/tasks/main.yml
    ```

1. Add custom variables into `settings.yml`:

    ```
    /opt/saltbox_mod/settings.yml
    ```


1. Add the Ansible role to `saltbox_mod.yml`:

    To edit:

    ```bash
    nano /opt/saltbox_mod/saltbox_mod.yml
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
        - ['/srv/git/saltbox/accounts.yml', '/srv/git/saltbox/defaults/accounts.yml.default']
        - ['/srv/git/saltbox/settings.yml', '/srv/git/saltbox/defaults/settings.yml.default']
        - ['/srv/git/saltbox/adv_settings.yml', '/srv/git/saltbox/defaults/adv_settings.yml.default']
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
