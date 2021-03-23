OSP Instance delete
=========

Role to delete Openstack instances defined in variable *osp_servers*.

Requirements
------------

This role has to be triggered on host that is properly [configured to communicate with OpenStack](https://docs.openstack.org/python-openstackclient/pike/configuration/index.html) cluster.

Role Variables
--------------

| variable name | purpose                  | default value |
|---------------|--------------------------|---------------|
| osp_servers   | Dict of OSP VM instances | `[]`          |
| osp_cloud     | cloud name               | *openstack*   |

Dependencies
------------

None

Example Playbook
----------------

    - hosts: workstation
      gather_facts: false
      become: yes
      vars:
        osp_cloud: openstack
        osp_servers:
          - instance_name: frontend
          - instance_name: app1
      roles:
        - name: osp-instance-delete

License
-------

BSD
