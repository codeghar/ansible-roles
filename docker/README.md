# Docker

Install and configure Docker.

It is configured to be listening to remote connections, which is insecure.

This role was developed and tested against Ubuntu 18.04.

## Role Variables

- docker_data_root - File system path where docker should be relocated.
Default value is _/var/lib/docker_.
- undo - Boolean whether to undo the changes made during install. Default value
is _false_.

## More Information

- [dockerd documentation](https://docs.docker.com/engine/reference/commandline/dockerd/#run-multiple-daemons)
- [How do I enable the remote API for dockerd](https://success.docker.com/article/how-do-i-enable-the-remote-api-for-dockerd)
- [How to move docker's default /var/lib/docker to another directory on Ubuntu/Debian Linux](https://linuxconfig.org/how-to-move-docker-s-default-var-lib-docker-to-another-directory-on-ubuntu-debian-linux)

## Example Playbook

Simple and default way to use this role,

    ---
    - hosts: "*"
      roles:
        - docker

Override the path where Docker stores its files,

    ---
    - hosts: alt
      roles:
        - role: docker
          docker_data_root: /mnt/alt/store/docker

To undo changes made in this role, set the variable _undo_ to _true_. It may not be a wholesale clean up, leaving some
bits behind. The reason is we want to be on the safe side most of the time.

    ---
    - hosts: "*"
      roles:
        - role: docker
          undo: true

## License

MIT
