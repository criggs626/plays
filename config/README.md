## Syslog-ng config
This ansible-playbook is built to manage the syslog ng config on our remote splunk heavy forwarders. It works by targeting the config of one server and syncing it to others in a group. The target server will have all contents of /etc/syslog-ng coppied to the controller and the directory structure of /opt/splunk-syslog coppied to the controller. The information on the controller is then synced to any host group specified.

Instructions
---
### Host specification
##### For target:
The target host to get the config from is specified on line 18 in the playbook
```yaml
- hosts: targetServer
```
And hosts are defined in the /opt/ansible/hosts file as follows
```
NAME ansible_host=127.0.0.1 ansible_user=USER
```
#### For remote:
The remote hosts to be configured are specified on line 44 in the playbook
```yaml
- hosts: group
```
And host groups are defined in the /opt/ansible/hosts file as follows
```
NAME1 ansible_host=127.0.0.1 ansible_user=USER

[GROUP_NAME]
NAME1
NAME2 ansible_host=127.0.0.2 ansible_user=USER
NAME3 ansible_host=127.0.0.3
NAME4 ansible_host=127.0.0.4
```
Note: The remote hosts will try to connect to root regardless of USER specified due to the fact that they have to operate on /opt and /etc which are root access only directories.
### Runtime instructions
To run make sure the ssh keys are all setup for access. Then simply
```
sudo ansible-playbook configSyslog.yml
```
