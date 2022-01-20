# Wildfly with mysql drivers

This repository contains a set of Ansible based roles and playbooks to demonstrate the deployment of a [Wildfly](https://wildfly.org/) cluster, mysql database, mysql drivers, and kitchensink application configured to connect to mysql.

These playbooks will install a single mysql instance on a dedicated server, and will deploy multiple Wildfly instances on individual servers.  These wildfly instances will be configured to connect to the mysql instance.  
## Prerequisites
2 or more RHEL 8.4+ vms with 4096mb per vm.


## Set up

The following sections describe the steps necessary to prepare your machine for execution

### JCliff Integration

First of all, you'll need to install the [JCliff Ansible collection](https://github.com/middleware_automation/ansible_collections_jcliff):

    $ ansible-galaxy collection install -r molecule/default/requirements.yml

### Ansible Inventory

Ansible groups are used to define the Wildfly instances. Configure these groups in the [hosts](inventory/hosts) file similar to the following:

```
# Placeholder Group
[demo]

# Wildfly Group
[wildfly]
192.168.122.11
192.168.122.125

# Mysql database Group
[mysql]
192.168.122.84

[demo:children]
wildfly
mysql
```

## Execution

That's all! You can now run the playbook to set up the demo:

    $ ansible-playbook -i inventory/hosts demo.yml --extra-vars "rhn_username=philip@deewhy.ie rhn_password=KIN03der" 
