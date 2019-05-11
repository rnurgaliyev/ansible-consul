# Ansible role for HashiCorp Consul

This is a simple yet very flexible role to install and configure [HashiCorp Consul](https://consul.io).

Major pros of this role:
* Simplicity
* Automatic detection of latest available Consul version
* Flexible configuration

## Example usage

```yaml
---

# consul_install controls if role should try to install consul.
# consul_install_version controls which version should be installed, and
# can be set to 'latest'. In this case, role will fetch latest consul version
# according to hashicorp software versioning REST API.
consul_install: True
consul_install_version: latest

# consul_configure controls if consul should be configured.
consul_configure: True

# consul_configuration is a list of consul configurations. Every item
# will be deployed to consul configuration directory as a separate json file.
# Content of configuration key will be translated to json.

consul_configuration:
  - name: global
    configuration:
      bind_addr: "{{ ansible_default_ipv4.address }}"
      client_addr: "127.0.0.1 {{ ansible_default_ipv4.address }}"
      data_dir: "/var/consul"
      encrypt: "0rhi/Tz9l1mZIk7f8l5PVQ=="
      ui: True

  - name: server
    configuration:
      bootstrap_expect: 3
      server: True
      retry_join: "{{ groups['consul-server'] }}"
```
