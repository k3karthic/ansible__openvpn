# Ansible — OpenVPN Gateway

The Ansible playbook configures an Ubuntu 20.04 instance running OpenVPN to act as a NAT proxy.

**Assumption:** The instance runs in Oracle Cloud using the scripts below,
* terraform__oci-instance-1
	* GitHub: [github.com/k3karthic/terraform__oci-instance-1](https://github.com/k3karthic/terraform__oci-instance-1).
	* Codeberg: [codeberg.org/k3karthic/terraform__oci-instance-1](https://codeberg.org/k3karthic/terraform__oci-instance-1).

## Requirements

Install the following before running the playbook,
```
pip install oci
ansible-galaxy collection install oracle.oci
```

## Dynamic Inventory

The Oracle [Ansible Inventory Plugin](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/ansibleinventoryintro.htm) populates public Ubuntu instances.

The target Ubuntu instances must have the freeform tag `openvpn_service: yes`.

## Playbook Configuration

1. Update `inventory/oracle.oci.yml`
    1. Specify the region where you have deployed your server on Oracle Cloud.
    1. Configure the authentication as per the [Oracle Guide](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/sdkconfig.htm#SDK_and_CLI_Configuration_File).
1. Set username and ssh authentication in `inventory/group_vars/all.yml`.
1. Set the OpenVPN virtual network in `inventory/group_vars/all.yml`.

## Deployment

Run the playbook using the following command,
```
./bin/apply.sh
```

## Encryption

Encrypt sensitive files (SSH private keys) before saving them. `.gitignore` must contain the unencrypted file paths.

Use the following command to decrypt the files after cloning the repository,

```
$ ./bin/decrypt.sh
```

Use the following command after running terraform to update the encrypted files,

```
$ ./bin/encrypt.sh <gpg key id>
```

