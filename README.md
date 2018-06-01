# mhutter.iptables

Manage iptables rules using Ansible.


## Usage

This role needs to be included in your playbook twice:

```yaml
- hosts: all
  vars:
    iptables_phase: pre
  roles:
    - name: mhutter.iptables
      tags: iptables

# ... all other plays & tasks of your playbook

- hosts: all
  vars:
    iptables_phase: post
  roles:
    - name: mhutter.iptables
      tags: iptables
```

The **pre** phase sets up all requirements (packages, folders, ...) while the
**post** phase creates some defaults, generates the final iptables rule files
and applies them.

### Rules

To add your own rules, drop the appropriate files in
`/etc/iptables/fragments.{v4,v6}`:

```yaml
# roles/etcd/tasks/main.yml

- name: Prepare iptables rules
  copy:
    content: '-A INPUT -p tcp -m multiport --dports 2379,2380 -j ACCEPT'
    dest: /etc/iptables/fragments.v4/10_etcd
    owner: root
    group: root
    mode: 0644
  tags: iptables
```

TODO: Add module that simplifies rule definitions

## Installation

Add the following snippet to your `requirements.yml`:

```yaml
- name: mhutter.iptables
  version: master
```

And then run `ansible-galaxy install -r requirements.yml` as usual

## Role Variables

* `iptables_phase` - either `pre` or `post`, see "Usage" section
