perfSONAR deployment for workshops and training
===============================================

This is a set of Ansible playbooks and roles that is suitable to install a full [perfSONAR][ps] environment on multiple machines.  It is suitable for a workshop or training setup but can also serve as a starting point for a fully customised perfSONAR deployment on any network.

Requirements
------------

The deployment is meant to work with any perfSONAR supported distro:

  - CentOS: http://docs.perfsonar.net/install_centos.html
  - Debian/Ubuntu: http://docs.perfsonar.net/install_debian.html

The hosts will all be managed through Ansible and for that this setup relies on the [robertdebock.bootstrap][rdbs] role to make all machines working with Ansible.  You can skip that part of the setup and provide your own privioning roles and playbooks taking care of that in your infrastructure.

The site.yml playbook also requires the perfsonar-testpoint and perfsonar-toolkit roles to be available as these are making most of the job.

Playbook Variables
------------------

I usually adapt the following variables for my workshop setups:

  - `perfsonar_optional_packages` is the list of additional optional packages you want to install with the testpoint bundle, see [the debian list][debian-optional] and [the centos list][centos-optional] for more information.  All optional packages are installed per default.
  - `perfsonar_ntp_servers`: depending on the place where the setup is established, I provide close enough and reliable NTP servers,

Tags
----

Some tags are useful when invoking this setup, they are meant to run only, or skip, a part of the process.  The following tags are existing:

  - `ps::install` : only install perfSONAR packages and their dependencies
  - `ps::config` : only configure any already installed perfSONAR package
  - `ps::running` : checks that your perfSONAR node is running as intended

Examples,

  - If you only want to install the perfSONAR software without configuring it automatically, you can run:

        ansible-playbook site.yml --tags "ps::install"

  - If you have already installed the perfSONAR packages and you only want to change an already existing config, you can run:

        ansible-playbook site.yml --skip-tags "ps::install"

Dependencies
------------

The perfsonar-testpoint and perfsonar-toolkit roles as well as the bootstrap role from Robert De Bock are dependencies to this setup.  Git submodules are provided to make for an easy starting point.  This means you have 2 different ways to use this setup:

  - use the git submodules, by running `git submodule init; git submodule update` after cloning this repo
  - use Ansible Galaxy (when the perfsonar roles will be published there…)

License
-------

Apache 2.0

Author Information
------------------

I'm a [perfSONAR][ps] developer and I provide this Ansible setup for anyone having similar requirements as mine to quickly setup a workshop, training or testing perfSONAR deployment.  Feedback and PR happily received.


[rdbs]: https://galaxy.ansible.com/robertdebock/bootstrap/
[debian-optional]: http://docs.perfsonar.net/install_debian.html#optional-packages
[centos-optional]: http://docs.perfsonar.net/install_centos.html#optional-packages
[ps]: http://www.perfsonar.net
