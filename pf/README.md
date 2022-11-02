# pf

Configure pf firewall on FreeBSD and OpenBSD.

## Role Variables

- admin_range_4 - String that can be comma separated IPs that follow pf syntax.
These IPs represent who can ssh into the box.
- admin_range_6 - String that can be comma separated IPs that follow pf syntax
These IPs represent who can ssh into the box.

## Example Playbook

    - hosts: "*"
      roles:
         - role: pf

## License

MIT
