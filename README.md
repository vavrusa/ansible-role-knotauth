Knot DNS authoritative
======================

Installs [Knot DNS][knot-dns] authoritative DNS server on Debian/Ubuntu or FreeBSD.

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

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: knot.auth }

License
-------

BSD

Author Information
------------------

* [www.knot-dns.cz][knot-dns]

[knot-dns]: http://www.knot-dns.cz
