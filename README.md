
**WORK IN PROGRESS !!!**

Don't use it on production environment until it will be released !!!

If you have any questions please don't hesitate to ask me:

aepifanov@mirantis.com

Any comments/suggestions are welcome :)

Prerequisites:
--------------

For Ansible installing you can use [install_ansible.sh](install_ansible.sh) script which
actually adds standard CentOS and EPEL reposistories for installing Ansible, installs Ansible
and then deleteis these repos for avoiding any issues with compatibility with Fuel services.

```
./install_ansible.sh
```

Install:
--------

Clone Git repository from GitHub on Fuel Master node:
```
git clone https://github.com/aepifanov/mos_mu.git
```

Configuration file:
-------------------

Conf file contains very important step flags:

[Apply MU steps](playbooks/vars/steps/apply_mu.yml)

Documentation:
--------------

[Architecture](doc/architecture.md)

Usage:
------

First of all you need to update Fuel master node:
```
ansible-playbook playbooks/update_fuel.yml
```

Then you can update OpenStack environments.

For the first step we would recommend to gather current customizations:
```
ansible-playbook playbooks/gather_customizations.yml -e '{"env_id":<env_id>}'
```

Then check that all customizations are applied on new versions
```
ansible-playbook playbooks/verify_patches.yml -e '{"env_id":<env_id>}'
```

It is also strongly recommended to identify and copy common customizations in
**patches** folder on Fuel and disable **use_current_customization** flag and
manage patches to successfully execute previous **verify_patches.yml** step.

Then you can continue to apply MU on environment
```
ansible-playbook playbooks/apply_mu.yml -e '{"env_id":<env_id>}'
```

This playbook contains all previous steps as well, so it might be used from
the beginning, but in this case it will apply the current customizations
and patches from **patches** folder from Fuel on each node and you will
get exactly the same customizations that were before.

Rollback:
---------

Rollback (actually pseudo rollback) playbook can return your cluster on any
specified release and apply gathered customizations:
```
ansible-playbook playbooks/rollback.yml -e '{"env_id":<env_id>,"rollback":"<release_name>"}'
```
