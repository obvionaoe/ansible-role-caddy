# Ansible Role: Caddy

<!-- [![CI](https://github.com/obvionaoe/ansible-role-caddy/workflows/CI/badge.svg?event=push)](https://github.com/obvionaoe/ansible-role-caddy/actions?query=workflow%3ACI) -->

An Ansible role to set up a [Caddy](https://caddyserver.com/) server running in Docker with the [Docker Proxy](https://github.com/lucaslorentz/caddy-docker-proxy) plugin installed.

## Requirements

Docker.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    caddy_host_user: caddy
    caddy_host_group: caddy

The `user` and `group` to use when creating the host directories.

    caddy_host_dir: /caddy
    caddy_host_data_dir: "{{ caddy_host_dir }}/data"

The root host directory and the data directory that will be mounted onto the container.

    caddy_image_tag: '2.4'
    caddy_image_name: 'lucaslorentz/caddy-docker-proxy'
    caddy_image: "{{ caddy_image_name }}:{{ caddy_image_tag }}"

Variables to control which image will be used when running the container, in case you build your own image.

    caddy_container_name: caddy

The name to give to the container.

    caddy_container_state: started
    caddy_container_restart_handler_state: restarted
    caddy_container_restart_policy: unless-stopped
    caddy_container_restart_retries: 5

Variables to control the `state` of the container, and the `restart_policy`.

    caddy_container_volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - "{{ caddy_host_data_dir }}:/data"

Variable to specify which host directories should be mounted to the container.

    caddy_container_ports:
      - 80:80
      - 443:443

Variable to specify which `container ports` to publish.

    caddy_create_networks: true
    caddy_container_networks:
      - name: caddy

Variables to specify what Docker `networks` to add the container to, and if these `networks` should be created.

    caddy_container_labels: {}

Variable to define labels to give to the container. These labels are used to configure Caddy. A comprehensive guide on how to use the labels can be found [here](https://github.com/lucaslorentz/caddy-docker-proxy#caddy-cli).

    caddy_container_env_vars:
      TZ: Europe/Lisbon
      CADDY_INGRESS_NETWORKS: '"{{ caddy_container_networks[0].name }}"'

Variable to define environment variables to pass to the container. A full list of the available options can be found [here](https://github.com/lucaslorentz/caddy-docker-proxy#caddy-cli).

## Dependencies

None.

## Example Playbook

```yaml
- hosts: all
  roles:
    - name: obvionaoe.caddy
      caddy_container_labels:
        caddy: www.example.com
        caddy.redir: https://example.com{uri}
```

## License

MIT


## Author Information

This role was created in 2022 by [Luís Guimarâes](https://obvionaoe.xyz).
