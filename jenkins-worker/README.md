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

- undo - Boolean whether to undo the changes made during install. Default value
is _false_.

## Example Playbook

    - hosts: '*'
      roles:
         - "jenkins-worker"

License
-------

MIT
