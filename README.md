# Ansible - OpenVPN Gateway

The Ansible playbook in this repository configures an Ubuntu 20.04 instance running OpenVPN to act as a NAT proxy.

The playbook assumes the instance runs in Oracle Cloud using the scripts below,
* [https://github.com/k3karthic/terraform__oci-instance-1](https://github.com/k3karthic/terraform__oci-instance-1).
* [https://github.com/k3karthic/ansible__busy-behind-nat](https://github.com/k3karthic/ansible__busy-behind-nat).

## Requirements

Install the following Ansible modules before running the playbook,
```
pip install oci
ansible-galaxy collection install oracle.oci
```

## Dynamic Inventory

This playbook uses the Oracle [Ansible Inventory Plugin](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/ansibleinventoryintro.htm) to populate public Ubuntu instances dynamically.

Public instances are assumed to have a freeform tag `openvpn_service: yes`.

## Playbook Configuration

1. Modify `inventory/oracle.oci.yml`
    1. specify the region where you have deployed your server on Oracle Cloud.
    1. Configure the authentication as per the [Oracle Guide](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/sdkconfig.htm#SDK_and_CLI_Configuration_File).
1. Set username and ssh authentication in `inventory/group_vars/all.yml`.
1. Set the OpenVPN virtual network in `inventory/group_vars/all.yml`.

## Deployment

Run the playbook using the following command,
```
./bin/apply.sh
```

## Encryption

Sensitive files like the SSH private keys are encrypted before being stored in the repository.

You must add the unencrypted file paths to `.gitignore`.

Use the following command to decrypt the files after cloning the repository,

```
./bin/decrypt.sh
```

Use the following command after running terraform to update the encrypted files,

```
./bin/encrypt.sh <gpg key id>
```
