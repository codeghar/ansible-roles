# websites

Push website's content to /var/www/DOMAIN_NAME and fix permissions of all files
and directories to be owned by www:www.

I broke the nginx work into three roles to make then smaller and more targeted.
This role was developed and tested against FreeBSD 12.0-RELEASE.

Supported operating systems:

- FreeBSD

## Role Variables

- contents_path - String that contains the path where your website's content
is stored. Default value is _/path/to/content_ and **must** be changed in the
playbook.
- domain_name - String that contains the domain name. Default value is
_CHANGEME.com_ and **must** be changed in the playbook.

## Dependencies

- certbot - From this repo
- nginx - From this repo
- nginx-site-config - From this repo

## Example Playbook

    - hosts: "*"
      roles:
        - role: websites
          contents_path: /home/user/example.com
          domain_name: example.com

License
-------

MIT
