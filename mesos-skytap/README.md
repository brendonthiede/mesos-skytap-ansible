## Mesos Cluster

#### Based on tutorial at [https://github.com/debianmaster/Notes/wiki/Mesos---marathon---Docker-cluster-setup-on-RHEL-7-with-three-masters]()

### Requirements
- Requires Ansible 1.2 or newer
- Expects CentOS/RHEL 7.x host/s

### Configuration

You need to copy the server config from `common/files/hosts` to your `etc/hosts`

#### Generating and Storing Secrets

For secrets management, you should set up a password file named `.demo_vault_pass.txt` in the mesos-skytap
directory.  You can grab the value from any of the hosts:

    ssh root@172.19.0.176 "cat /root/.demo_vault_pass.txt"

For adding secrets to the file, you can decrypt the file by running:

    ansible-vault decrypt --vault-password-file=./.demo_vault_pass.txt group_vars/vault

After modifying the file, re-encrypt it with:

    ansible-vault encrypt --vault-password-file=./.demo_vault_pass.txt group_vars/vault

Make sure not to commit the file while unencrypted.  More here: [http://docs.ansible.com/ansible/playbooks_vault.html]()

### Applying Playbook

#### Hosts configuration

You can grab the key from any of the SkyTap hosts:

    scp root@172.19.0.176:/root/.ssh/id_rsa ~/.ssh/id_rsa_demo

#### Updating hosts

To apply the configuration, use the following:

    ansible-playbook mesos-master.yml -i ./hosts -e@group_vars/vault --vault-password-file=./.demo_vault_pass.txt
    ansible-playbook mesos-slave.yml -i ./hosts -e@group_vars/vault --vault-password-file=./.demo_vault_pass.txt