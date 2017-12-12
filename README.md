# ansible-openshift
Ansible Playbook to setup a single-node OpenShift 'cluster' on AWS.

## Preparation

Make a copy of file `group_vars/all.example` and rename it to `all`. Change default values with your AWS values.

## Create the OpenShift Instance

To create a new instance run this Ansible playbook:

```shell
$ ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i inventory/inventory.cfg create_openshift.yml
```

## Teardown the OpenShift Instance

To destroy the instance run this Ansible playbook:

```shell
$ ansible-playbook -i inventory/inventory.cfg teardown_openshift.yml
```

WARNING: 
This deletes the EC2 volume, i.e. ALL YOUR DATA IS LOST!
