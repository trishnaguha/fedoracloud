Cockpit on Atomic Host
======================

`Cockpit <http://cockpit-project.org/>`_ is a **remote manager for GNU/Linux servers**.

- Cockpit is a server manager that makes it easy to administer your GNU/Linux servers via a web browser.
- Cockpit makes it easy for any sysadmin to perform simple tasks, such as administering storage, inspecting journals and starting and stopping services.
- Jumping between the terminal and the web tool is no problem. A service started via Cockpit can be stopped via the terminal. Likewise, if an error occurs in the terminal, it can be seen in the Cockpit journal interface.
- You can monitor and administer several servers at the same time. Just add them with a single click and your machines will look after its buddies.

The Cockpit team is currently uploading the cockpit container to the Fedora repo on the Docker Hub, but Fedora Release Engineering is working on publishing layered images. We now have a super-privileged container (SPC) for the web service (cockpit-ws) with the bridge, shell, and docker components installed by default on the Atomic host.

This means you can simply run ``atomic run fedora/cockpitws`` as root or with sudo and cockpit will be running on ``port 9090``.

Getting Started
---------------

**Boot up Fedora Atomic instance**

Install the Container
---------------------

Install **cockpitws** container using ``atomic``.

.. code-block:: bash

   # atomic install fedora/cockpitws
   /usr/bin/docker run -ti --rm --privileged -v /:/host fedora/cockpitws /container/atomic-install
   + sed -e /pam_selinux/d -e /pam_sepermit/d /etc/pam.d/cockpit
   + mkdir -p /host/etc/cockpit/ws-certs.d
   + chmod 755 /host/etc/cockpit/ws-certs.d
   + chown root:root /host/etc/cockpit/ws-certs.d
   + mkdir -p /host/var/lib/cockpit
   + chmod 775 /host/var/lib/cockpit
   + chown root:wheel /host/var/lib/cockpit
   + /bin/mount --bind /host/etc/cockpit /etc/cockpit
   + /usr/sbin/remotectl certificate --ensure

There’s a few things going on here in the install method.

Note that we’re exposing the Atomic host root directory to the container at ``/host``. As a SPC, this allows the container to access the host filesystem and make changes. The install method creates a set of directories in ``/etc`` and ``/var`` to persist configs. This means that we don’t need any particular cockpitws container to stick around, any cockpitws container will be able to read the appropriate state from the host. We can upgrade the cockpit image and not worry about losing data. Since ``/etc`` and ``/var`` are writable on an Atomic host, and ``/etc`` content will be appropriately merged on a tree change, cockpit data will also survive an atomic host upgrade as well.

Set up the systemd unit
-----------------------

.. code-block:: bash

   # vi /etc/systemd/system/cockpitws.service

   [Unit]
   Description=Cockpit Web Interface
   Requires=docker.service
   After=docker.service

   [Service]
   Restart=on-failure
   RestartSec=10
   ExecStart=/usr/bin/docker run --rm --privileged --pid host -v /:/host --name %p fedora/cockpitws /container/atomic-run --local-ssh
   ExecStop=-/usr/bin/docker stop -t 2 %p

   [Install]
   WantedBy=multi-user.target


With the container available to docker, we’ll build the systemd unit file next. For local systemd unit files, we want them to reside in ``/etc/systemd/system``.

The ``ExecStart`` line in the unit file looks nearly identical to the ``RUN label``, with one change. When running containers from systemd, we don’t want to use ``docker -d``, instead we want either ``docker -a`` or ``docker --rm``. We’re using ``docker --rm`` here since we don’t need this particular container instance to survice a restart. We are going to name container using the %p tag to pick up the systemd service name, just to make it easier to find in ``docker ps``.

Start the Service
-----------------

Now we can reload systemd to read the new unit file, enable the service to start on reboot, and then start the new cockpitws service.

.. code-block:: bash

   # systemctl daemon-reload
   # systemctl enable cockpitws.service
   Created symlink from /etc/systemd/system/multi-user.target.wants/cockpitws.service to /etc/systemd/system/cockpitws.service.
   # systemctl start cockpitws.service
   # systemctl status cockpitws.service

   ● cockpitws.service - Cockpit Web Interface
   Loaded: loaded (/etc/systemd/system/cockpitws.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2016-08-16 12:42:23 UTC; 10s ago
 Main PID: 2047 (docker)
    Tasks: 6 (limit: 512)
   Memory: 0B
      CPU: 1ms
   CGroup: /system.slice/cockpitws.service
           └─2047 /usr/bin/docker run --rm --privileged --pid host -v /:/host --name cockpitws fedora/cockpitws /container/atomic-run --local-ssh

   Aug 16 12:42:25 atomic.novalocal docker[2047]: + sed -e /pam_selinux/d -e /pam_sepermit/d /etc/pam.d/cockpit
   Aug 16 12:42:25 atomic.novalocal docker[2047]: + mkdir -p /host/etc/cockpit/ws-certs.d
   Aug 16 12:42:25 atomic.novalocal docker[2047]: + chmod 755 /host/etc/cockpit/ws-certs.d
   Aug 16 12:42:25 atomic.novalocal docker[2047]: + chown root:root /host/etc/cockpit/ws-certs.d
   Aug 16 12:42:25 atomic.novalocal docker[2047]: + mkdir -p /host/var/lib/cockpit
   Aug 16 12:42:25 atomic.novalocal docker[2047]: + chmod 775 /host/var/lib/cockpit
   Aug 16 12:42:25 atomic.novalocal docker[2047]: + chown root:wheel /host/var/lib/cockpit
   Aug 16 12:42:25 atomic.novalocal docker[2047]: + /bin/mount --bind /host/etc/cockpit /etc/cockpit
   Aug 16 12:42:25 atomic.novalocal docker[2047]: + /usr/sbin/remotectl certificate --ensure
   Aug 16 12:42:25 atomic.novalocal docker[2047]: INFO: cockpit-ws: Using certificate: /etc/cockpit/ws-certs.d/0-self-signed.cert


Now that the service is up and running, point your web brower at ``port 9090`` on the Atomic host and you should see the Cockpit login page. You’ll need to log in with a user in the ``wheel`` group in order to administrate the system, but you can log in as any user to view the local host. For the published Fedora Atomic cloud image, log in with the fedora credentials and you should be ready to go. You can add other hosts to this Cockpit instance, with the knowledge that reboots and upgrades to the host or the container won’t affect the configuration.
