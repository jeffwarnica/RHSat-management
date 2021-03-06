# satellite-management-playbooks

The main entry point for Satellite installation, configuration and artifact export.

## Layout and Design

This project is for the management of a Satellite installation, modeled around a lab/dev system, and several production systems. Install is broken up into two phases, install and configure, and there is additionally an export mechanism.

The theory of operation is that you can define in code the baseline configuration of several Satellite systems (themselves being very similar), and deploy them together. Day 0 install time and first run configuration time settings are encoded as Ansible inventory settings.

There is, furthermore, the ability to export the "runtime" configuration of a Satellite system ("everything" except the content itself), and import that dump to another system.

### Common Inventory

inventory/ : Stores an inventory of the various Satellite infrastructures. Generally we would expect many common settings to be in inventory/group_vars/all.yml (git repos and install time configuration settings, for example), though one may wish to have, e.g. different passwords in inventory/group_vars/<infra>.yml 

Depending on your circumstances, you may wish to store passwords vaulted in these inventory files, or locally in an extra var file, or perhaps leverage an external credential management system.

### Common Roles

roles/ stores the *configuration* settings for all the dependent roles. At runtime it would be populated with the roles themselves from their respective git or Galaxy repositories. See roles/README.md for details.

### Significant Operations

#### Install

satellite_install.yml : Does a day 0 install of Satellite to a suitably configured RHEL 7.x server. Connects to the Red Hat Network (RHN), patches the OS, configures LVM, and runs the Satellite installation script proper. Additionally, critical day 0 configurations are setup.

#### Configure

satellite_configure.yml : Day 0/1 configuration and reconfiguration. Reads settings from a specified git repository. Intentionally dumb as it applies "all" settings found in the git repository.

#### Export

export_configuration.yml : Exports "all" the configuration out of a Satellite, and stores it in a specified git repository.


## Sample invocation

```
ansible-playbook -i inventory/lab export_configuration.yml -e satellite_admin_password='my_cool_password17'
```

Authors
--------
Jeff Warnica: <jwarnica@redhat.com>    
Cory McKee: <cmckee@redhat.com>      
Ritesh Chitala: <rchitali@redhat.com>    


