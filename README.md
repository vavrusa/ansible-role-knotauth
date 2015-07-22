Knot DNS authoritative
======================

Installs [Knot DNS][knot-dns] authoritative DNS server on Debian/Ubuntu or FreeBSD.

Requirements
------------

None.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

	knot_from_source: true
	knot_from_pkgs: false

Set either of this to `true` or `false` to choose installation from sources or distribution packages.

	knot_git_branch: master

If building from sources, pick a git branch or tag.

	knot_install_dir: /usr/local

If building from sources, pick an installation prefix (`/usr/local` means the binary will be installed in `/usr/local/sbin/knotd` for example).

	knot_user: knot

Create a user for running Knot DNS daemon.

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: knot.auth, knot_from_pkgs: true }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).

[knot-dns]: http://www.knot-dns.cz