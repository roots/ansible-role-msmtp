# Ansible Role: msmtp

<a href="https://github.com/roots/ansible-role-msmtp/releases"><img alt="Release" src="https://img.shields.io/github/release/roots/ansible-role-msmtp.svg?style=flat-square" /></a>
<a href="https://github.com/roots/ansible-role-msmtp/actions"><img alt="Build Status" src="https://img.shields.io/github/actions/workflow/status/roots/ansible-role-msmtp/ci.yml?branch=main&style=flat-square" /></a>
<a href="https://galaxy.ansible.com/roots/msmtp"><img alt="Galaxy Downloads" src="https://img.shields.io/badge/dynamic/json?color=blueviolet&label=galaxy%20downloads&query=%24.download_count&url=https%3A%2F%2Fgalaxy.ansible.com%2Fapi%2Fv1%2Froles%2F%3Fname%3Dmsmtp%26namespace%3Droots&style=flat-square" /></a>
<a href="https://twitter.com/rootswp"><img alt="Follow Roots" src="https://img.shields.io/badge/follow%20@rootswp-1da1f2?logo=twitter&logoColor=ffffff&message=&style=flat-square"></a>
<a href="https://github.com/sponsors/roots"><img src="https://img.shields.io/badge/sponsor%20roots-525ddc?logo=github&style=flat-square&logoColor=ffffff&message=" alt="Sponsor Roots"></a>

Installs and configures [msmtp](https://marlam.de/msmtp/), a lightweight SMTP client, on Debian-based Linux systems. msmtp is a sendmail-compatible SMTP client that can be used to relay mail through an external SMTP server.

The `msmtp-mta` package provides a `/usr/sbin/sendmail` symlink, so services like fail2ban and cron continue to work without changes.

If you're using PHP and would like to route all PHP email through msmtp, you will need to update the `sendmail_path` configuration option in php.ini, like so:

```yaml
sendmail_path = "/usr/bin/msmtp -t"
```

## Support us

Roots is an independent open source org, supported only by developers like you. Your sponsorship funds [WP Packages](https://wp-packages.org/) and the entire Roots ecosystem, and keeps them independent. Support us by purchasing [Radicle](https://roots.io/radicle/) or [sponsoring us on GitHub](https://github.com/sponsors/roots) — sponsors get access to our private Discord.

## Requirements

A Debian-based (eg: Ubuntu) system.

## Role Variables

This role is designed for [Trellis](https://github.com/roots/trellis) and defaults to using Trellis's `mail_*` variables. When used outside of Trellis, override the variables below.

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
msmtp_server: "{{ mail_smtp_server }}"
msmtp_from: "{{ mail_admin }}"
msmtp_domain: "{{ mail_hostname }}"
msmtp_user: "{{ mail_user }}"
msmtp_password: "{{ mail_password }}"
```

The SMTP server (host:port format, supports IPv6 with brackets like `[::1]:587`), envelope sender, EHLO domain, and credentials. If no port is specified in `msmtp_server`, the `msmtp_port` default (587) is used.

For non-Trellis usage, set these directly:

```yaml
msmtp_server: smtp.example.com:587
msmtp_from: admin@example.com
msmtp_domain: example.com
msmtp_user: smtp_user
msmtp_password: secret
```

```yaml
msmtp_auth: "on"
```

SMTP authentication. Set to `"off"` to disable.

```yaml
msmtp_tls: "on"
msmtp_tls_starttls: "on"
```

TLS settings.

```yaml
msmtp_accounts: []
```

Optional list of additional accounts. Each account supports: `name`, `host`, `port`, `from`, `user`, `password`, `auth`, `tls`, `tls_starttls`, and `domain`. See `defaults/main.yml` for an example.

## Example Playbook

With [Trellis](https://github.com/roots/trellis) (uses `mail_*` variables automatically):

```yaml
- hosts: servers
  roles:
    - { role: roots.msmtp }
```

Standalone:

```yaml
- hosts: servers
  roles:
    - role: roots.msmtp
      vars:
        msmtp_server: smtp.example.com:587
        msmtp_from: admin@example.com
        msmtp_domain: example.com
        msmtp_user: smtp_user
        msmtp_password: "{{ vault_smtp_password }}"
```

## Community

Keep track of development and community news.

- Join us on Discord by [sponsoring us on GitHub](https://github.com/sponsors/roots)
- Join us on [Roots Discourse](https://discourse.roots.io/)
- Follow [@rootswp on Twitter](https://twitter.com/rootswp)
- Follow the [Roots Blog](https://roots.io/blog/)
- Subscribe to the [Roots Newsletter](https://roots.io/subscribe/)
