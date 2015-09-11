Knot DNS authoritative
======================

Installs [Knot DNS][knot-dns] authoritative DNS server on Debian/Ubuntu/RedHat or FreeBSD.

Requirements
------------

None.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

	knot_from_source: false # Default

Set either of this to `true` or `false` to choose installation from sources or distribution packages.

	knot_git_branch: master

If building from sources, pick a git branch or tag.

	knot_install_dir: /usr/local

If building from sources, pick an installation prefix (`/usr/local` means the binary will be installed in `/usr/local/sbin/knotd` for example).

	knot_user: knot
	knot_group: knot

Create a user for running Knot DNS daemon.

	knot_daemon: knot

Pick a different name for Knot DNS daemon service.

	knot_interfaces:
	  - 127.0.0.1
	  - 192.168.1.1@5353

Make Knot DNS listen on specific interfaces or ports. By default it listens on default IPv4/v6 interfaces and localhost.

	knot_zones:
	  - { name: 'example.com', file: '/tmp/example.zone', template: 'default', module: 'mymodule' }

List of enabled zones. `name` is the only mandatory field, rest is undefined by default.
You can reference defined templates or modules here.

	knot_config_extras: |
	  server:
	    rate-limit: 10
	  template:
	    - id: default
	      semantic-checks: on

Extend configuration with server-specific or more advanced configuration here. Here you can define additional templates, ACLs or remotes,
or redefine server options.

Dependencies
------------

None.

Example Playbook
----------------

The role can be configured as a slave using just `knot_zones` and `knot_extras` to define remotes, you can complete these
from host variables or include from a file:

    - hosts: slaves
      roles:
         - role: knot.auth
           knot_zones:
            - { name: 'example.com' }
           knot_extras: |
             remote:
               - id: master
                 address: 192.168.1.1
             acl:
               - id: master_acl
                 address: 192.179.1.1
                 action: notify
             template:
               - id: default
                 master: master
                 acl: master_acl

Example master role is the opposite, except this role doesn't guarantee bootstrapping of the zone files, you have to do this
yourself, for example with [synchronize][ansible-synchronize]:

    - hosts: master01
      roles:
         - role: knot.auth
           knot_zones:
            - { name: 'example.com' }
           knot_keys:
            - { id: 'slave1_key', algorithm: 'hmac-md5', secret: 'Wg==' }
           knot_extras: |
             remote:
               - id: slave01
                 address: 192.168.2.1
                 key: slave_key
             acl:
               - id: slaves
                 address: 192.168.2.0/24
                 action: transfer
                 key: slave_key
             template:
               - id: default
                 storage: /var/lib/zones
                 notify: slave01
                 acl: slaves

License
-------

BSD

Author Information
------------------

* [www.knot-dns.cz][knot-dns]

[knot-dns]: http://www.knot-dns.cz
[ansible-synchronize]: http://docs.ansible.com/ansible/synchronize_module.html
