Config Tower
=========

Ansible role used to configure Ansible Tower objects such as Job Templates, Credentials and Inventories.
The role is not currently configurable and requires major re-structure.


Requirements
------------

None

Role Variables
--------------

| variable name       | purpose                                   | default value          |
|---------------------|-------------------------------------------|------------------------|
| tower_cli_file_path | File path of tower-cli configuration file | */root/.tower_cli.cfg* |


Dependencies
------------

None

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }
    - hosts: localhost
      connection: local
      gather_facts: false
      become: false
      vars:
        tower_cli_conf_file_path: /root/.tower_cli.cfg
      roles:
        - name: config-tower

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
