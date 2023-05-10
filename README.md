# THIS REPOSITORY IS OUTDATED AND WILL BE REMOVED. Please use https://github.com/perfsonar/ansible-playbook-perfsonar instead

perfSONAR deployment example / quickstart
=========================================

This repository contains a set of Ansible playbooks and roles that is suitable to install a full [perfSONAR][ps] environment on multiple machines.  It can be used for a workshop or training setup but can also serve as a quick starting point for a fully customised perfSONAR deployment on any network.

Requirements
------------

The deployment is meant to work with any perfSONAR supported distro:

  - CentOS: http://docs.perfsonar.net/install_centos.html
  - Debian/Ubuntu: http://docs.perfsonar.net/install_debian.html

The hosts will all be managed through Ansible and for that this setup relies on a [fork][tonin-bootstrap] of the [robertdebock.bootstrap][rdbs] role to make all machines working with Ansible.  It also makes use of a [simple users management role][tonin-users] to create and manage the admin users on the machines.  This is just meant as a starting point, you can skip that parts of the setup and provide your own provisioning roles and playbooks taking care of that in your infrastructure.

At the core of the `site.yml` playbook are the [perfsonar-testpoint][ps-testpoint] and [perfsonar-toolkit][ps-toolkit] roles which are making most of the job.

Playbook Variables
------------------

After checking out this repo and creating a new branch for a specific deployment, one will usually want to adapt the following variables for the deployment needs:

  - `perfsonar_web_user` and `perfsonar_web_passwd` to define the define the credential used to access the perfSONAR toolkit admin GUI
  - `perfsonar_optional_packages` is the list of additional optional packages that will be installed alongside the testpoint bundle, see [the debian list][debian-optional] and [the centos list][centos-optional] for more information.  All perfSONAR optional packages are installed per default.
  - and of course the list and characteristics of user accounts.

There is also some provision, in the inventory file as well as in the roles, to use different types of perfSONAR installation, namely: stable, staging or snapshot packages.

Usage
-----

In its simplest form, this playbook and roles can be used with the following command:

    ansible-playbook site.yml

It is usually useful to edit the following before running any Ansible command:

  - edit `inventory/hosts`
  - remove any unneeded directory under `inventory/group_vars/` or edit exisiting files
  - remove any unneeded directory under `inventory/host_vars/` or edit exisiting files
  - change the domain in `inventory/group_vars/all/perfsonar.yml`
  - change the user list in `inventory/group_vars/all/users.yml`
  - create a vault password in the `secret` file and remove/create/edit `inventory/group_vars/all/crypt.yml`

Tags
----

Some tags are useful when invoking this setup, they are meant to run only, or skip, a part of the process.  The following tags are existing:

  - `ps::install` : only install perfSONAR packages and their dependencies
  - `ps::config` : only configure any already installed perfSONAR package
  - `ps::running` : checks that your perfSONAR node is running as intended

Examples,

  - To only install the perfSONAR software without configuring it automatically, one can run:

        ansible-playbook site.yml --tags "ps::install"

  - If the perfSONAR packages are already installed, changing an already existing config can be done by running:

        ansible-playbook site.yml --skip-tags "ps::install"

Dependencies
------------

The [perfsonar-testpoint][ps-testpoint] and [perfsonar-toolkit][ps-toolkit] roles as well as the [bootstrap][tonin-bootstrap] and [users][tonin-users] roles are the main dependencies to this setup.  They are declared in the `requirements.yml` file.

  - use the git submodules, by running `git submodule init; git submodule update` after cloning this repo
  - use Ansible Galaxy (when the perfsonar roles will be published there… which is not yet the case)

Some inventory files are crypted with ansible-vault and the corresponding password is not kept in this git repository, of course!

License
-------

Apache 2.0

Author Information
------------------

This Ansible setup is provided by the [perfSONAR][ps] development team as an example and a starting point for a more complete automated perfSONAR deployment.

Feedback and PR happily received.


[tonin-bootstrap]: https://github.com/tonin/ansible-role-bootstrap
[tonin-users]: https://github.com/tonin/ansible-role-users
[ps-testpoint]: http://github.com/perfsonar/ansible-role-perfsonar-testpoint
[ps-toolkit]: https://github.com/tonin/ansible-role-perfsonar-toolkit
[rdbs]: https://galaxy.ansible.com/robertdebock/bootstrap/
[debian-optional]: http://docs.perfsonar.net/install_debian.html#optional-packages
[centos-optional]: http://docs.perfsonar.net/install_centos.html#optional-packages
[ps]: http://www.perfsonar.net
