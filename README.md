# Ansible Role: NTP

[![CI](https://github.com/softbauware/ansible-role-ntp/workflows/playbook-run/badge.svg)](https://github.com/softbauware/ansible-role-ntp/actions/workflows/playbook-run.yml)

Installs NTP on Linux.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
ntp_enabled: true
```

Whether to start the ntpd service and enable it at system boot. On many virtual machines that run inside a container (like OpenVZ or VirtualBox), it's recommended you don't run the NTP daemon, since the host itself should be set to synchronize time for all its child VMs.

```yaml
ntp_timezone: Etc/UTC
```

Set the timezone for your server.

```yaml
ntp_package: ntp
```

The package to install which provides NTP functionality. The default is `ntp` for most platforms, or `chrony` on RHEL/CentOS 7 and later.

```yaml
ntp_daemon: [various]
```

The default NTP daemon should be correct for your distribution, but there are some cases where you may want to override the default, e.g. if you're running `ntp` on newer versions of RHEL/CentOS.

```yaml
ntp_config_file: /etc/ntp.conf
```

The path to the NTP configuration file. The default is `/etc/ntp.conf` for most platforms, or `/etc/chrony.conf` on RHEL/CentOS 7 and later.

```yaml
ntp_manage_config: false
```

Set to true to allow this role to manage the NTP configuration file (`/etc/ntp.conf`).

```yaml
ntp_driftfile: [various]
```

The default NTP driftfile should be correct for your distribution, but there are some cases where you may want to override the default.

```yaml
ntp_area: ""
```

Set the [NTP Pool Area](http://support.ntp.org/bin/view/Servers/NTPPoolServers) to use. Defaults to none, which uses the worldwide pool.

```yaml
ntp_servers:
  - "0{{ '.' + ntp_area if ntp_area else '' }}.pool.ntp.org iburst"
  - "1{{ '.' + ntp_area if ntp_area else '' }}.pool.ntp.org iburst"
  - "2{{ '.' + ntp_area if ntp_area else '' }}.pool.ntp.org iburst"
  - "3{{ '.' + ntp_area if ntp_area else '' }}.pool.ntp.org iburst"
```

Sets the keyword used for servers in the NTP configuration file. The keyword pool is a good choice when DNS resolve is an option, if not you should use server instead.

```yaml
ntp_server_keyword: "pool"
```

Specify the NTP servers you'd like to use. Only takes effect if you allow this role to manage NTP's configuration, by setting `ntp_manage_config` to `True`.

```yaml
ntp_restrict:
  - "127.0.0.1"
  - "::1"
```

Restrict NTP access to these hosts; loopback only, by default.

```yaml
ntp_cron_handler_enabled: false
```

Whether to restart the cron daemon after the timezone has changed.

```yaml
ntp_tinker_panic: true
```

Enable tinker panic, which is useful when running NTP in a VM.

## Dependencies

None.

## Example Playbook

```yaml
- hosts: all
  roles:
    - softbauware.ntp
```

_Inside `vars/main.yml`_:

```yaml
ntp_timezone: America/Chicago
```

## License

MIT / BSD

## Author Information

This role was created in 2014 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
