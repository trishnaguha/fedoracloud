Automate Building Fedora Atomic Host
====================================

`Project Atomic <http://www.projectatomic.io/>`_ hosts are built from standard RPM packages which have been composed into filesystem trees using rpm-ostree. This guide provides method for automation of Building Atomic host(Creating new trees).

One of the primary benefits to Atomic Host and OSTree has been the ability to "configure once, deploy many times" using custom OSTree images.

The procedure and playbook below will enable you to create your own Atomic Host OSTree image. This is the first step in creating your own "distributions" of Atomic Host to install on your cloud servers. Note that it will install a bunch of requirements on your local server, as well as using system resources heavily. As such, you may want to run it on a development machine instead of your personal laptop.


Requirements
------------

- Fedora Atomic QCOW2 Image. The image can be downloaded from here: `Fedora-Atomic <https://getfedora.org/en/atomic/download/>`_.
- Make sure that Ansible is installed on your system. Note that I am installing on my `Fedora-Workstation <https://getfedora.org/en/workstation/download>`_.

.. code:: bash
   $ sudo dnf install ansible

Process
-------
Clone the Git repo on your working machine `Build-Atomic-Host <https://github.com/trishnaguha/build-atomic-host/>`_.

.. code:: bash

    $ git clone https://github.com/trishnaguha/build-atomic-host.git
    $ cd build-atomic-host


Create VM from the QCOW2 Image
------------------------------
The following creates VM from QCOW2 Image where username is ``atomic-user`` and password is ``atomic``.
Here ``atomic-node`` in the instance name.

.. code:: bash

    $ sudo sh create-vm.sh atomic-node /path/to/fedora-atomic25.qcow2
    # For example: /var/lib/libvirt/images/Fedora-Atomic-25-20170131.0.x86_64.qcow2


Start HTTP Server
-----------------
The tree is made available via web server. The following playbook creates directory structure, initializes OSTree repository and starts the HTTP server.

.. code:: bash

    $ ansible-playbook httpserver.yml --ask-sudo-pass

Use ``ip addr`` to check IP Address of the HTTP server.


Give OSTree a name and add HTTP Server IP Address
-------------------------------------------------
Replace the variables given in `vars/atomic.yml <https://github.com/trishnaguha/build-atomic-host/tree/master/vars/atomic.yml/>`_ with OSTree name and HTTP Server IP Address.
For Instance:

.. code:: bash

    # Variables for Atomic host
    atomicname: my-atomic
    httpserver: 192.168.122.1

Here ``my-atomic`` is OSTree name and ``192.168.122.1`` is HTTP Server IP Address.


Run All-in-one Playbook
-----------------------
The following playbook installs requirements, starts HTTP Server, composes OSTree, performs SSH-setup and rebases on created Tree.

.. code:: bash

    $ ansible-playbook main.yml --ask-sudo-pass


Check IP Address of the Atomic instance
---------------------------------------
The following command returns the IP Address of the running Atomic instance

.. code:: bash

    $ sudo virsh domifaddr atomic-node


Reboot
------
Now SSH to the Atomic Host and reboot it so that it can reboot in to the created OSTree:

.. code:: bash

    $ ssh atomic-user@<atomic-hostIP>
    $ sudo systemctl reboot


Verify: SSH to the Atomic Host
------------------------------
Wait for 10 minutes, You may want to go for a Coffee now.

.. code:: bash

    $ ssh atomic-user@192.168.122.221
    [atomic-user@atomic-node ~]$ sudo rpm-ostree status
    State: idle
    Deployments:
    ● my-atomic:fedora-atomic/25/x86_64/docker-host
        Version: 25.1 (2017-02-07 05:34:46)
         Commit: 15b70198b8ec7fd54271f9672578544ff03d1f61df8d7f0fa262ff7519438eb6
         OSName: fedora-atomic

    fedora-atomic:fedora-atomic/25/x86_64/docker-host
        Version: 25.51 (2017-01-30 20:09:59)
         Commit: f294635a1dc62d9ae52151a5fa897085cac8eaa601c52e9a4bc376e9ecee11dd
         OSName: fedora-atomic

Now you have the Updated Tree.

Shout-Out for the following folks:

- `Gerard Braad <http://gbraad.nl/>`_ who mentored me for the project.
- `Jonathon Lebon <https://github.com/jlebon/>`_ who demonstrated Building Atomic host workshop in `DevConf.CZ <https://devconf.cz/>`_, 2017 at Brno. His slides are here: `jlebon-devconf-slides <http://jlebon.com/devconf/slides.pdf/>`_.
