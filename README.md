# Ansible Notes
Create local public-private key, then copy it to the servers to access.
```
ssh-keygen -t ed25519 -C "jay default"   # ed25519 is very secure
-t ed25519 type (very secure key with shorter key link)
-C "jay default" comment (meta data about the key)
```
Enter a passphrase (can cache this later)
Saves to /home/jay/.ssh/id_ed25519 : id_ed25519 and id_ed25519.pub
ssh-copy-id -I ~/.ssh/id_ed25519.pub 172.16.250.132
.ssh/authorized_keys is created on each server if it was not already there, and the public key is added into that file (you could just copy the string in manually, ssh-copy-id is just a convenience
The id is copied to the remote host and prompts you to ssh onto that host to test
ssh onto each server and enter the passphrase
Now create a key just for ansible:
Ssh-keygen -t ed25519 -C "ansible"   # This will try to save to the same location!
So change path to /home/jay/.ssh/ansible
Do NOT create a passphrase for this key
```

ssh-copy-id -I ~/.ssh/ansible.pub 172.16.250.132
ssh-copy-id -I ~/.ssh/ansible.pub 172.16.250.133
ssh-copy-id -I ~/.ssh/ansible.pub 172.16.250.134
Will ask for passphrase that it will use to facilitate the copy to the other server.
cat .ssh/authorized_keys    # will show id_ed25519 and ansible
Ssh -I ~/.ssh/ansible 172.16.250.133
eval $(ssh-agent)      # cache ssh passphrases
Agent pid 2362     # ssh agent is now running in the background
ssh-add    # Enter passphrase to add the identity
alias ssha='eval $(ssh-agent) && ssh-add'
```
`Ansible all --key-file ~/.ssh/ansible -i inventory -m ping`
-I is the file with the ip addresses of the files
-m 'module' ping, not actually a ping, makes a test connection to each server
```
Vi ansible.cfg
[defaults]
inventory = inventory
private_key_file = ~/.ssh/ansible
```
With ansible.cfg set, we can simply do:
```
Ansible all -m ping
Ansible all --list-hosts
Ansible all gather_facts
Ansible all gather_facts --limit 172.16.250.134   # limit scope to a single host
```

`# ansible-notes
