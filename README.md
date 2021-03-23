# Ansible Advanced - Homework

This repository contains Ansible roles and playbooks to:
- Configure Ansible Tower job template workflow
- deploy Infrastructure for QA and Prod environments
- deploy 3-tier application to QA and Prod environments and check application health with smoke test.

## Prerequisites

### On Control node

- Login as root
- Create file with env variables describing your environment:

```bash
cat <<EOF > ~/labrc
export TOWER_GUID=<Tower env GUID>
export OSP_GUID=<OSP Env GUID>
export OSP_DOMAIN=<OSP Env domain name>
export OPENTLC_ID=<username-example.com>
export MAIL_ID=<username@example.com>
export OPENTLC_PASSWORD=<Your OPENTLC Password>
export GITHUB_REPO=https://github.com/<github username>/nextgen_ansible_advanced_homework.git
export JQ_REPO_BASE=http://www.opentlc.com/download/ansible_bootcamp
export REGION=us-east-1
export VAULT_PASSWORD=r3dh4t1!
export ANSIBLE_VAULT_PASSWORD_FILE=/root/.vault-pwd-file
EOF

```

Create vault password file.
For the convenience, there is already file in *vars/secrets.yml* containing secrets:

```yaml
---
tower_username: admin
tower_password: r3dh4t1!
vault_password: r3dh4t1!
...
```
The file is encrypted with password `r3dh4t1`.

Update the secrets file with your credentials.

```bash
echo ${VAULT_PASSWORD} > ${ANSIBLE_VAULT_PASSWORD_FILE}
```

## Setup Tower environment

```bash
ansible-playbook site-config-tower.yml \
      -e tower_guid=${TOWER_GUID} \
      -e osp_guid=${OSP_GUID} \
      -e osp_domain=${OSP_DOMAIN} \
      -e opentlc_login=${OPENTLC_ID} \
      -e path_to_opentlc_key=/root/.ssh/mykey.pem \
      -e param_repo_base=${JQ_REPO_BASE} \
      -e opentlc_password=${OPENTLC_PASSWORD} \
      -e REGION_NAME=${REGION} \
      -e EMAIL=${MAIL_ID} \
      -e github_repo=${GITHUB_REPO} \
      -e @vars/secrets.yml
```

## Start deployment to qa and prod environments

Start Workflow Template after *site-config-tower.yml* is successfully finished.

Start Workflow Template *cicd_workflow* in order to trigger environments creation and application deployment.

Go to Ansible Tower -> Templates -> find cid_workflow -> launch.

## Project components

### Playbooks

| Job Template                              | Playbook name          | Purpose                                                                                                       |
|-------------------------------------------|------------------------|---------------------------------------------------------------------------------------------------------------|
| 01_Provision Prod Env                     | aws_provision.yml      | Provision production environment in AWS                                                                       |
| 02_Provision QA Env                       | site-osp-instances.yml | Provision QA environment in OpenStack                                                                         |
| 04_3tier app deployment on QA Env         | site-3tier-app.yml     | Deploy three tier application to QA                                                                           |
| 05_Smoke test QA Env                      | site-smoke-osp.yml     | Playbook to test three tier app on OSP                                                                        |
| 07_Prod Check the status of AWS instances | aws_status_check.yml   | Check aws instances are up or not                                                                             |
| 08_Prod SSH keys three tier app           | aws_creds.yml          | Use ssh private key from AWS bastion host and use it to create machine credential to connect to AWS instances |
| 09_3 tier app on Prod                     | site-3tier-app.yml     | Deploy three tier application to Production                                                                   |
| 10_Smoke test Prod env                    | site-smoketest-aws.yml | test three tier app on AWS                                                                                    |
| Nuke QA Env                               | site-osp-delete.yml    | Delete QA OpenStack instances                                                                                 |
| Executed on controller node only          | site-config-tower.yml  | Configure Ansible Tower with jobs from this project                                                           |
| Executed on controller node only          | site-setup-prereqs.yml | Configure Ansible Tower to use isolated node                                                                  |
| Executed on controller node only          | grading-script.yml     | Self grading script                                                                                           |

### Roles

Basic overview of roles.
Documentation is part of each role.

| Playbook name       | Purpose                                                                  |
|---------------------|--------------------------------------------------------------------------|
| app-tier            | Install application on server                                            |
| db-tier             | Install postgresql server                                                |
| lb-tier             | Install HA proxy server                                                  |
| base-config         | Setup yum repo and base packages role                                    |
| setup-workstation   | Setup workstation, create network, ssh keypair, security group etc. role |
| osp-servers         | Provision OSP Instances role                                             |
| osp-instance-delete | Delete OSP Instances role                                                |
| osp-facts           | Generate in-memory inventory for OSP instances role                      |
| tower-cli-setup     | Install tower-cli and create config file                                 |
| config-tower        | Role to configure ansible tower job templates and workflow               |

## Notes

Connection to prod bastion instance didn't work for my IdM user, therefore step **08_Prod SSH keys three tier app** failed.
To overcome this, copy manually your ssh keys to bastion host and re-run the workflow from failed step or from start.

## Future work recommendation

### Ansible Tower objects creation

Currently creation of Ansible Tower Objects, such as Job Templates, credentials, etc is very static and ansible code is not being re-used.
Recommendation is to create Role for tower object creation that would specify general variables structure as input for Job Templates creation.

### Commands to create Ansible tower objects

Some commands used to create Ansible Tower objects will only create resources, but won't update them.

Recommendation is to migrate to modules from *ansible.tower* collection.
