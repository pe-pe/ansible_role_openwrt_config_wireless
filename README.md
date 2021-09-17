OpenWRT Config Wireless
=========

[![CI](https://github.com/pe-pe/ansible_role_openwrt_config_wireless/workflows/CI/badge.svg)](https://github.com/pe-pe/ansible_role_openwrt_config_wireless/actions)

Ansible role that configures wireless settings of OpenWRT device (mainly those which are usually defined in `/etc/config/wireless`).

Requirements
------------
None

Role variables
--------------
Configuration settings defined in `openwrt_config_wireless` variable which are to be applied on OpenWRT device.
```yaml
openwrt_config_wireless:
  devices:
    radio0: { channel:  1, htmode: HT40, country: PL, legacy_rates: 1, disabled: 1 }
    radio1: { channel: 36 }
  interfaces:
    default_radio0: { device: radio0, mode: sta, ssid: mywifi, encryption: psk2+ccmp, key: Password, network: lan, isolate: 1,wps_pushbutton: 1 }
    mesh: { device: radio1, mode: mesh, encryption: sae, key: MeshPassword, network: lan, mesh_id: mymesh, mesh_fwding: 1, mesh_rssi_threshold: 0 }
```
If `openwrt_config_wireless` configuration variable is not present - no changes are made by role.

Dependencies
------------
This role depends on Ansible role `gekmihesg.openwrt` which allows to manage OpenWRT and derivatives with Ansible but without Python.

Example playbook using the role
-------------------------------
`./host_vars` \
`./host_vars/myrouter.yml`
```yaml
---
openwrt_config_wireless:
  devices:
    radio0: { channel:  1, htmode: HT40, country: PL, legacy_rates: 1, disabled: 1 }
    radio1: { channel: 36 }
  interfaces:
    default_radio0: { device: radio0, mode: sta, ssid: mywifi, encryption: psk2+ccmp, key: Password, network: lan, isolate: 1,wps_pushbutton: 1 }
    mesh: { device: radio1, mode: mesh, encryption: sae, key: MeshPassword, network: lan, mesh_id: mymesh, mesh_fwding: 1, mesh_rssi_threshold: 0 }
```
`./inventory.ini`
```ini
[openwrt]
myrouter
```
`./roles` \
`./roles/requirements.yml`
```yaml
---
roles:
- name: gekmihesg.openwrt
- name: pe_pe.openwrt_config_wireless
```
`./site.yml`
```yaml
---
- hosts: all
  roles:
    - role: gekmihesg.openwrt
    - role: pe_pe.openwrt_config_wireless
```
`./ansible.cfg`
```ini
[defaults]
# Define inventory location
inventory = ./inventory.ini
# Where to put roles downloaded from galaxy and other repos
roles_path = ./roles
# Defaults to /tmp to avoid flash wear on target device
remote_tmp = /tmp
```

Example execution
-----------------
Install roles and requirements:
```
ansible-galaxy role install -r roles/requirements.yml
```
Preview changes execution would perform on inventory:
```
ansible-playbook site.yml --check --diff
```
Execute playbook on inventory:
```
ansible-playbook site.yml
```
License
-------
MIT

Author Information
------------------
PePe
