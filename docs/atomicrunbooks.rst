Cloud/Atomic Runbooks
=====================

This page documents how Atomic images are generated, updated, etc. to provide a better understanding for Cloud Working Group members.

Prerequisites
-------------

- Idea about `Project Atomic <http://www.projectatomic.io/>`_ and `What Fedora Project is Doing with it <https://fedoraproject.org/wiki/Changes/Atomic_Cloud_Image/>`_.

ostree Command Usage
--------------------

The ``ostree summary`` command runs on ostree repositories. It generates a summary file describing the repository, which is the file that MirrorManager will need to be updated to look for.

In order to create an ostree repository locally, you can run ``mkdir REPO && ostree --repo=REPO init --mode=archive-z2``, where **REPO** is the desired path of the repository. This command will set up the proper file structure for a new ostree repository.

The ``rpm-ostree compose tree`` command can be used to capture a set of RPMs as ostree repository commits. This command takes a **treefile** as an argument, and is intended to be used on build servers.

Treefiles should be formatted as JSON. Documentation on creating a treefile can be found `here <https://github.com/projectatomic/rpm-ostree/blob/master/docs/manual/treefile.md/>`_.

Official Documentation on `RPM-OSTree <https://github.com/projectatomic/rpm-ostree/blob/master/README.md/>`_ is `here <https://rpm-ostree.readthedocs.org/en/latest/>`_.

Compose Script Operations
-------------------------

Scripts that compose images need to be updated to generate the files required by Atomic. All the scripts need to run ``ostree summary -u/--update`` in order to generate ostree summary files for each compose.

Image (Qcow2) Generation
------------------------

The Atomic cloud images are built using ImageFactory, which calls into Anaconda using a kickstart file. The kickstart file refers to the tree composed above (just like how with mainline packages, the kickstart file points to yum repositories).

The kickstart files for Atomic are located here: `https://pagure.io/fedora-atomic <https://pagure.io/fedora-atomic/>`_.

**Further Reading:** Worknotes by `Kushal Das <https://kushaldas.in/>`_ : `http://worknotes.readthedocs.io <http://worknotes.readthedocs.io/>`_.
