# Ansible Role: Caddy

[![Project Status](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)
[![Ansible Galaxy](https://img.shields.io/ansible/role/57668)](https://galaxy.ansible.com/obvionaoe/caddy)

An Ansible role to set up a [Caddy](https://caddyserver.com/) server running in Docker with the [Docker Proxy](https://github.com/lucaslorentz/caddy-docker-proxy) plugin installed.

## Requirements

- [Docker](https://docker.io)

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
timezone: Etc/UTC
```

The timezone the container should be in. This variable does not include the `caddy` prefix so that all roles that support this feature can be configured through a single variable.

```yaml
caddy_timezone: "{{ timezone }}"
```

The timezone the container should be in. This variable is by default set to the value passed to the `timezone` variable and is used only when, for some reason, the timezone to be passed to Caddy's container should not be the one that the `timezone` variable is set to.

```yaml
caddy_host_user: caddy
caddy_host_group: caddy
```

The user and group to use when creating the host directories.

```yaml
caddy_host_dir: /caddy
caddy_host_data_dir: "{{ caddy_host_dir }}/data"
```

The root host directory and the data directory that will be mounted onto the container.

```yaml
caddy_image_tag: '2.8'
caddy_image_name: 'lucaslorentz/caddy-docker-proxy'
caddy_image: "{{ caddy_image_name }}:{{ caddy_image_tag }}"
```

Variables to control which image will be used when running the container, in case you build your own image.

```yaml
caddy_container_name: caddy
```

The name to give to the container.

```yaml
caddy_container_restart_policy: unless-stopped
caddy_container_restart_retries: 5
```

Variables to control the state of the container, and the restart policy.

```yaml
caddy_container_volumes:
  - /var/run/docker.sock:/var/run/docker.sock:ro
  - "{{ caddy_host_data_dir }}:/data"
```

Variable to specify which host directories should be mounted to the container.

```yaml
caddy_container_ports:
  - 80:80
  - 443:443
```

Variable to specify which container ports to publish.

```yaml
caddy_create_networks: true
caddy_container_networks:
  - name: caddy
```

Variables to specify what Docker networks to add the container to, and if these networks should be created.

```yaml
caddy_container_labels: {}
```

Variable to define labels to give to the container. These labels are used to configure Caddy. A comprehensive guide on how to use the labels can be found [here](https://github.com/lucaslorentz/caddy-docker-proxy#caddy-cli).

```yaml
caddy_container_env_vars: {}
```

Variable to define environment variables to pass to the container. A full list of the available options can be found [here](https://github.com/lucaslorentz/caddy-docker-proxy#caddy-cli). 

## Dependencies

None.

## Example Playbook

```yaml
- hosts: all
  roles:
    - obvionaoe.caddy
```

## License

MIT


## Author Information

This role was created in 2022 by [Luís Guimarâes](https://obvionaoe.dev).
