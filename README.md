# Overview

This playbook is used to deploy a node exporter on a target server. 

It does the following:
- Creates a self-signed certificate LOCALLY
- Installs node exporter
- Creates a systemd service for the node exporter
- Creates the appropriate nginx configuration files
- Copies the certificate to remote server
- Starts the node exporter systemd service

# How to run it

To run it,
1) we must first specify the servers we wish to run it on in our inventory file. A sample inventory is present on this repo. The user is encouraged to follow this format and fill in all apropriate info.
2) Then we should change the developer_input_vars.yml appropriatelly

Path to the Certificate Authority cert.
```
abs_path_ca: /opt/ca/ca.crt
```
Path to the Certificate Authority key.
```
abs_path_ca_key: /opt/ca/ca.key
```
Where the process will take place (the generated cert and key will be placed there on {{signer}})
```
workspace: /opt/ca/certs
```

Specify where the certificate authority is located (could be localhost or could be another third party server e.g. the monitoring server)
```
signer: localhost
```

A list of IPs that are allowed through NGINX for this domain. Here you should provide a list of the IPs that will query this endpoint. For example Prometheus ip (maybe your IP for debugging purposes etc).
allowips:
  - {{some-allowed-ip}}

3) Clone required roles:

```
ansible-galaxy install -r requirements.yml --roles-path ./roles
```

4) Run the playbook:
```
ansible-plyabook playbook.yml -i inventory.yml --ask-become-pass
```