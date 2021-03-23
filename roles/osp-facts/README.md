OSP Facts
=========

Read VM Instance info from OpenStack and use it to create in-memory inventory.

Requirements
------------

This role has to be triggered on host that is properly [configured to communicate with OpenStack](https://docs.openstack.org/python-openstackclient/pike/configuration/index.html) cluster.

Role Variables
--------------

None

Dependencies
------------

None

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: workstation
      gather_facts: false
      roles:
        - name: osp-facts

License
-------

BSD
