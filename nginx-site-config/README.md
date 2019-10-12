# nginx-site-config

Configure one site in nginx. Supports TLS, Let's Encrypt, redirection, and many
other features.

One salient feature is the ability to configure the site without TLS if a Let's
Encrypt certificate is not present. It attempts to create a certificate with
`certbot` and when successful it configures the site with TLS.

I broke the nginx work into three roles to make then smaller and more targeted.
This role was developed and tested against FreeBSD 12.0-RELEASE.

Support operating systems:

- FreeBSD

More information,

- https://mozilla.github.io/server-side-tls/ssl-config-generator
- https://ssl-config.mozilla.org/#server=nginx&server-version=1.16.1&config=intermediate&openssl-version=1.1.1a&hsts=false&ocsp=false

## Role Variables

- admin_email - String that contains the administrator's email. This is used
by `certbot` to create Let's Encrypt certificates. Default value is _CHANGEME_
and **must** be changed in the playbook.
- autoindex - Boolean on whether to turn on or turn off directory listing.
Default value is _false_.
- default_file_type: String to set the default file type in nginx. Default
value is _text/html_.
- domain_name - String that contains the domain name. For example,
codeghar.com. Default value is _CHANGEME.com_ and **must** be changed in the
playbook.
- index_pages - String that contains a space separated list of pages to
consider as an
[index page](https://docs.nginx.com/nginx/admin-guide/web-server/serving-static-content/#root).
- redirect_domain - Optional string to use in nginx site config file when http
must be redirected to https. Default value is an empty string.
- web_root - String that contains the full path of where the website contents
are stored. This is used by `certbot`. Default value is _/var/www/CHANGEME.com_
and **must** be changed in the playbook.

## Dependencies

- certbot - From this repo
- nginx - From this repo

## Example Playbook

    - hosts: "*"
      roles:
        - {
            role: nginx-conf,
            admin_email: something@example.com,
            domain_name: example.com,
            web_root: /var/www/example.com,
        }

License
-------

MIT
