The eduPERT role
================

This ansible role provides all that is needed to prepare a perfSONAR node for an eduPERT workshop.

Requirements
------------

This should work with CentOS 7 and Debian 9 hosts.

Role Variables
--------------

In default/main.yml:
    - username: the account name to be used by workshop participants

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: smallnodes
      roles:
         - role: edupert

License
-------

Apache 2.0

Author Information
------------------

perfSONAR developers - https://github.com/perfsonar/

