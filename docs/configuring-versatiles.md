<!--
SPDX-FileCopyrightText: 2020 Aaron Raimist
SPDX-FileCopyrightText: 2020 Chris van Dijk
SPDX-FileCopyrightText: 2020 Dominik Zajac
SPDX-FileCopyrightText: 2020 Mickaël Cornière
SPDX-FileCopyrightText: 2020-2024 MDAD project contributors
SPDX-FileCopyrightText: 2020-2024 Slavi Pantaleev
SPDX-FileCopyrightText: 2022 François Darveau
SPDX-FileCopyrightText: 2022 Julian Foad
SPDX-FileCopyrightText: 2022 Warren Bailey
SPDX-FileCopyrightText: 2023 Antonis Christofides
SPDX-FileCopyrightText: 2023 Felix Stupp
SPDX-FileCopyrightText: 2023 Pierre 'McFly' Marty
SPDX-FileCopyrightText: 2024-2026 Suguru Hirahara

SPDX-License-Identifier: AGPL-3.0-or-later
-->

# Setting up VersaTiles

This is an [Ansible](https://www.ansible.com/) role which installs [VersaTiles server](https://github.com/versatiles-org/versatiles-rs) bundled with [the developer front‑end](https://github.com/versatiles-org/versatiles-frontend) to run as a [Docker](https://www.docker.com/) container wrapped in a systemd service.

[VersaTiles](https://versatiles.org/) is a free stack for generating, distributing, and using map tiles based on OpenStreetMap data.

See the project's [documentation](https://docs.versatiles.org/) to learn what VersaTiles does and why it might be useful to you.

## Prerequisites

To run a VersaTiles server bundled with the developer front‑end it is necessary to prepare a built tile data file (`*.versatiles`). Refer to [this page](https://docs.versatiles.org/guides/converter.html) on the documentation for details about VersaTiles data converter.

## Adjusting the playbook configuration

To enable VersaTiles with this role, add the following configuration to your `vars.yml` file.

**Note**: the path should be something like `inventory/host_vars/mash.example.com/vars.yml` if you use the [MASH Ansible playbook](https://github.com/mother-of-all-self-hosting/mash-playbook).

```yaml
########################################################################
#                                                                      #
# versatiles                                                           #
#                                                                      #
########################################################################

versatiles_enabled: true

########################################################################
#                                                                      #
# /versatiles                                                          #
#                                                                      #
########################################################################
```

### Set the hostname

To enable VersaTiles you need to set the hostname as well. To do so, add the following configuration to your `vars.yml` file. Make sure to replace `example.com` with your own value.

```yaml
versatiles_hostname: "example.com"
```

After adjusting the hostname, make sure to adjust your DNS records to point the domain to your server.

**Note**: hosting VersaTiles under a subpath (by configuring the `versatiles_path_prefix` variable) does not seem to be possible due to VersaTiles's technical limitations.

### Specify built tile data file URL

It is also necessary to set `.versatiles` built tile data file URL by adding the following configuration to your `vars.yml` file:

```yaml
versatiles_built_tile_data_url: SET_URL_HERE
```

>[!NOTE]
> **Pre-built VersaTiles dataset cannot be directly served.** You might wish to use [the container image which includes the VersaTiles binary](https://github.com/versatiles-org/versatiles-docker/blob/main/versatiles/README.md) to download and crop map data.

### Extending the configuration

There are some additional things you may wish to configure about the service.

Take a look at:

- [`defaults/main.yml`](../defaults/main.yml) for some variables that you can customize via your `vars.yml` file. You can override settings (even those that don't have dedicated playbook variables) using the `versatiles_environment_variables_additional_variables` variable

## Installing

After configuring the playbook, run the installation command of your playbook as below:

```sh
ansible-playbook -i inventory/hosts setup.yml --tags=setup-all,start
```

If you use the MASH playbook, the shortcut commands with the [`just` program](https://github.com/mother-of-all-self-hosting/mash-playbook/blob/main/docs/just.md) are also available: `just install-all` or `just setup-all`

## Usage

After running the command for installation, VersaTiles becomes available at the specified hostname like `https://example.com`.

## Troubleshooting

### Check the service's logs

You can find the logs in [systemd-journald](https://www.freedesktop.org/software/systemd/man/systemd-journald.service.html) by logging in to the server with SSH and running `journalctl -fu versatiles` (or how you/your playbook named the service, e.g. `mash-versatiles`).
