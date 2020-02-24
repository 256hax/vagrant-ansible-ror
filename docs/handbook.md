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
### Get Started
- [User Gudie](https://docs.ansible.com/ansible/latest/user_guide/index.html)
- [Best Prctice](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html)
- [Playbook Debugger](https://docs.ansible.com/ansible/latest/user_guide/playbooks_debugger.html)

## Coding Guideline
### command vs shell
Conclusion
- Command through the shell such as <, >, |, source, export, etc => shell
- Else (highly recommend) => command

[command – Execute commands on targets](https://docs.ansible.com/ansible/latest/modules/command_module.html#notes)

```
If you want to run a command through the shell (say you are using <, >, |, etc), you actually want the shell module instead. Parsing shell metacharacters can lead to unexpected commands being executed if quoting is not done correctly so it is more secure to use the command module when possible.
```

## Errors
### SSH Error
If you got following SSH error,

```
fatal: [192.168.33.10]: UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@\r\n@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @\r\n@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@\r\nIT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!\r\nSomeone could be eavesdropping on you right now (man-in-the-middle attack)!\r\nIt is also possible that a host key has just been changed.\r\nThe fingerprint for the ECDSA key sent by the remote host is\nSHA256:IpYETf/cvdT1t7RXpRsPWxc9uqYyGOjawYnMRWHO4/g.\r\nPlease contact your system administrator.\r\nAdd correct host key in /Users/user/.ssh/known_hosts to get rid of this message.\r\nOffending ECDSA key in /Users/user/.ssh/known_hosts:19\r\nECDSA host key for 192.168.33.10 has changed and you have requested strict checking.\r\nHost key verification failed.",
    "unreachable": true
}
```

Generate key again.

```
$ ssh-keygen -R 192.168.33.10
```

Then run Ansible Playbook tasks again and say "yes".

```
The authenticity of host '192.168.33.10 (192.168.33.10)' can't be established.
ECDSA key fingerprint is SHA256:IpYETf/cvdT1t7RXpRsPWxc9uqYyGOjawYnMRWHO4/g.
Are you sure you want to continue connecting (yes/no)? yes
```
