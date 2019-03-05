# Jenkins Worker

Prepare a worker node to connect with Jenkins.

A worker requires Java to be installed. I use SDKMAN to manage the JRE
installed on the worker.

- https://sdkman.io/
- https://github.com/Comcast/ansible-sdkman/

This role was developed and tested against Ubuntu 18.04.

## Requirements

Copy the ssh public key to _files_ directory with the name _key.pub_. This key
will be added to ssh authorized keys on the target box.

## Role Variables

- docker_installed - Boolean whether Docker is expected to be installed already. Adds the ssh user to _docker_ group when true. Default is _false_, to give more control to admins to decide whether they want to do this.
- java_version - Version of Java to install. Default is _8.0.202-zulu_. Run ``. /usr/local/sdkman/bin/sdkman-init.sh && sdk list java`` on the target node to get a list of other versions. Jenkins 2.150.3 requires version 8.
- password - Initial [crypted](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module) password of the user. Default value is _IamInsecure_, which is not crypted on purpose.
- undo - Boolean whether to undo the changes made during install. Default value
is _false_.
- username - User to create for Jenkins leader to ssh as. Default value is
_jenkins_.

## Example Playbook

Run these two required steps first:

- Create ssh keys
- Generate crypted password

Create ssh keys,

    $ ssh-keygen -t rsa -b 4096 -f files/key

Generate a [crypted password](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module),

    $ python3 -m pip install --user passlib
    $ python3 -c "from passlib.hash import sha512_crypt; import getpass; print(sha512_crypt.using(rounds=5000).hash(getpass.getpass()))"
    Password:
    $6$ebcDFuwHyR/D/SRP$r3nVDhepbjLEKkSkQxi2gDApd9Yitj3XRm1cUdTf88V0DIZCHnf22HRorSDund7xUlDeAXX8MJECDjSZ4ZOCD1

Now pass this password to the role,

    - hosts: "*"
      roles:
         - role: jenkins-worker
           password: "$6$ebcDFuwHyR/D/SRP$r3nVDhepbjLEKkSkQxi2gDApd9Yitj3XRm1cUdTf88V0DIZCHnf22HRorSDund7xUlDeAXX8MJECDjSZ4ZOCD1"

Another example where we want to use the _docker_ role (from this repo) as well,

    - hosts: "*"
      roles:
         - docker
         - role: jenkins-worker
           docker_installed: true
           password: "$6$ebcDFuwHyR/D/SRP$r3nVDhepbjLEKkSkQxi2gDApd9Yitj3XRm1cUdTf88V0DIZCHnf22HRorSDund7xUlDeAXX8MJECDjSZ4ZOCD1"

To undo changes made in this role, set the variable _undo_ to _true_. It may not be a wholesale clean up, leaving some
bits behind. The reason is we want to be on the safe side most of the time.

    ---
    - hosts: "*"
      roles:
        - role: jenkins-worker
          undo: true

License
-------

MIT
