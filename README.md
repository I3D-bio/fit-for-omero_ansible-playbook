# Fit for OMERO - Ansible playbook
Ansible playbook examples to install OMERO used during the Fit for OMERO workshop.


# About this repository
The playbooks are not intended to be used as is for a production server but can be used as a starting point.

Ansible is a powerful tool that can control many server installation & configuration through SSH. However in this workshop, we will focus on installing OMERO on a single machine. We thus not need to setup the SSH to the server and can instead run the playbooks locally on a test server.

To learn more about Ansible and how to setup the SSH connection with remote servers, check out [this youtube serie](https://youtube.com/playlist?list=PLT98CRl2KxKEUHie1m24-wkyHpEsa4Y70&feature=shared).

See also [this OME repository](https://github.com/ome/prod-playbooks/tree/master/omero) for more playbook examples for OMERO.

# Using this repository

The commands are given assuming you have a Rocky Linux 9 or Ubuntu 22.04, using a sudoer account.
Connect to your test server, update it and install prerequisites to run Ansible.

For Rocky Linux 9
```
sudo dnf -y upgrade
sudo dnf install epel-release -y
sudo dnf install git ansible python3-pip -y
sudo reboot
```

For Ubuntu 22.04
```
sudo apt-get update -y && sudo apt-get upgrade -y
sudo apt install -y git ansible python3-pip
sudo reboot
```

Clone this repository with a sudoer account
```
git clone https://github.com/I3D-bio/fit-for-omero_ansible-playbook && cd fit-for-omero_ansible-playbook
```

Execute the playbook locally to install PostgreSQL, OMERO.server, and OMERO.web.
Encrypting the vault containing the passwords is good practice, but not a requirement for this to work (in which case, do not use `--ask-vault-pass`).
```
ansible-vault encrypt vars/vault.yml  
sudo ansible-galaxy install -r requirements.yml
sudo ansible-playbook --ask-vault-pass --become --connection=local -i inventory.yml play_install_omero.yml
```

# Additional considerations
* File descriptor limit: see https://omero.readthedocs.io/en/stable/sysadmins/troubleshooting.html#too-many-open-files
* SSL Certificates for nginx
* Tempory fix of the memory leak with image import:
  * https://forum.image.sc/t/first-release-of-omero-process-container-steward/85067
  * https://github.com/glencoesoftware/omero-pc-steward#installation
 
# Server update
An update of OMERO.sever and OMERO.web is performed by changing the desired version in the respective config files (`vars/omero-server_config.yml`, `vars/omero-web_config.yml`) and re-running the playbooks as described above.

In the case of an OMERO.server upgrade, the previous version of OMERO.server is kept. Just from using the playbook given here, scripts that were imported manually or changed will not be moved to the new OMERO.server folder.

In the case of a change of configurations (for either web or server), the configuration files in `/opt/omero/web/config/` or `/opt/omero/server/config/` are regenerated.
