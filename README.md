# Shared user management example with ansible

This is example for managing all of company admin/operator users with ansible for all servers.


**Notice: It has been tested  with `Ubuntu` virtual servers. 

## What's included?
* Manage all your admin/operator users with  ansible
* Disables servers from asking for sudo password
* Disables ssh root access and password login for ssh

## How to setup
`vars/all/{groups,admins,operators}.yml` files contains list of company super admins/operatos which should gain access to all created servers. Remove our example data and put list of your admins over there instead.

`environments/production/inventory` file contains list of your servers. this is an inventory for ansible

Then create inventory file for the servers. This is an example:
```
[servers]
xxx.xxx.xxx.xxx
yyy.yyy.yyy.yyy
```
You can use hostnames or ip-addresses here.


after that run  : 

```
ansible-playbook -i environments/production users.yml -u root --ask-pass
```

## Example Disable Old SSH key

```
- name: "Pouria Dehghani"
    username: "pouria"
    keys:
      active:
        - "ssh-rsa NEW SSH KEY"
      disabled:
        - "ssh-rsa OLD SSH KEY "
    shell: "/bin/bash"
    state: present
```

## Example Disable user


before run the playbook, run following ansible command to kill all process's for that user you want to delete it :
```
ansible -m raw -a "sudo pkill -U SOMEUSERNAME" -i environments/production/inventory all  -u your-admin-username
```
After that change the state from present to absent  for SOMEUSERNAME:
```
- name: "SOMENAME SOMEFAMILY"
    username: "SOMEUSERNAME"
    keys:
      active:
        - "ssh-rsa NEW SSH KEY"
      disabled:
        - "ssh-rsa OLD SSH KEY "
    shell: "/bin/bash"
    state: absent
```


