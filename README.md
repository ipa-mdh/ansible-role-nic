# ansible-role-NIC

## Description

This Ansible role manages Ethernet network connections via NetworkManager’s `nmcli`.  
It can configure a static IPv4 address or use DHCP/automatic addressing, and optionally enable autoconnect on boot.

## Requirements

- Ansible **2.10** or newer  
- [NetworkManager](https://networkmanager.org/) and `nmcli` installed on target hosts  
- The [`community.general`](https://galaxy.ansible.com/community/general) collection  

## Role Variables

| Variable                  | Default           | Description                                                                                             |
|---------------------------|-------------------|---------------------------------------------------------------------------------------------------------|
| `network_name`            | `network-name`    | Name of the connection profile to create or manage.                                                     |
| `ethernet_interface_name` | `eno1`            | Interface name (as shown by `ip link show`) to bind the connection to.                                  |
| `ipv4`                    | `'IGNORE'`        | Static IPv4 CIDR (e.g. `192.168.1.10/24`). Set to `'IGNORE'` to skip static config and use `method4`.    |
| `method4`                 | `auto`            | IPv4 addressing method when `ipv4` is `'IGNORE'`. One of `auto` (DHCP) or `manual`.                     |
| `auto_connect`            | `false`           | Whether the connection should autoconnect on boot (`true` or `false`).                                  |

---

## Dependencies

This role depends on the `community.general` collection for the `nmcli` module:

```bash
ansible-galaxy collection install community.general
```

---

## Example Playbook

```yaml
- hosts: all
  become: true
  roles:
    - role: NIC
      network_name: private
      ethernet_interface_name: ens19
      ipv4: 10.0.0.3/24
      auto_connect: true

    - role: NIC
      network_name: internet
      ethernet_interface_name: ens19
      # Use DHCP/auto addressing
      method4: auto
      auto_connect: true
```

---

## Usage in `requirements.yml`

To install both the role and its collection dependency:

```yaml
collections:
  - name: community.general
    version: "5.3.0"

roles:
  - name: nic
    src: git@…/NIC.git
```

Then:

```bash
ansible-galaxy collection install -r requirements.yml
ansible-galaxy role install       -r requirements.yml
```

---

## License

This role is released under the MIT License.

