Docker Image
============

Fedora Docker images are available to download via `Docker Hub <https://hub.docker.com/_/fedora/>`_.
You can also find it `here <https://getfedora.org/en/cloud/download/docker.html/>`_.

Rationale
---------

We have the ability to **respin** Fedora's Docker images when packages are updated, but need to determine a policy for what package updates trigger a rebuild. Fedora does not update its ISO images between release cycles, but instead depends on users to use update tools to bring Fedora up to date once an installation is finished.

However, users are going to interact with Docker images somewhat differently than a traditional system. We should assume that users will not be using ``dnf update`` or other tools to keep an image updated.

To put it another way, when a user/developer does ``docker pull fedora`` they will assume that the image is ready to use and does not require a round of updates before it is usable to deploy/work with.

Pull The Image
--------------

``docker pull fedora``

Start Container
---------------

``docker run -it fedora /bin/bash``

Current set of Packages Available in Fedora Docker Image
--------------------------------------------------------

Latest Fedora Docker Image now has **163 packages** installed.

.. code-block:: bash

   libreport-filesystem-2.7.1-1.fc24.x86_64
   tzdata-2016d-1.fc24.noarch
   xkeyboard-config-2.17-2.fc24.noarch
   ncurses-base-6.0-5.20160116.fc24.noarch
   fedora-release-24-1.noarch
   filesystem-3.2-37.fc24.x86_64
   nss-softokn-freebl-3.23.0-1.0.fc24.x86_64
   glibc-common-2.23.1-7.fc24.x86_64
   ncurses-libs-6.0-5.20160116.fc24.x86_64
   libsepol-2.5-3.fc24.x86_64
   pcre-8.38-11.fc24.x86_64
   zlib-1.2.8-10.fc24.x86_64
   bzip2-libs-1.0.6-20.fc24.x86_64
   libgpg-error-1.21-2.fc24.x86_64
   expat-2.1.1-1.fc24.x86_64
   nss-util-3.23.0-1.0.fc24.x86_64
   libcom_err-1.42.13-4.fc24.x86_64
   libattr-2.4.47-16.fc24.x86_64
   libcap-2.24-9.fc24.x86_64
   popt-1.16-7.fc24.x86_64
   libidn-1.32-2.fc24.x86_64
   libxml2-2.9.3-3.fc24.x86_64
   p11-kit-0.23.2-2.fc24.x86_64
   libassuan-2.4.2-2.fc24.x86_64
   readline-6.3-8.fc24.x86_64
   gmp-6.1.0-2.fc24.x86_64
   nss-softokn-3.23.0-1.0.fc24.x86_64
   libseccomp-2.3.1-0.fc24.x86_64
   libnfnetlink-1.0.1-8.fc24.x86_64
   libtasn1-4.8-1.fc24.x86_64
   kmod-22-4.fc24.x86_64
   libcomps-0.1.7-4.fc24.x86_64
   e2fsprogs-libs-1.42.13-4.fc24.x86_64
   libmetalink-0.1.2-9.fc24.x86_64
   cyrus-sasl-lib-2.1.26-26.2.fc24.x86_64
   gawk-4.1.3-3.fc24.x86_64
   libpsl-0.13.0-1.fc24.x86_64
   elfutils-default-yama-scope-0.166-2.fc24.noarch
   ncurses-6.0-5.20160116.fc24.x86_64
   libsss_nss_idmap-1.13.4-3.fc24.x86_64
   libverto-0.2.6-6.fc24.x86_64
   openssl-libs-1.0.2h-1.fc24.x86_64
   crypto-policies-20151104-2.gitf1cba5f.fc24.noarch
   libblkid-2.28-2.fc24.x86_64
   libfdisk-2.28-2.fc24.x86_64
   gzip-1.6-10.fc24.x86_64
   cracklib-dicts-2.9.6-2.fc24.x86_64
   libpwquality-1.3.0-4.fc24.x86_64
   dbus-libs-1.11.2-1.fc24.x86_64
   nss-sysinit-3.23.0-1.2.fc24.x86_64
   openldap-2.4.44-1.fc24.x86_64
   shared-mime-info-1.6-1.fc24.x86_64
   pinentry-0.9.7-2.fc24.x86_64
   libssh2-1.7.0-5.fc24.x86_64
   qrencode-libs-3.4.2-6.fc24.x86_64
   libsemanage-2.5-2.fc24.x86_64
   libutempter-1.1.6-8.fc24.x86_64
   util-linux-2.28-2.fc24.x86_64
   system-python-libs-3.5.1-7.fc24.x86_64
   python3-pip-8.0.2-1.fc24.noarch
   python3-3.5.1-7.fc24.x86_64
   python3-six-1.10.0-2.fc24.noarch
   libmnl-1.0.3-11.fc24.x86_64
   iptables-1.4.21-16.fc24.x86_64
   device-mapper-libs-1.02.122-1.fc24.x86_64
   systemd-229-8.fc24.x86_64
   libnghttp2-1.7.1-1.fc24.x86_64
   curl-7.47.1-4.fc24.x86_64
   libarchive-3.1.2-17.fc24.x86_64
   rpm-libs-4.13.0-0.rc1.27.fc24.x86_64
   libsolv-0.6.20-3.fc24.x86_64
   python3-hawkey-0.6.3-2.fc24.x86_64
   rpm-plugin-systemd-inhibit-4.13.0-0.rc1.27.fc24.x86_64
   gnupg2-2.1.11-3.fc24.x86_64
   python3-pygpgme-0.3-15.fc24.x86_64
   python3-librepo-1.7.18-2.fc24.x86_64
   rpm-python3-4.13.0-0.rc1.27.fc24.x86_64
   dnf-1.1.9-2.fc24.noarch
   bash-completion-2.3-1.fc24.noarch
   e2fsprogs-1.42.13-4.fc24.x86_64
   diffutils-3.3-13.fc24.x86_64
   gpg-pubkey-81b46521-55b3ca9a
   libgcc-6.1.1-2.fc24.x86_64
   dnf-conf-1.1.9-2.fc24.noarch
   emacs-filesystem-25.0.94-1.fc24.noarch
   coreutils-common-8.25-5.fc24.x86_64
   fedora-repos-24-1.noarch
   setup-2.10.1-1.fc24.noarch
   basesystem-11-2.fc24.noarch
   glibc-all-langpacks-2.23.1-7.fc24.x86_64
   glibc-2.23.1-7.fc24.x86_64
   bash-4.3.42-5.fc24.x86_64
   libstdc++-6.1.1-2.fc24.x86_64
   libselinux-2.5-3.fc24.x86_64
   xz-libs-5.2.2-2.fc24.x86_64
   info-6.1-2.fc24.x86_64
   libdb-5.3.28-14.fc24.x86_64
   nspr-4.12.0-1.fc24.x86_64
   elfutils-libelf-0.166-2.fc24.x86_64
   audit-libs-2.5.2-1.fc24.x86_64
   libacl-2.2.52-11.fc24.x86_64
   libuuid-2.28-2.fc24.x86_64
   libgcrypt-1.6.4-2.fc24.x86_64
   chkconfig-1.7-2.fc24.x86_64
   libffi-3.1-9.fc24.x86_64
   sed-4.2.2-15.fc24.x86_64
   grep-2.25-1.fc24.x86_64
   lua-5.3.2-3.fc24.x86_64
   sqlite-libs-3.11.0-3.fc24.x86_64
   kmod-libs-22-4.fc24.x86_64
   lz4-r131-2.fc24.x86_64
   libcap-ng-0.7.7-4.fc24.x86_64
   p11-kit-trust-0.23.2-2.fc24.x86_64
   nettle-3.2-2.fc24.x86_64
   acl-2.2.52-11.fc24.x86_64
   libss-1.42.13-4.fc24.x86_64
   libdb-utils-5.3.28-14.fc24.x86_64
   libksba-1.3.4-1.fc24.x86_64
   libunistring-0.9.4-3.fc24.x86_64
   file-libs-5.25-6.fc24.x86_64
   elfutils-libs-0.166-2.fc24.x86_64
   libsss_idmap-1.13.4-3.fc24.x86_64
   keyutils-libs-1.5.9-8.fc24.x86_64
   krb5-libs-1.14.1-6.fc24.x86_64
   coreutils-8.25-5.fc24.x86_64
   ca-certificates-2016.2.7-1.0.fc24.noarch
   libmount-2.28-2.fc24.x86_64
   gnutls-3.4.12-1.fc24.x86_64
   cracklib-2.9.6-2.fc24.x86_64
   pam-1.2.1-5.fc24.x86_64
   systemd-libs-229-8.fc24.x86_64
   nss-3.23.0-1.2.fc24.x86_64
   nss-tools-3.23.0-1.2.fc24.x86_64
   glib2-2.48.1-1.fc24.x86_64
   libsecret-0.18.5-1.fc24.x86_64
   pkgconfig-0.29-2.fc24.x86_64
   libxkbcommon-0.5.0-4.fc24.x86_64
   ustr-1.0.4-21.fc24.x86_64
   shadow-utils-4.2.1-8.fc24.x86_64
   libsmartcols-2.28-2.fc24.x86_64
   gdbm-1.11-7.fc24.x86_64
   python3-libs-3.5.1-7.fc24.x86_64
   python3-setuptools-20.1.1-1.fc24.noarch
   python3-libcomps-0.1.7-4.fc24.x86_64
   python3-iniparse-0.4-19.fc24.noarch
   libnetfilter_conntrack-1.0.4-6.fc24.x86_64
   device-mapper-1.02.122-1.fc24.x86_64
   cryptsetup-libs-1.7.1-1.fc24.x86_64
   dbus-1.11.2-1.fc24.x86_64
   libcurl-7.47.1-4.fc24.x86_64
   lzo-2.08-8.fc24.x86_64
   rpm-plugin-selinux-4.13.0-0.rc1.27.fc24.x86_64
   rpm-4.13.0-0.rc1.27.fc24.x86_64
   hawkey-0.6.3-2.fc24.x86_64
   deltarpm-3.6-15.fc24.x86_64
   npth-1.2-3.fc24.x86_64
   gpgme-1.4.3-7.fc24.x86_64
   librepo-1.7.18-2.fc24.x86_64
   rpm-build-libs-4.13.0-0.rc1.27.fc24.x86_64
   python3-dnf-1.1.9-2.fc24.noarch
   dnf-yum-1.1.9-2.fc24.noarch
   sssd-client-1.13.4-3.fc24.x86_64
   vim-minimal-7.4.1718-1.fc24.x86_64

**Further Reading:**

- `https://fedoramagazine.org/container-technologies-fedora-docker <https://fedoramagazine.org/container-technologies-fedora-docker/>`_.
- `https://docs.docker.com <https://docs.docker.com/>`_.
