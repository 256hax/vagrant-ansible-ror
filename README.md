Vagrant x Ansible for Ruby on Rails.

## Purpose
Quick and easy(like out-of-the-box) to start Ruby on Rails development environments.

## Environments
- Host
  - ansible-playbook 2.9.3
  - Vagrant 2.2.6
  - macOS Catalina version 10.15.3
- Guest
  - Frontend: Not specified (Ruby on Rails default)
  - Application: Ruby on Rails
  - Middleware: PostgreSQL, Puma
  - OS: CentOS 8, Fish shell
  - IP: 192.168.33.10
- Note
  - [Ruby version](https://github.com/256hax/vagrant-ansible-ror/blob/master/roles/ruby/vars/main.yml)

## Setup
Install VirtualBox and Vagrant.

## Run
```
$ cd [vagrant-ansible-ror directory]
$ vagrant up
```
Wait for 30-40 minutes.

```
$ vagrant ssh
vagrant$ cd src/sample-rails-sqlite
vagrant$ bundle exec rails s -b 0.0.0.0 -p 3000
```

Go to http://192.168.33.10:3000 on your local browser.
Yay! You’re on Rails!

Go to http://192.168.33.10:3000/users/ on your local browser.
You will see Rails sample scaffold with SQLite.

You can run Rails sample scaffold with PostgreSQL. Then got to same URL.
```
vagrant$ cd src/sample-rails-sqlite
vagrant$ bundle exec rails s -b 0.0.0.0 -p 3000
```

## Sync Directory Host<->Guest
We use Vagrant RSync.

Directory Path: (Host)src/ => (Guest)/home/vagrant/src

1. Install vagrant-rsync-back plugin
```
$ vagrant plugin install vagrant-rsync-back
```

2. Get all src from Guest to Host
```
$ vagrant rsync-back
$ ls -la ./src
```

3. Edit some files then sync from Host to Guest
```
$ vagran rsync
```

4. Optionally sync auto from Host to Guest
```
$ vagrant rsync-auto
```

## Handbook
[My Handbook](https://github.com/256hax/vagrant-ansible-ror/blob/master/docs/handbook.md)
