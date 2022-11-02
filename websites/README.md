# websites

Push website's content to /\[var,srv\]/www/DOMAIN_NAME and fix permissions of
all files and directories to be owned by OS-dependent nginx user and group.

I broke the nginx work into three roles to make them smaller and more targeted.

This role was developed and tested against FreeBSD 13.1-RELEASE and clones of
Red Hat Enterprise Linux 9.

Supported operating systems:

- FreeBSD
- Red Hat Enterprise Linux and its clones

## Role Variables

- contents_path - String that contains the path where your website's content
is stored. Default value is _/path/to/content_ and **must** be changed in the
playbook.
- domain_name - String that contains the domain name. Default value is
_CHANGEME.com_ and **must** be changed in the playbook.

## Dependencies

- certbot - From https://github.com/codeghar/ansible-roles/tree/master/certbot
- nginx - From https://github.com/codeghar/ansible-roles/tree/master/nginx
- nginx-site-config - From https://github.com/codeghar/ansible-roles/tree/master/nginx-site-config

## Example Playbook

    - hosts: "*"
      roles:
        - role: websites
          contents_path: /home/user/example.com
          domain_name: example.com

## License

MIT
