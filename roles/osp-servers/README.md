Role Name
=========

Role to create Openstack instances defined in variable *osp_servers*.

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

    - hosts: localhost
      connection: local
      become: no
      gather_facts: false
      vars:
        osp_servers:
          - instance_name: frontend
            state: present
            image: rhel-guest
            key_name: ansible_ssh
            flavor: m1.medium
            security_group: frontend
            meta: "group=frontends,deployment_name=QA"
          - instance_name: app1
            state: present
            image: rhel-guest
            key_name: ansible_ssh
            flavor: m1.medium
            security_group: apps
            meta: "group=apps,deployment_name=QA"
          - instance_name: app2
            state: present
            image: rhel-guest
            key_name: ansible_ssh
            flavor: m1.medium
            security_group: apps
            meta: "group=apps,deployment_name=QA"
      roles:
      - name: osp-servers


License
-------

BSD
