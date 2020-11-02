# ansible-lihas-proxmox
Installs Proxmox PVE6

added Features:

* wireguard Module preloaded to enable wireguard in LXCs

## Requirements

To run solo:
```
# make sure the FQDN points to the IP you want on the (future) proxmox host

ansible-galaxy install -r requirements.yml
ansible-playbook -i localhost, proxmox.yml
```

## Dependencies

* lihas_common

## Example Playbook

```
---
- hosts: '*'
  role: lihas_proxmox
...
```

# Configuration
Variables:
```
forward_ipv6: [0|1]
forward_ipv4: [0|1]
%:
  config:
    hosts:
      "MY.MANAGE.MENT.IP":
         - pve.example.com
         - pve
roles:
  proxmox:
    templates:
      debian_latest: local:vztmpl/debian-10.0-standard_10.0-1_amd64.tar.gz
    lxc:
      100:
        cmode: shell
        hostname: test
        memory: 2048
        swap: 512
        rootfs: "zp_pve:2"
        onboot: 1
        unprivileged: 1
        features:
        net:
          - "name=eth0,bridge=vmbr0,firewall=0,ip=192.0.2.100/24,gw=192.0.2.1,type=veth"
        start: 1
        mp:
          - "ceph:60,mp=/srv,mountoptions=noatime"
        cluster: proxmox
```
Variables are mostly the `pct` parameters, additionally:

* cluster
  * proxmox: use ha-manager
  * application: non-proxmox application cluster
* hagroup: ha-manager group name
