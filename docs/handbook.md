# Vagrant x Ansible Development HandBook

## Vagrant Command
### Start provisioning
```
$ vagrant up
```

### Delete Vagrant machine
```
$ vagrant destroy
```

## Reload Vagrant (halt and up)
```
$ vagrant reload
```

## Ansible Playbook Command
### Common Options
- `--syntax-check`: check syntax
- `--check`: dry run
- `--tags "<tags name>"`: only run plays and tasks tagged
- `--start-at-task "<task name>"`: start the playbook at the task name
- `-vvv`: debug on console
- `--step`: one-step-at-a-time: confirm each task before running

### Example
Run Playbook
```
$ ansible-playbook -i hosts site.yml
```

Dry run with tags and starting at task name
```
$ ansible-playbook -i hosts site.yml --check --tags "rails" --start-at-task="Rails new"
```

Run Playbook one step at a time with debug
```
$ ansible-playbook -i hosts site.yml --tags "rails" --step -vvv
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

## PostgreSQL
### Test Connection
1. Connection to PostgreSQL Server

```
$ vagrant ssh
vagrant$ bundle exec rails db
```

or

```
$ psql -h 192.168.33.10 -U postgres
```

2. Show Databases
```
postgres=# \l
                                               List of databases
                Name                 |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
-------------------------------------+----------+----------+-------------+-------------+-----------------------
 postgres                            | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 sample_rails_postgresql_development | vagrant  | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 sample_rails_postgresql_test        | vagrant  | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 template0                           | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
                                     |          |          |             |             | postgres=CTc/postgres
 template1                           | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
                                     |          |          |             |             | postgres=CTc/postgres
(5 rows)
```

3. Connection to Database
```
postgres=# \c sample_rails_postgresql_development
```

4. Show Tables
```
sample_rails_postgresql_development=# \d
                 List of relations
 Schema |         Name         |   Type   |  Owner
--------+----------------------+----------+---------
 public | ar_internal_metadata | table    | vagrant
 public | schema_migrations    | table    | vagrant
 public | users                | table    | vagrant
 public | users_id_seq         | sequence | vagrant
(4 rows)
```

5. Show Record
```
sample_rails_postgresql_development=# select * from users;
 id |   name   | age |         created_at         |         updated_at
----+----------+-----+----------------------------+----------------------------
  1 | pg tarou |  30 | 2020-02-24 13:52:39.927298 | 2020-02-24 13:52:39.927298
(1 row)
```
