# mini-buildd

[![Build Status](https://travis-ci.org/pari-/ansible-mini-buildd.svg?branch=master)](https://travis-ci.org/pari-/ansible-mini-buildd)

An Ansible role which installs and configures mini-buildd

<!-- toc -->

- [Requirements](#requirements)
- [Example](#example)
- [Defaults](#defaults)
- [Dependencies](#dependencies)
- [License](#license)
- [Author Information](#author-information)

<!-- tocstop -->

## Requirements

Currently this role is developed for and tested on Debian GNU/Linux (release: stretch).

Ansible version compatibility: [Dockerfile](https://github.com/pari-/docker-debian-ansible/blob/master/debian/stretch/Dockerfile)

## Example

```yaml
---

- hosts: "all"
  roles:
    - role: "ansible-mini-buildd"
      tags:
        - "mini-buildd"
  post_tasks:
    - block:
        - include: "tests/test_mini_buildd.yml"
      tags:
        - "tests"
```

## Defaults

Available variables are listed below, along with default values (see defaults/main.yml). They're generally prefixed with `mini_buildd_` (which I deliberately leave out here for better formatting).

variable | default | notes
-------- | ------- | -----
`admin_password` | `unsecureDefaultPassword` | `The 'admin'-user default password`
`cache_valid_time` | `3600` | `Update the apt cache if its older than the set value (in seconds)`
`connection_delay` | `5` | `Connection delay used in tests`
`connection_retries` | `60` | `Connection retries used in tests`
`default_release` | `{{ ansible_distribution_release\|lower }}-backports` | `The default release to install packages from`
`listen_address` | `{{ ansible_default_ipv4['address'] }}` | `The address mini-buildd listens to`
`listen_port` | `8066` | `The port mini-buildd listens to`
`package_list` | `['mini-buildd']` | `The list of packages to be installed`
`pre_default_release` | `{{ mini-buildd_default_release }}` | `The default release to install packages (pre_package_list) from`
`pre_package_list` | `['apt-transport-https','ca-certificates','haveged']` | `The list of prerequisite packages to be installed`
`repo_list` | `[]` | `Source string for the repositories`
`service_name` | `mini-buildd` | `Name of the (mini-buildd) service`
`supported_distro_list` | `['stretch']` | `A list of distribution releases this role supports`
`update_cache` | `yes` | `Run the equivalent of apt-get update before the operation`

## Dependencies

None

## License

MIT

## Author Information

* Patrick Ringl
