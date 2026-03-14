<!--
SPDX-FileCopyrightText: 2024 Julian-Samuel Gebühr

SPDX-License-Identifier: AGPL-3.0-or-later
-->

# Versatiles Ansible Role




[VersaTiles](https://versatiles.org/) is a free stack for generating and serving vector tiles. This role helps you to set up versatiles:

- with everything run in [Docker](https://www.docker.com/) containers
- powered by [the official versatiles container image](https://github.com/versatiles-org/versatiles-docker/pkgs/container/versatiles)


## Installing

To configure and install versatiles on your own server(s), you should use a playbook like [Mother of all self-hosting](https://github.com/mother-of-all-self-hosting/mash-playbook) or write your own.

# Configuring this role for your playbook

```
versatiles_enabled: true
versatiles_hostname: 'example.org'
```

## Support

- Github issues: [moan0s/role-versatiles/issues](https://github.com/moan0s/role-versatiles/issues)
