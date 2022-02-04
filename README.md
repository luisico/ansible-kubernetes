Kubernetes
==========

Bootstrap kubernetes cluster.

This role installs kubernetes cluster in high availability. The role needs to be called twice from a playbook, first to bootstrap the cluster on one manager node, and then to join all other nodes (managers and workers).

This role is only compatible with kubernetes and calico minor versions defined in `defaults/main.yml`. Other version combinations might work, but are not supported.

Requirements
------------
See `meta/main.yml`.

Role Variables
--------------
See `defaults/main.yml`.

Dependencies
------------

Docker engine needs to be installed, ie using role `luisico.docker`. This role does not manage the firewall.

Example Playbook
----------------
The following example bootstrap a single kubernetes cluster:

```
- name: Init kubernetes cluster
  hosts: managers[0]
  roles:
    - role: kubernetes
      kubernetes_task: init

- name: Join kubernetes cluster
  hosts: all:!managers[0]
  roles:
    - role: kubernetes
      kubernetes_task: join
      kubernetes_leader: '{{ groups["managers"][0] }}'
```

TODO
----

- Support single master (non-HA) apiserver_vip not needed?
- Autogenerate kubernetes_certificate_key if not present
- Autogenerate kubernetes_keepalived_pass if not present
- Restart kubelet if keepalive/haproxy configs change?
- Having token and hash in kubeadm-join.yaml config might not be safe
- Revisit kubeadm preflight errors to ignore in vagrant vs production

License
-------
Released under the [MIT license](https://opensource.org/licenses/MIT).

Author Information
------------------
Luis Gracia while at [The Rockefeller University](https://www.rockefeller.edu):
- lgracia [at] rockefeller.edu
- GitHub at [luisico](https://github.com/luisico)
- Galaxy at [luisico](https://galaxy.ansible.com/luisico)

Garth Kerr while at [Rockefeller University](http://www.rockefeller.edu):
- GitHub at [garthkerr](https://github.com/garthkerr)
