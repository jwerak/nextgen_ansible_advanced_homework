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

## Note

Connection to prod bastion instance didn't work for my IdM user, therefore step **08_Prod SSH keys three tier app** failed.
To overcome this, copy manually your ssh keys to bastion host and re-run the workflow from failed step or from start.
