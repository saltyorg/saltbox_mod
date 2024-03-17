# saltbox_mod

Environment for managing custom Ansible roles within a Saltbox host.

## Installation

```bash
sb install saltbox-mod
```

Alternatively:
```bash
git clone https://github.com/saltyorg/saltbox_mod.git /opt/saltbox_mod
```

## Usage

### From Scratch

1. Create folders for the Ansible role:

    ```bash
    mkdir -p /opt/saltbox_mod/roles/newrole/{defaults,tasks}
    ```

1. Place the defaults and tasks files in there:

    ```bash
    touch /opt/saltbox_mod/roles/newrole/{defaults,tasks}/main.yml
    ```

1. Code your role by adding variables and tasks to the respective files.

1. (_Legacy_*) Optionally, add custom variables into `settings.yml`:

    ```bash
    /opt/saltbox_mod/settings.yml
    ```
   
   &ast; Use of the [Inventory system](https://docs.saltbox.dev/saltbox/inventory) is now preferred over this method.
    
1. Add the Ansible role and tags to the `saltbox_mod.yml` playbook:

    To edit:

    ```bash
    $EDITOR /opt/saltbox_mod/saltbox_mod.yml
    ```

    Add the following line in the appropriate section under `roles:`:

    ```yaml
        - { role: newrole, tags: ['newrole'] }
    ```

    Final result:

    ```yaml
    ---
    - hosts: localhost
      module_defaults:
        ansible.builtin.setup:
          fact_path: "/srv/git/saltbox/ansible_facts.d"
      vars_files:
        - settings.yml
        - ['/srv/git/saltbox/accounts.yml', '/srv/git/saltbox/defaults/accounts.yml.default']
        - ['/srv/git/saltbox/settings.yml', '/srv/git/saltbox/defaults/settings.yml.default']
        - ['/srv/git/saltbox/adv_settings.yml', '/srv/git/saltbox/defaults/adv_settings.yml.default']
      roles:
        # Reqs
        - { role: pre_tasks, tags: ['always', 'pre_tasks'] }
        # Apps Start
        - { role: helloworld, tags: ['helloworld'] }
        - { role: myrole, tags: ['myrole'] }
        - { role: newrole, tags: ['newrole'] }
        # Apps End
    ```

    Caution: The `pre_tasks` role is required and should not be removed.

1. Deploy the Ansible role:

    ```bash
    sb install mod-newrole
    ```

    Alternatively :
    ```bash
    sudo ansible-playbook saltbox_mod.yml --tags newrole
    ```

---

### From an Existing Role

Steps 1 to 3 can be simplified by using the `helloworld` role as a template.
It should be usable without too much modification for most web apps that use a single web port.

```bash
cp -r /opt/saltbox_mod/roles/helloworld /opt/saltbox_mod/roles/newrole && \
sed -i 's/helloworld/newrole/g' /opt/saltbox_mod/roles/newrole/*/main.yml
```

Then edit the defaults settings:

```bash
$EDITOR /opt/saltbox_mod/roles/newrole/defaults/main.yml
```

At the very minimum, you may expect to have to update the following variables:

```yaml
newrole_web_port:
newrole_docker_image:
newrole_docker_envs_default:
newrole_docker_volumes_default:
```

Proceed to step 4.
