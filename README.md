Gluster Servers Role
====================

This role deploys a gluster cluster on a server group

Requirements
------------


Role Variables
--------------

gluster_ferm_support: yes
gluster_interface: eth1
gluster_servers

Dependencies
------------

This role requires [ansible-ferm](https://github.com/leucos/ansible-ferm).
However, no hard dependency is enforced in the meta file.

It is up to you to open proper ports for gluster servers communication.

For instance!

  - role: ansible-ferm
    when: gluster_ferm_support
    ferm_input_group_list:
      - type: 'dport_accept'
        dport: [ '24007' ]
        saddr: '{{ play_hosts }}'
        weight: '50'
        filename: 'gluster_probes_accept'
  - role: ansible-ferm
    when: gluster_ferm_support
    ferm_input_group_list:
      - type: 'dport_accept'
        protocols: ['tcp', 'udp']
        dport: [ '111', '2049', '24007', '38465', '38466', '38467', '49152', '49153', '49154', '49155' ]
        saddr: [ '{{ gluster_client_group }}' ]
        weight: '50'
        filename: 'gluster_clients_accept'

License
-------

MIT

Author Information
------------------

@leucos

