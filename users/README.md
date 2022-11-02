# Users

Create user. Only admin users are supported.

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

- admin_password - String containing the plaintext password string for user.
Default is _null_.
- admin_user - String containing the username of the admin user. Default is
_first_.
- password_salt: String containing the salt to use in the password. Default is
_null_.
- suse_prefer_yast: Boolean indicating whether to use yast or Ansible's user
module to create the user. Default is _no_.


## Example Playbook

    - hosts: "*"
      roles:
         - role: users
           admin_password: myInsecurePassword
           admin_user: myadminuser
           password_salt: myInsecureSalt

## License

MIT
