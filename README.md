# Ansible Role: BadBlood Tiny ([Ludus](https://ludus.cloud))

An Ansible Role that runs [BadBlood](https://github.com/davidprowe/BadBlood) on a Domain Controller. By default the original script generates 2500 users, 500 groups, and several "computers" in AD, which was (at least in my original use case) overkill for a small AD lab. Additionally, there were issues and unneeded steps ran to execute BadBlood, which included downloading Git for Windows and cloning the repo. 
This role reduces how many AD objects are created, simplifies the BadBlood download process, plus saves the output of the current state of the domain as CSV files under `C:\ludus\badblood_tiny`.

I named it BadBlood Tiny because it creates less than the defaults. Maybe it's just bleeding?

> [!WARNING]
> This is meant to run on the Primary DC.

## Requirements

An internet connection to download BadBlood.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    # How many AD groups will be created
    ludus_badblood_tiny_groupcount: 5
    # How many AD users will be created
    ludus_badblood_tiny_usercount: 50
    # How many AD computers will be created (this is outside of domain-joined Windows boxes on the range)
    ludus_badblood_tiny_computercount: 0

## Dependencies

None.

## Example Playbook

```yaml
- hosts: primary_dc
  roles:
    - ludus_badblood_tiny
  role_vars:
    ludus_badblood_tiny_groupcount: 5
    ludus_badblood_tiny_usercount: 50
    ludus_badblood_tiny_computercount: 0
```

## Example Ludus Range Config

```yaml
ludus:
  - vm_name: "{{ range_id }}-ad-dc-win2022-server-x64-1"
    hostname: "{{ range_id }}-DC01-2022"
    template: win2022-server-x64-template
    vlan: 10
    ip_last_octet: 11
    ram_gb: 6
    cpus: 4
    windows:
      sysprep: true
    domain:
      fqdn: ludus.domain
      role: primary-dc
    roles:
      - ludus_badblood_tiny
    role_vars:
      ludus_badblood_tiny_groupcount: 5
      ludus_badblood_tiny_usercount: 50
      ludus_badblood_tiny_computercount: 0
```

## License

[//]: # (If you change the License type, be sure to change the actual LICENSE file as well)
GPL-3.0

## Author Information

This role was created by [@randy-halim](https://github.com/randy-halim), for [Ludus](https://ludus.cloud/).
