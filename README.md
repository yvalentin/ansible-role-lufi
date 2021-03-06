# Ansible Role for Lufi (Let's Upload That File)

[![Build Status][travis-build-status]][travis-tests] [![Ansible Role][ansible-role-shield]][ansible-role]

---

:rocket: Development has moved to **[git.feneas.org]**.

(The repository on GitHub is only a mirror, so fork on Feneas to contribute. No registration needed, just sign in with your GitHub account.)

---

This role installs and configures Lufi on Debian/Ubuntu servers.
Find out more about [Lufi], created by [Luc Didry].

This role will automatically install a service that will start when the server boots up.
It will figure out which service manager is being used automatically too.

## Requirements

Using this role doesn't install Nginx or Apache as a reverse proxy, you need to do that yourself!
Take a look at the [example configurations].

## Role Variables

Set the user/group that will be used to run Lufi. It makes sense to use the webserver user/group.

```
lufi_user: www-data
lufi_group: www-data
```

Set if Lufi should be kept up to date. (default: no)

```
lufi_keep_updated: no
```

There are a few mandatory and many optional values. Check all possible variables in [`defaults/main.yml`][defaults].

```
# Required!
lufi_working_dir: "/var/www/example.com"
lufi_listen: "http://127.0.0.1:8080"    # Or an array, if multiple addresses.
lufi_contact: "admin@example.com"
lufi_secrets: ["array", "of", "random", "secrets"]

# Optional
lufi_theme: "default"
lufi_proxy: no
lufi_workers: 30
lufi_clients: 1
lufi_url_length: 8
lufi_provis_step: 5
lufi_provisioning: 100
lufi_token_length: 32
lufi_max_file_size: 104857600
lufi_piwik_img: ""
lufi_broadcast_message: ""
lufi_default_delay: 0
lufi_max_delay: 0
lufi_delay_for_size:
    10000000: 90   # between 10MB and 50MB => max is 90 days, less than 10MB => max is max_delay (see above)
    50000000: 60   # between 50MB ans 1GB  => max is 60 days
    1000000000: 2  # more than 1GB         => max is 2 days
lufi_prefix: "/"
lufi_allowed_domains: []
lufi_fixed_domain: ""
lufi_mail:
    how: "smtp"
    howargs: ["smtp.example.org"]
lufi_mail_sender: "no-reply@lufi.io"
lufi_db_type: "sqlite"
lufi_db_path: "lufi.db"
lufi_pgdb:
    database: "lufi"
    host: "localhost"
    user: "DBUSER"
    pwd: "DBPASSWORD"
lufi_upload_dir: "files"
lufi_ldap:
    uri: "ldaps://ldap.example.org"
    user_tree: "ou=users,dc=example,dc=org"
    bind_dn: ",ou=users,dc=example,dc=org"
    bind_user: "uid=ldap_user"
    bind_pwd: "secr3t"
    user_filter: "!(uid=ldap_user)"
lufi_htpasswd: "lufi.passwd"
lufi_session_duration: 3600
lufi_allow_pwd_on_files: no
lufi_keep_ip_during: 365
lufi_max_total_size: 10*1024*1024*1024
lufi_policy_when_full: "warn"
lufi_delete_no_longer_viewed_files: 90
```

## Role Tags

Each part of the setup has a tag.

```
lufi:install
lufi:site
lufi:service
```

## Dependencies

None.

## Example Playbook

```
# playbook.yml
---
- hosts: servers
  become: yes
  vars_files:
    - vars/main.yml
  roles:
    - { role: noplanman.lufi }
```
```
# vars/main.yml
---
lufi_working_dir: "/var/www/lufi.example.com"
lufi_listen: "http://127.0.0.1:8080"
lufi_contact: "admin@lufi.example.com"
lufi_secrets: ["xud7ooJu","aiNg7duG","ih7kom8Z","Ocaish3I","Ooja7chi","Eet4weil","Ethee4Go","xahJ0ohy"]
lufi_broadcast_message: "Welcome to Lufi. Upload those files!"
```

## Tests

Docker is used to test the role with different operating systems.

Check the [`tests`] folder.

## License

MIT

[travis-build-status]: https://img.shields.io/travis/noplanman/ansible-role-lufi.svg?style=flat-square "Travis-CI Build Status"
[travis-tests]: https://travis-ci.org/noplanman/ansible-role-lufi "Travis-CI Tests"
[ansible-role-shield]: https://img.shields.io/ansible/role/12067.svg?style=flat-square "Lufi on Ansible Galaxy"
[ansible-role]: https://galaxy.ansible.com/noplanman/lufi "Lufi on Ansible Galaxy"
[git.feneas.org]: https://git.feneas.org/noplanman/ansible-role-lufi "Ansible Role Lufi on Feneas"
[Lufi]: https://framagit.org/luc/lufi "Lufi on Framagit"
[Luc Didry]: https://framagit.org/u/luc "Luc on Framagit"
[example configurations]: https://framagit.org/luc/lufi/wikis/installation#reverse-proxies "Example configurations"
[defaults]: https://git.feneas.org/noplanman/ansible-role-lufi/blob/master/defaults/main.yml "Default variables"
[`tests`]: https://git.feneas.org/noplanman/ansible-role-lufi/tree/master/tests "Tests"
