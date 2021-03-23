Setup Workstation
=========

Configure server to serve as workstation for purpose of homework assignment project.
Workstation is mainly used as isolated group node.

Requirements
------------

None

Role Variables
--------------

None

Dependencies
------------

None

Example Playbook
----------------


    - hosts: workstation
      become: yes
      roles:
        - name: setup-workstation

License
-------

BSD
