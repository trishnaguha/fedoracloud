Contribute to Test Cases
========================

This page documents How to write testcase for Fedora Cloud/Atomic Image, How to run Fedora Cloud/Atomic images on your local system, How to debug/test image.

How to Write new Test Case for Autocloud?
-----------------------------------------

How to write testcases for Fedora Cloud Base and Atomic Images for Autocloud.

What are Autocloud/Tunir/Tunirtests?
-----------------------------------

`Autocloud <https://apps.fedoraproject.org/autocloud/>`_ is part of Fedora Project that we use to test Fedora Cloud Base/Atomic image build on `Koji <http://koji.fedoraproject.org/koji/>`_. It listens to `fedmsg <http://www.fedmsg.com/>`_, and pushes new messages on the status of the tests.
`Tunir <http://tunir.readthedocs.io/>`_ is a simple testing tool **(Continuous Integration Tool)** that runs testcases for the images on Autocloud. `Tunirtests <https://github.com/kushaldas/tunirtests/>`_ is the actual repository that contains those testcases.

What We are Using?
------------------

The tests are `Python Unittests <https://docs.python.org/3/library/unittest.html/>`_. As from Fedora 23 our Cloud Base and Atomic images have only Python3, we write the testcases in Python3 only.

How They Work?
--------------

While we do manual testing, we generally run some commands, check the output or exit code to verify if the command did what it was supposed to do. We have automated this task using Python Unittests. If you have checked the codebase of Tunirtests, you will find that we have imported some modules like ``system``, ``if_atomic``.

.. code-block:: bash

   from .testutils import system, if_atomic

The ``system`` function can execute a terminal command, it returns the output, any error message, and the exit code of the command given. We use ``if_atomic`` to functions to skip or run tests particularly for an Atomic image. The definition of ``system`` and ``if_atomic`` are given in the file `testutils.py <https://github.com/kushaldas/tunirtests/blob/master/testutils.py/>`_.

Anatomy of a Test Case
----------------------

For example we have to check if ``docker`` rpm is installed in the Atomic image. We can do it manually using rpm command in the following way.

.. code-block:: bash

   rpm -q docker

If the docker package is not installed, it will say that in the output of the command.

.. code-block:: bash

   package docker is not installed

**Now let's convert this to Python Unittest to automate the task**

.. code-block:: python

   @unittest.skipUnless(if_atomic(), "It's not an Atomic image")
   class TestDockerInstalled(unittest.TestCase):

       def test_run(self):
            """Test to check if docker is installed on Atomic image"""
            out, err, eid = system('rpm -q docker')
            out = out.decode('utf-8')
            self.assertFalse('not installed' in out, out)

We are converting back the ``out`` to a string from bytes using ``decode`` method. The rest you can easily understand :).

Testing Fedora Cloud/Atomic images on Local system
--------------------------------------------------

How to test Fedora Cloud Base/Atomic images on local system?

Download Fedora Cloud/Atomic Image
----------------------------------

Download Fedora Cloud/Atomic Image from `https://dl.fedoraproject.org/pub/fedora/linux/releases <https://dl.fedoraproject.org/pub/fedora/linux/releases/>`_.


Clone Tunir and Install Dependencies
------------------------------------

.. code-block:: bash

   git clone https://github.com/kushaldas/tunir.git
   sudo dnf -y install libguestfs-tools python-paramiko docker-io vagrant-libvirt ansible net-tools python-crypto python2-typing python2-systemd


Clone Tunirtests
----------------

.. code-block:: bash

   cd tunir
   git clone https://github.com/kushaldas/tunirtests.git


How to Execute the Tests
------------------------

.. code-block:: bash

   cd tunirtests
   vim fedora.json

Make sure to replace the ``image field`` with ``image name`` (you downloaded) with full path in the file `fedora.json <https://github.com/kushaldas/tunirtests/blob/master/fedora.json/>`_.

Now run ``localrunner.sh``. We have `localrunner.sh <https://github.com/kushaldas/tunirtests/blob/master/localrunner.sh/>`_ that creates tarball of tests, and runs a webserver at ``port 8000`` using Python ``SimpleHTTPServer`` module. So run it from one terminal. You can stop the server by pressing ``Ctrl + C``. In tunir a job is defined inside ``jobname.txt``, you can see example in `fedora.txt <https://github.com/kushaldas/tunirtests/blob/master/fedora.txt/>`_ file.

.. code-block:: bash

   sudo python3 -m unittest tunirtests.atomictests.TestDockerInstalled -v
   ## sudo python3 -m unittest tunirtests.nongatingtests.TunirNonGatingtestsCpio -v
   ## sudo python3 -m unittest tunirtests.nongatingtests.TunirNonGatingtests -v

Here we execute the actual tests, if you want to reboot the system, you can do it by ``sudo reboot`` command. After any reboot, remeber to add a ``SLEEP 60`` like command to sleep for 60 seconds (or any time based on your system's performance).

If a command returns non zero exit code, you can mark it with ``@@`` in the beginning of the command. If it fails to return a non-zero exit code, tunir will mark that command as failed, and stop the job there. We can also have non-gating tests, if any command starts with ``##``, tunir marks it as non-gating. Non-gating means even if the test fail tunir will continue the commands/tests defined in the jobname.txt. At the end of tunir job it prints the details about non-gating tests like.

Run Test
--------

Edit `fedora.txt <https://github.com/kushaldas/tunirtests/blob/master/fedora.txt/>`_ file and add the your test case there.
Now run

.. code-block:: bash

   sudo tunir --job fedora


How to Debug/Test an Image
--------------------------

You can visit the `Autocloud jobs <https://apps.fedoraproject.org/autocloud/jobs/>`_ page which contains links to all Cloud image build from koji.

First find a failed job from the above mentioned page, or you can randomly take an image from the list of jobs there, click on the left most number in the row (which is the koji build id), this will open the koji build page in another tab.

You will find the images in the results part. Now if you are testing a cloud image, you can download the qcow2 image (or say you upload it to an Openstack cloud). If you are going to test a Vagrant image, feel free to download the corresponding Vagrant image from the koji page (you can use vagrant in a remote server too). Remember that we create Vagrant images for both libvirt and Virtualbox, so please choose the image carefully based on your setup.

Find Where did it Failed on Autocloud
-------------------------------------

If you click on the output link for any job, it will open the output of the given job. Now for a failed job, first check if you can see any failed Python 3 unittest case. If you can find one, you can open up the test manually (they are available in this git repo), and then execute the same commands manually in an instance, if required feel free to submit a Pull Request to the tests.

If you do not see any direct test failure, the look for the output lines just above the following line

    ``Job status: False``

There you may find that there is a SSH exception, or connection refused etc. This means after following the commands in the sequence they ran, the instance failed on the SSH connection in the next command.

Official Fedora tests from QA Team
----------------------------------

`This link <https://fedoraproject.org/wiki/Test_Results:Current_Cloud_Test/>`_ contains the official tests from the Fedora QA team, if you are running them, feel free to update the wiki (after you login) with the results.

Get in touch with Fedora-Cloud Team
-----------------------------------

- Join the mailing list `here <https://lists.fedoraproject.org/archives/list/cloud@lists.fedoraproject.org/>`_.
- Ping kushal, trishnag on IRC channel **#fedora-cloud** on freenode server regarding test.
- Attend weekly meeting on IRC channel **#fedora-meeting-1** on freenode server.
