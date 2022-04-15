# <h1>Ansible Playbook create/trigger a configuration backup </h1>

These playbooks help backup/export configurations. It uses the cisco.aci.aci_rest module as there are
no native modules that cover this scenario. You may be able to just run the cisco.aci.config_snapshot modules
to get what you need. 

The playbooks that this covers are:

- remote_location.yml - Creates a remote location for backups. This will probably only need to be run once. The attributes are as follows:
    - dn - Distinguished name of the remote path 
    - remotePort - Remote port, most likely 22
    - name - name of the remote location
    - protocol - Protocol that will be used with this remote location. This can be FTP (no), SCP, or SFTP
    - host - The IP address or hostname of the host. 
    - remotePath - The remote path where the configuration export will go
    - userName - Username on the remote host that this should use
    - userPasswd - Password for the provided username
    - status - Created to create a new policy. May be able to use deleted, but I have not tested it.

There is a child object for the Management EPG. In this example, I use the OOB/Management EPG.

- create_config_export.yml - Creates the configuration export policy. This probably only needs to be run once to create the policy. It requires that the remote location(Export Destination) be specified. This can be one that you created with remote_location.yml or one already configured through the GUI. 

This has a child object which references the remote path previously created.

trigger_config_export.yml - Triggers a configuration backup based on an export policy that was already configured. Notice that it just has the adminSt as triggered to run the config backup/export.

This playbook only requires the cisco.aci collection.

In a future update, I may combine these and add logic to test if policies and/or remote locations are already created.

This was tested with Ansible 2.12 core (Built with python 3.9.7) with ACI version 4.2(6h).
