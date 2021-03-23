OSP Setup
=========

Role to set network resources in OpenStack cloud.

Requirements
------------

This role has to be triggered on host that is properly [configured to communicate with OpenStack](https://docs.openstack.org/python-openstackclient/pike/configuration/index.html) cluster.

Role Variables
--------------


| variable name | purpose                  | default value |
|---------------|--------------------------|---------------|
| osp_networks   | Declare external and internal networks |  see below |
| osp_router     | cloud name               | *openstack*   |

Default Values:

```yaml
osp_networks:
  # Public Facing Network
  external:
    cloud_name: openstack
    network_name: ext_network
    provider_network_type: flat
    provider_physical_network: datacentre
    subnet_name: ext_subnet
    cidr_network: "192.0.2.0/24"
    state: present
    external: true
    gateway_ip: 192.0.2.254
    admin_state: yes
    ip_version: 4
    enable_dhcp: no
    allocation_pool_start: 192.0.2.150
    allocation_pool_end: 192.0.2.200


  # Private Facing Network
  internal:
    cloud_name: openstack
    network_name: int_network
    subnet_name: int_subnet
    cidr_network: "172.16.0.0/24"
    state: present
    external: false
    admin_state: yes
    ip_version: 4
    enable_dhcp: yes

  # Public Facing Router connecting the two networks
osp_router:
  router:
    cloud_name: openstack
    state: present
    name: router
    network: ext_network
    interfaces: int_subnet
```

Dependencies
------------

None

Example Playbook
----------------

    - hosts: workstation
      become: false
      roles:
        - osp-setup

License
-------

BSD
