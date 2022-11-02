# firewalld

Install and configure firewalld.

Configures:

- Cockpit
- http and https
- Prometheus
- ssh

Supported operating systems:

- Red Hat Enterprise Linux and its clones
- SUSE Linux and openSUSE

## Role Variables

- allow_web_ports - Boolean to indicate whether to open http and https ports.
Default is _false_.
- firewalld_running - Boolean to indicate whether firewalld is running or not.
It is updated during execution of the role. Default is _false_.
- firewalld_command_prefix - String containing the command name to use for
firewalld since it has two commands depending on whether it is enabled+running
or not. Default is _firewall-offline-cmd_ to match default value of
firewalld_running.
- harden - Boolean to indicate whether to harden access, especially for ssh.
Default is _false_.
- trusted_source_ips - List of strings containing trusted IPv4 and/or IPv6 IPs
or subnets. Default is empty list.

## Example Playbook

    - hosts: "*"
      roles:
         - role: firewalld

## License

MIT
