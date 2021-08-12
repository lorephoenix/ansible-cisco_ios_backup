cisco_ios_backup
=========

An Ansible role to backup the configuration of Cisco network devices that runs on IOS/IOS-XE software.
```
    cd roles
    git clone https://github.com/lorephoenix/ansible-cisco_ios_backup.git cisco_ios_backup
```

Requirements
------------

This role makes use of the Ansible Cisco IOS and ansible.netcommon collection.
Both collections includes a variety of Ansible content to help automate the management of Cisco network appliances. 

To install the collection:
```
    cd cisco_ios_backup
    ansible-galaxy collection install -r requirements.yml
```

Role Variables
--------------

#### 1. defaults/main.yml

The backup variables are needed when you want to copy the current running 
configuration to a specific path on the Ansible Controller.

* `backup`: Enable or disable the function to make backups of running-configs from remote devices.
* `backup_dir`: Configuration backups into hierarchy based in DIR.
* `backup_clean`: Enable or disable cleanup on the oldest stored configuration files.
* `backup_keep`: Only keep the `number` newest changed configuration files.
```
    backup: <Boolean>
    backup_dir: <String>
    backup_cleanup: <Boolean>
    backup_keep: <Integer>
```

#### 2. Other variables for the role

You can store variables in the main inventory file or storing separate host and group variable files. Host and group variable files 
must use YAML syntax. Valid file extensions include ‘.yml’, ‘.yaml’, ‘.json’, or no file extension. 

* `ansible_become`: Equivalent of the become directive, decides if privilege escalation is used or not.
* `ansible_become_method`: Which privilege escalation method should be used.
* `ansible_become_password`: Set the privilege escalation password (never store this variable in plain text; always use a vault).
* `ansible_connection`: Connection type to the host. The string "ansible.netcommon.network_cli" is part of the ansible.netcommon 
collection.
* `ansible_network_os`: Informs Ansible which Network platform this hosts corresponds to. The string "ios" is part of the Cisco IOS 
collection.
* `ansible_user`: The default ssh user name to use.
* `ansible_password`: The ssh password to use (never store this variable in plain text; always use a vault).

```
    ansible_become: <Boolean>
    ansible_become_method: enable
    ansible_become_password: <Vault String>
    ansible_connection: ansible.netcommon.network_cli
    ansible_network_os: cisco.ios.ios
    ansible_user: <String>
    ansible_password: <Vault String>
```

Modes of Operation
------------

The role has 6 distinct modes of operation following the same order as below:

* **backup**: Make running configuration backup from the remote device.
* **facts**: Just gather facts about the remote device

Example Playbook
----------------

This is an example playbook:
```
    - hosts: cisco
      gather_fact: no
      roles:
         - cisco_ios_backup
```

To run a specific mode, simply specify the mode by using the tag attribute.

Using attribute tag 'backup' will only backup of the running configuration but it requires to enable the default variable 'backup'.
```
ansible-playbook your_playbook_name.yml -i your_inventory_file --tag backup -e "backup=yes"
```

Using attribute tag 'facts' will only gather device information.
```
ansible-playbook your_playbook_name.yml -i your_inventory_file --tag facts
```

License
-------

GPLv3

Author Information
------------------

- Christophe Vermeren | [GitHub](https://github.com/lorephoenix) | [Facebook](https://www.facebook.com/cvermeren)

