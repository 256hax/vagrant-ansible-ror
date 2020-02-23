# Vagrant x Ansible Development HandBook

## Vagrant Command
### Start provisioning
```
$ vagrant up
```

### Delete vagrant machine
```
$ vagrant destroy
```

## vagrant設定の反映
```
$ vagrant reload
```

## Ansible Playbook Command
### Check syntax(--syntax-check)
```
$ ansible-playbook -i hosts --syntax-check site.yml
```

### Dry run
#### Standard(--check)
```
$ ansible-playbook -i hosts site.yml --check
```

#### With tags(--tags)
```
$ ansible-playbook -i hosts site.yml --check --tags "common"
```

#### With tags(--tags) and tasks(--start-at)
```
$ ansible-playbook -i hosts site.yml --check --tags "common" --start-at="Rails new"
```

#### With debug(-vvv) (Tracing tasks)
```
$ ansible-playbook -i hosts site.yml --check -vvv
```

### Run
```
$ ansible-playbook -i hosts site.yml
```

## Documents
### Best Prctice
https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html

### Playbook Debugger
https://docs.ansible.com/ansible/latest/user_guide/playbooks_debugger.html

## Error
### SSH Error
If you got following SSH error,
```
fatal: [192.168.33.10]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh:
```

Generate key again.
```
$ ssh-keygen -R 192.168.33.10
```

Then run Ansible Playbook tasks.
