# Red Hat Enterprise Linux

Call other roles

Supported operating systems:

- Red Hat Enterprise Linux and its clones

More information,

- https://www.freebsd.org/doc/handbook/configtuning-syslog.html
- https://www.freebsd.org/cgi/man.cgi?query=newsyslog.conf&sektion=5&manpath=freebsd-release-ports
- https://www.guyrutenberg.com/2017/01/01/lets-encrypt-reload-nginx-after-renewing-certificates/
- https://certbot.eff.org/docs/using.html#renewing-certificates

## Role Variables

- cron_hour - String to indicate at what hour to run certbot renew. Default is
_14_.
- cron_minute - String to indicate at what minute to run certbot renew. Default
is _14_.
- cron_weekday - String to indicate on what day of the week to run certbot
renew. Default is _Tue,Fri_ to avoid running into Let's Encrypt rate limiting.
- nginx - Boolean which indicates that all tasks to support nginx in the
environment must be run. Default is _true_.
- production - Boolean which indicates that the configuration applied is for a
production instance or for testing/sandbox purposes. Default is _true_. It is
used on Red Hat Enterprise Linux (RHEL, EL) only.

## Example Playbook

    - hosts: "*"
      roles:
         - role: certbot

## License

MIT
