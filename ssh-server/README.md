# ssh Server

Configure ssh server.

Supports two kinds of very opinionated configurations:

- Centralize user ssh public keys in /etc/ssh/keys
- Hardening configuration

For both kinds of configurations, supports a modern and a legacy way to
configure the server.

Supported operating systems:

- Fedora
- Red Hat Enterprise Linux and its clones
- SUSE Linux and openSUSE

## Role Variables

- centralize_authorized_keys - Boolean to indicate to configure /etc/ssh/keys
to centralize all users keys into one place instead of configuring them in
their home directories. When true, it requires centralized_ssh_user_keys is
also configured properly. Default is _true_.
- centralized_ssh_user_keys - List of maps containing username and list of
their public keys to add to central location. Required when
centralize_authorized_keys is set to true.
- harden - Boolean to indicate to add hardening configuration. Default is
_true_.

## Example Playbook

    - hosts: "*"
      roles:
         - role: ssh-server
           centralized_ssh_user_keys:
           - name: myusername
             public_keys:
             - "ssh-rsa _public_key_contents_"
             - "ssh-rsa _public_key_contents_"

## License

MIT
