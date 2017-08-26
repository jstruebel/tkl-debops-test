DebOps for Turnkey Tests
========================

"DebOps for Turnkey Tests", aka tkl-debops-test, is a set of tests to
validate that the settings of the Turnkey Linux appliances aren't changed
after running the `tkl-debops`_ playbooks.

Installation
------------

tkl-debops-tests requires Ansible and DebOps to be installed.
If using the `Ansible Turnkey appliance`_ the following commands will need
to be run to install and configure tkl-debops for use::

    $ sudo apt-get install python-pip
    $ sudo pip install debops
    $ debops-update
    $ git clone https://github.com/jstruebel/tkl-debops-test.git
    $ cd tkl-debops-test
    $ git clone https://github.com/jstruebel/tkl-debops.git
    $ ./tkl-debops-test-init

Some of the roles in DebOps require Ansible > 1.9 which is the version included
with Turnkey v14.1. To update to the latest Ansible version run the following
command::

    $ sudo pip install --upgrade --upgrade-strategy only-if-needed ansible

Usage
-----

Before running the tests, the ansible inventory linked in the
ansible/inventory/hosts file in the project directory must be edited to
add the host for each appliance to the appropriate group. The tests are
typically run against a remote host. If running the tests manually, the
host needs to be added to the group for the appliance that is being tested.
The group names are just the appliance name prefixed with 'tkl_'. This can
be done in the ``ansible/inventory/hosts`` file.

Each appliance has its own playbook which saves the initial configuration of
the appliance services, runs the debops site.yml and verifies that the
configuration of the appliance didn't change. The project directory is
structured so that it is only required to run
``debops playbooks/tkl_<appliance>.yml`` or ``debops tkl_<appliance>``
in order to run the test suite against that appliance. For example, running
``debops playbooks/tkl_core.yml`` will run the tests against the Turnkey
Linux Core appliance.

These tests and 'tkl_debops'_ are developed and validated by running them against
the appropriate 'Turnkey Linux'_ appliance running in an LXC container hosted on
the 'LXC Turnkey appliance'_. A set of ansible playbooks is included to automate
the setup and teardown of the appliance for testing and continuous integration.
The test_app script included with this project sequences the playbooks as appropriate
to setup a container with a fresh install of the appliance, run the test playbook
against it, and remove the container. This script requires that the LXC host be
configured in the ansible inventory and either use public keys or have SSH password
configured in the inventory file. To use the script enter the following command::

    $ ./test_app <appliance> <port>

For example to test the core appliance::

    $ ./test_app core 2222

Development
-----------

Contributions to this project are welcome. For any new appliances that have
default configurations added to the tkl-debops project, a corresponding
test playbook should be added to this project.
To contribute, fork `this repository`_ and issue a pull request on Github.

.. _tkl-debops: https://github.com/jstruebel/tkl-debops
.. _Ansible Turnkey appliance: https://www.turnkeylinux.org/ansible
.. _this repository: https://github.com/jstruebel/tkl-debops-test
.. _Turnkey Linux: https://www.turnkeylinux.org
.. _LXC Turnkey appliance: https://www.turnkeylinux.org/lxc
