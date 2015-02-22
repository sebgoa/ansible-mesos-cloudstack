Ansible Recipes to Deploy Mesos with Marathon on CloudStack
===========================================================

This is based of off Michael Hamrah [playbooks](https://github.com/mhamrah/ansible-mesos-playbook)

Prerequisites
-------------

You will need Ansible and [cs](https://github.com/exoscale/cs) :)

    $ sudo apt-get install -y python-pip
    $ sudo pip install ansible
    $ sudo pip install cs
    $ sudo gem install librarian-ansible

Setup cs
--------

Create a `~/.cloudstack.ini` file with your creds and cloudstack endpoint:

    [cloudstack]
    endpoint = <your cloudstack api endpoint>
    key = <your api access key> 
    secret = <your api secret key> 
    method = post

We need to use the http POST method to pass the userdata to the coreOS instances.

Clone recursive
---------------

    $ git clone --recursive https://github.com/runseb/ansible-mesos-cloudstack.git
    $ cd ansible-mesos-cloudstack

There is the [ansible-cloudstack](https://github.com/resmo/ansible-cloudstack) submodule in there.

Get the Ansible roles needed
----------------------------

    $ sudo librarian-ansible install

Create all the needed hosts
---------------------------

    $ ansible-playbook mesos.yml

Provision the nodes
-------------------

    $ ansible-playbook mesos_cluster.yml

Launch Application on Marathon
------------------------------

    $ curl -XPOST -H 'Content-Type:application/json' -d @nginx.json http://http://185.19.29.75:8080/v2/apps

Dependencies
------------

This depends on the Ansible cloudstack [module](https://github.com/resmo/ansible-cloudstack). It is loaded in this repo as a git submodule.

This module depends on [cs](https://github.com/exoscale/cs), this module will look for the `CLOUDSTACK_ENDPOINT`, `CLOUDSTACK_KEY`, `CLOUDSTACK_SECRET` and `CLOUDSTACK_METHOD` environment variables.

