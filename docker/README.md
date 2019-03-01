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

    ---
    - hosts: "*"
      roles:
        - docker

    ---
    - hosts: alt
      roles:
        - docker
      vars:
        new_docker_path: /mnt/alt/store/docker

## License

MIT
