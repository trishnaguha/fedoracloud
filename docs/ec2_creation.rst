Creating EC2 Images with Boxgrinder
===================================

Introduction
------------

Creating your own custom images for EC2 is a quick and easy process. While there are many ways to create an EC2 image, this page will walk you through using `boxgrinder <http://boxgrinder.org/>`_. Boxgrinder is chosen here because the packages are in Fedora, and it is a simple process that anyone can use. The Cloud SIG maintains basic appliance definition files in the `cloud-kickstarts git repository <http://git.fedorahosted.org/git/?p=cloud-kickstarts.git/>`_ on fedorahosted.org.

Local creation or EC2 based creation
------------------------------------

Install Boxgrinder on your existing Fedora host by running:

.. code-block:: bash

   # dnf install -y rubygem-boxgrinder-core rubygem-boxgrinder-build

Or, if you would rather have the image creation and publish happen on AMI hosts and save the upload time, you can run boxgrinder in one of the projects supported `meta-appliances <http://boxgrinder.org/download/boxgrinder-build-meta-appliance/>`_.

Configuration
-------------

First, create a config file at ``/root/.boxgrinder/config`` using the following template. Insert appropriate values, see: http://boxgrinder.org/tutorials/boxgrinder-build-plugins/#S3_Delivery_Plugin, for more information.

.. code-block:: none

   plugins:
     s3:
       access_key: AWS_ACCESS_KEY                        # (required)
       secret_access_key: AWS_SECRET_ACCESS_KEY          # (required)
       bucket: stormgrind-test                           # (required)
       account_number: 0000-0000-0000                    # (required)
       path: /images                                     # default: /
       cert_file: /home/a/cert-ABCD.pem                  # required only for ami type
       key_file: /home/a/pk-ABCD.pem                     # required only for ami type
     ebs:
       access_key: AWS_ACCESS_KEY                        # required
       secret_access_key: AWS_SECRET_ACCESS_KEY          # required
       account_number: AWS_ACCOUNT_NUMBER                # required
       delete_on_termination: false                      # default: true

Creation
--------

Actual image creation requires an appliance definition file to get started. The Cloud SIG maintains basic configuration files for current Fedora releases in `cloud-kickstarts git repository <http://git.fedorahosted.org/git/?p=cloud-kickstarts.git/>`_ on fedorahosted.org. You can grab the configuration file from the boxgrinder directory here, and modify it add packages as necessary for your application. More advanced configuration instructions, including post install scripting, are available on the `boxgrinder website <http://boxgrinder.org/tutorials/appliance-definition/>`_. Once you have the configuration file you want, it is as simple as running a single command:

For an S3 backed image:

.. code-block:: bash

   # boxgrinder-build /path/to/your/fedora.appl -p ec2 -d ami

For an EBS backed image:

.. code-block:: bash

   # boxgrinder-build /path/to/your/fedora.appl -p ec2 -d ebs


This will build your image, upload it, and register the AMI, though you will have to make it public yourself. Note that boxgrinder works through plugins, and everything is done in steps. If you want both S3 and EBS based images, run the commands in order, and the EBS image will use the output from the S3 backed to save considerable time. If you require any assistance, people are always willing to help on irc.freenode.net in `#fedora-cloud <irc://irc.freenode.net/fedora-cloud/>`_, or on the `fedora-cloud-sig mailing list <https://admin.fedoraproject.org/mailman/listinfo/cloud/>`_.
