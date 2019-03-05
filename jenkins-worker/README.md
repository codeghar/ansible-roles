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

    - hosts: '*'
      roles:
         - "jenkins-worker"

License
-------

MIT
