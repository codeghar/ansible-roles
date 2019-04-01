# Unifi Controller

This role was developed and tested against Ubuntu 18.04 on Raspberry Pi 3 Model
B+. It should also work with Ubuntu 16.04 even though I have not fully tested
it. Similarly, it should work with AMD64 family of processors.

On Ubuntu 18.04, Unifi controller does not install because it has newer version
of MongoDB (3.6) than what Unifi expects (3.4). A
[workaround was proposed](https://community.ubnt.com/t5/UniFi-Feature-Requests/Support-Ubuntu-18-04-and-others-including-MongoDB-3-6-x/idc-p/2474562/highlight/true#M16164)
which I use in this role.

Issues I ran into when installing on Raspberry Pi,

- On Raspberry Pi 2 Model B+ (armv7l), can't install MongoDB on Ubuntu 18.04
because MongoDB does not support 32-bit.
- On Raspberry Pi 3 Model B+ (aarch64), use Ubuntu 18.04 64-bit so MongoDB can
be installed.
- On Raspberry Pi 3 Model B+ (aarch64), could not install Unifi controller from
repo. Ansible errored out with _No package matching 'unifi' is available_. The
workaround I used was to download the deb package and install it from local
file system.

More information,

- https://help.ubnt.com/hc/en-us/articles/220066768-UniFi-How-to-Install-Update-via-APT-on-Debian-or-Ubuntu
- https://www.technologist.site/2016/06/02/how-to-install-ubiquiti-unifi-controller-5-on-the-raspberry-pi/3/
- https://lazyadmin.nl/it/installing-unifi-controller-on-a-raspberry-pi-in-5-min/
- https://docs.mongodb.com/v3.4/tutorial/install-mongodb-on-ubuntu/
- https://stackoverflow.com/a/28070759


## Role Variables

- mongodb_apt_key_id - String to contain the GPG key ID used by Mongo to sign
its packages. This may need to be updated periodically and the information
should be available on MongoDB's website.
- undo - Boolean whether to undo the changes made during install. Default value
is _false_.
- unifi_apt_key_id - String to contain the GPG key ID used by Ubnt to sign its
packages. This may need to be updated periodically and the information should
be available on Unifi's website.
- unifi_version - String to contain the version of Unifi to download when
running on Ubuntu 18.04. This needs to be updated periodically by getting the
information from https://www.ubnt.com/download/unifi.

## Example Playbook

    - hosts: "*"
      roles:
         - role: unifi

To undo changes made in this role, set the variable _undo_ to _true_. It may not be a wholesale clean up, leaving some
bits behind. The reason is we want to be on the safe side most of the time.

    ---
    - hosts: "*"
      roles:
        - role: unifi
          undo: true

License
-------

MIT
