# JBoss EAP with mod_cluster and postgresql drivers

This repository contains a set of Ansible based roles and playbooks to demonstrate the deployment of a [JBoss EAP](https://wildfly.org/) cluster, load balancer, postgresql database, postgresql drivers, and addressbook application configured to connect to postgresql.

These playbooks will install a single postgresql instance on a dedicated server, and will deploy JBoss EAP instances on individual servers.  These Jboss EAP instances will be configured to connect to the postgresql instance. Mod_cluster is deployed as a load balancer. 
## Prerequisites
3 or more RHEL 8.4+ vms with 4096mb per vm.


## Set up

The following sections describe the steps necessary to prepare your machine for execution

### pre-requisites

* Navigate to https://cloud.redhat.com/ansible/automation-hub/token/.
* Click Load Token.
* Click copy icon to copy the API token to the clipboard.


Add your token to ansible.cfg, then you'll need to install ansible collections.  This is done by running the following command:

    $ ansible-galaxy collection install -r molecule/default/requirements.yml

### Ansible Inventory

Ansible groups are used to define the Wildfly instances. Configure these groups in the [hosts](inventory/hosts) file similar to the following:

```
# Placeholder Group
[demo]

# jboss Group
[jboss]
192.168.122.11
192.168.122.125

# postgresql database Group
[postgresql]
192.168.122.84

# jbcs Group
[jbcs]
192.168.122.85

[demo:children]
jboss
postgresql
jbcs
```

## Execution
You need to provide the credentials associated with a service account. You can manage service accounts using the hybrid cloud console. Within this portal, on the [service accounts tab](https://console.redhat.com/application-services/service-accounts), you can create a new service account if one does not already exist. Copy your service account username and password and use these below.  You will also need to provide the external-domain-name of your frontend where JBCS is going to be deployed


    `ansible-playbook -i inventory/hosts demo.yml --extra-vars "rhn_username=<your_username> rhn_password=<your_password> jbcs_external_domain_name=external-domain-name"`
