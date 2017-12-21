# ansible-openshift
Ansible Playbook to setup a single-node OpenShift 'cluster' on AWS.

## Preparation

Make a copy of file `group_vars/all.example` and rename it to `all`. Change default values with your AWS values.

NOTE:
Value of `ansible_ssh_private_key_file` has to be an absolute path!


## Create the OpenShift Instance

To create a new instance run this Ansible playbook:

```shell
$ ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i inventory/inventory.example create_openshift.yml
```

## Teardown the OpenShift Instance

To destroy the instance run this Ansible playbook:

```shell
$ ansible-playbook -i inventory/inventory.example teardown_openshift.yml
```

WARNING: 
This deletes the EC2 volume, i.e. ALL YOUR DATA IS LOST!

## Cluster Administration

In order to get admin access to the cluster execute the following steps *INSIDE* the AWS instance the cluster is running on:

	$ cp /opt/openshift/data/v1/master/admin.kubeconfig .

Now you can login as the admin user:

	$ oc login -u system:admin --config=admin.kubeconfig https://<master.example.com>:8443

### Add sudoer permissions to user 'developer'

	oc adm policy add-cluster-role-to-group sudoer system:authenticated --config=admin.kubeconfig

Now any user is sudoer. They can execute commands with '--as=system:admin'

### DANGERZONE

Make user 'developer' a cluster admin

	$ oc adm policy add-cluster-role-to-user cluster-admin developer --config=admin.kubeconfig

If this created havoc, remove the permission:

	$ oc adm policy remove-cluster-role-from-user cluster-admin developer --config=admin.kubeconfig