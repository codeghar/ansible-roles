# EPEL

Install certbot and configure it to auto-renew twice a week.

Optionally supports reloading nginx with a renewal hook.

Supported operating systems:

- CentOS Stream
- Red Hat Enterprise Linux and its clones

## Example Playbook

    - hosts: "*"
      roles:
         - role: epel

## License

MIT
