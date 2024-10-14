# fit-for-omero_ansible-playbook
Ansible playbook examples to install OMERO used during the Fit for OMERO workshop.


# About this repository
The playbooks are not intended to be used as is for a production server but can be used as a starting point.

Ansible is a powerful tool that can control many server installation & configuration through SSH. However in this workshop, we will focus on installing OMERO on a single machine. We thus not need to setup the SSH to the server and can instead run the playbooks locally on a test server.

To learn more about Ansible and how to setup the SSH connection with remote servers, check out [this youtube serie](https://youtube.com/playlist?list=PLT98CRl2KxKEUHie1m24-wkyHpEsa4Y70&feature=shared).

See also [this OME repository](https://github.com/ome/prod-playbooks/tree/master/omero) for more playbook examples for OMERO.

# Using this repository

The commands are given assuming you have a Rocky Linux 9. For Ubuntu 22.04, perform the equivalent commands (see comments).
Connect to your test server, update it and install prerequisites to run Ansible.
```
sudoer> sudo dnf -y upgrade
sudoer> sudo dnf install epel-release -y
sudoer> sudo dnf install git ansible python3-pip -y
sudoer> sudo reboot
```

Clone this repository with a sudoer account
```
sudoer> git clone https://github.com/I3D-bio/fit-for-omero_ansible-playbook && cd fit-for-omero_ansible-playbook
```

Execute the playbook locally to install PostgreSQL, OMERO.server, and OMERO.web.
Encrypting the vault containing the passwords is good practice, but not a requirement for this to work.
```
sudoer> ansible-vault encrypt vars/vault.yml  
sudoer> sudo ansible-galaxy install -r requirements.yml
sudoer> sudo ansible-playbook --ask-vault-pass --become --connection=local -i inventory.yml playbook.yml
```

