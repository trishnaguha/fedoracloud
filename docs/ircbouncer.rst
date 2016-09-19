IRC Bouncer: ZNC
================

This page describes how to use IRC Bouncer on Atomic host. We will run ZNC.

**First you will need to boot up an atomic host**

Copy the **Sources** down from here `Fedora Dockerfile for IRC Bouncer ZNC <https://github.com/fedora-cloud/Fedora-Dockerfiles/tree/master/znc/>`_ on Atomic host.

Now perform the build with the following command:

    ``docker build -t fedora/znc``

After the build is successful run the container with the following command:

    ``atomic run fedora/znc``

**Note** that is you are not using Atomic host, you can run the container with this command:
    ``docker run -d --name znc -p 6667:6667 fedora/znc``

Now do ``docker ps -a`` to check if znc is running.

**Note:**

    If you want to set your own nick and Server edit the file `znc.conf.default <https://github.com/fedora-cloud/Fedora-Dockerfiles/blob/master/znc/root/usr/share/container-scripts/znc/znc.conf.default/>`_ and perform Docker build.

