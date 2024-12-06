## About
This repository contains two playbooks where I use Ansible tasks to automate common post deployment procedures after deploying a new OpenStack deployment.

## Requirements
This has been successfully tested in the following platforms:

#### Openstack and OpenStack SDK versions tested
- OpenStack Train through Wallaby 
- openstacksdk == 0.62.0

#### Ansible and Ansible Collections versions tested
- Ansible core == 2.15.x
- openstack.cloud == 1.10.0


## Playbooks Descriptions

`bootstrap_openstack_cluster_post_deployment.yml`
The playbook demonstrates the use of the openstack.cloud.* modules for bootstrapping the initial resources generally not created by default following a new installation of an OpenStack deployment.

This playbook performs the following actions which can be customized using the variables file. This is used to bootstrap the admin project in OpenStack:

- Creates default openstack flavors
- Uploads OS images to the openstack image store
- Creates neutron provider networks(can be shared between projects)
- Creates neutron provider subnets for the admin project(can be a shared between projects)
- Creates security groups for the admin project
- Create security group rules for the admin project

`bootstrap_openstack_tenant.yml`
The playbook demonstrates the use of the openstack.cloud.* modules for bootstrapping a new OpenStack project, also known as tenants. These are the consumers of the OpenStack cloud.

This playbook performs the following actions which can be customized using the variables file. 

- Creates an openstack project/tenant
- Sets OpenStack resource quotes for project/tenant
- Creates Openstack local users for project/tenant
- Configures OpenStack role assignments
- Creates neutron tenant networks
- Creates neutron tenant subnets
- Creates neutron router in tenant
- Creates security groups in tenant
- Creates security group rules for the tenant

## Additional Notes
- Vaulted variables files with sensitive information have been ommited from this repository.  There is an example file containing these variables as a reference:

```
#######################################################
# vaulted variables used for openstack api authorization
#######################################################
openstack_auth_url: https://openstack-api.lab.example.com:13000
openstack_username: admin
openstack_user_passwd: iliketurtles
```
- These playbooks while fully functional are by no means complete and you may run into issues, however they provide a decent baseline for you to start with.

- Most issues that I have encountered has been related to version mismatches between OpenStack SDK and the Ansible Collection versions. Review the Ansible OpenStack Collection documentation to ensure you are aligned with the collection and OpenStack SDK versions supported by your OpenStack environment: [https://github.com/openstack/ansible-collections-openstack](https://github.com/openstack/ansible-collections-openstack)

- Sample ansible.cfg file. These playbooks run from the same host where the requirements are installed, but can also be modified to your preferences. My example uses the vault_password_file parameter for vault decryption without a user prompt:
```
[defaults]
inventory   = ./inventory
vault_password_file = /home/arlaporte/.ansible/vault/projects/vault_pass.txt
```
