Ansible Role: FireHOL
=========

Installs [FireHOL](http://firehol.org/). Templates the conf file.

Requirements
------------

Apt or Yum  package manager

Role Variables
--------------

```yaml
firehol_version: 5

firehol_ports:
  - service: some_service
    server: "tcp/1234"
    client: "default"

firehol_interfaces:
  - interface: "any world"
    client_rules: [ "all accept" ]
    server_rules: []
    other_rules: []

firehol_routers:
  - router: "any inface eth0 outface ppp+"
    route_rules: [ "all accept" ]
    other_rules: [ "masquerade" ]
```

Dependencies
------------

None

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
    - role: cmprescott.firehol
      sudo: Yes
      firehol_interfaces:
        - interface: "eth0 mylan"
          other_rules: [ "policy accept" ]
        - interface: "ppp+ internet"
          client_rules: [ "all accept" ]
          server_rules: [ "smtp accept", "http accept", "ftp  accept", "ssh  accept src example.firehol.org" ]
      firehol_routers:
        - router: "mylan2internet inface eth0 outface ppp+"
          route_rules: [ "all accept" ]
          other_rules: [ "masquerade" ]
```

generates 

```shell
interface eth0 mylan
    policy accept

interface ppp+ internet    
    client all  accept
    server smtp accept
    server http accept
    server ftp  accept
    server ssh  accept src example.firehol.org


router mylan2internet inface eth0 outface ppp+
    route all accept
    masquerade
```

License
-------

BSD

Author Information
------------------

Prescott Chris
