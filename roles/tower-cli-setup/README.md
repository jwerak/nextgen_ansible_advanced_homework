Tower CLI Setup
=========

Role to prepare server to use ansible tower cli tool *tower-cli*.

Requirements
------------

None

Role Variables
--------------


| variable name            | purpose                                | default value                             |
|--------------------------|----------------------------------------|-------------------------------------------|
| tower_cli_conf_file_path | Set location of tower_cli config file  | /root/.tower_cli.cfg                      |
| tower_cli_tower_host     | Ansible Tower host FQDN                | tower1.{{tower_guid}}.example.opentlc.com |
| tower_cli_username       | Ansible Tower user                     | admin                                     |
| tower_cli_password       | Ansible Tower password                 | secret_password                           |
| tower_cli_verify_ssl     | Verify ssl connection to Ansible Tower | false                                     |

Dependencies
------------

None

Example Playbook
----------------


    - hosts: towerclients
      gather_facts: false
      become: true
      vars:
        tower_cli_conf_file_path: /root/.tower_cli.cfg
        tower_cli_tower_host: tower.example.com
      roles:
        - name: tower-cli-setup


License
-------

BSD
