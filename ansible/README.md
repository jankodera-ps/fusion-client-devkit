# Ansible Playbooks for the Fusion DevKit

## Summary
The goal of this document is to help you set up and understand the functionality and scope of the ansible playbooks here providers.


### How run a playbook
```
ansible-playbook playbook_name.yml
```

### Enviroment Variables
In almost every playbook, there are 2 lines that required a enviroment variable

```
app_id: "{{ ansible_env.API_CLIENT}}"
key_file: "{{ ansible_env.PRIV_KEY_FILE}}"
```
In this case, the variables are: API_CLIENT and PRIV_KEY_FILE
To set them, you can temporaly enable them with:
```
export API_CLIENT='pure1:apikey:123456789'
export PRIV_KEY_FILE='/home/user/key.pem'
```
in the case of ```PRIV_KEY_FILE```, the path to the ```key.pem``` need to be aboslute.

IF you prefer to not use enviroment variables, you can change the values inside the playbook:

```
app_id: "<your_API_Application_ID_here>"
key_file: "/home/user/key.pem"
```

## Folder: simple
This series of playbooks are meant to run as standalone, so no need for external input, and all information required to create a resource/element are inside each playbook.
Some elements need a previous element to exist to be linked.

## Folder: sample_production
This series of playbooks are mean to run based on the info inside the files on folder ```group_vars```.
Usually the name in the files are almost identical.
To detect without error what files are linked to specific playbook, loom inside the same, there will be an import with the value:path to the file.
```
   - include_vars: group_vars/consumer.yml
```

### sample_production/inventory.ini
Inside this file, you can declare your hosts that will act as initiators.
```
[Initiators_Hosts]
initiatorserver1
initiatorserver2
initiatorserver3
```
In this example we have declared 3 hosts, and for each one inside folder ```host_vars``` in the current folder folder, you will found a file ```.yml``` that matches the name of the host initiator.
Inside there is a template that covers the minimal information ansible needs to make the conection with that host.
```
ansible_user: username
ansible_host: 192.168.1.100
ansible_ssh_private_key_file: ~/.ssh/<private_key_file>
```
