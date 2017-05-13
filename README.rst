DebOps for Turnkey Tests
========================

"DebOps for Turnkey Tests", aka tkl-debops-tests, is a set of tests to
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
    $ git clone https://github.com/jstruebel/tkl-debops-tests.git
    $ cd tkl-debops-tests
    $ git clone https://github.com/jstruebel/tkl-debops.git
    $ ./tkl-debops-tests-init

Some of the roles in DebOps require Ansible > 1.9 which is the version included
with Turnkey v14.1. To update to the latest Ansible version run the following
command::

    $ sudo pip install --upgrade --upgrade-strategy only-if-needed ansible

Usage
-----

Before running the tests, the ansible inventory linked in the
ansible/inventory/hosts file in the project directory must be edited to
add the host for each appliance to the appropriate group. The simplest
is to run the tests from within the appliance itself in which case the
localhost needs to be added to the group for that appliance. The group
names are just the appliance name prefixed with "tkl_". The tests can
also be run against a remote host if that is preferred.

Each appliance has its own playbook which saves the initial configuration of
the appliance services, runs the debops site.yml and verifies that the
configuration of the appliance didn't change. The project directory is
structured so that it is only required to run
``debops playbooks/tkl_<appliance>``
in order to run the test suite against that appliance. For example, running
``debops playbooks/tkl_core.yml`` will run the tests against the Turnkey
Linux Core appliance.

Development
-----------

Contributions to this project are welcome. For any new appliances that have
default configurations added to the tkl-debops project, a corresponding
test playbook should be added to this project.
To contribute, fork `this repository`_ and issue a pull request on Github.

.. _tkl-debops: https://github.com/jstruebel/tkl-debops
.. _this repository: https://github.com/jstruebel/tkl-debops-tests
