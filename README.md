# Ansible Docker

An Ansible role that installs the latest version of Docker CE on the official installation instructions.

## Support distributive

* centos7
* centos8
* debian10
* debian11
* fedora34
* fedora35
* ubuntu2004
* ubuntu1804

## Requirements

N/A

## Role Variables

* `docker_debian_release`: Version of Debian release. Using `lsb_release -cs` If not present
* `docker_ubuntu_release`: Version of Ubuntu release. Using `lsb_release -cs` If not present
* `docker_users`: A list of users to add to the docker group. They will not be evaluated until a session is created.
* `docker_service_state`: Docker service state
* `docker_service_enabled`: Docker service enable status

## Dependencies

N/A

## Example Playbook

```yaml
- hosts: all
  become: true
  roles:
    - role: kirill_zak.docker
```

## License

GPLv3

## Author Information

https://kirill-zak.ru

Based on [Ansible role](https://github.com/calvinbui/ansible-docker) by [Calvin Bui](https://github.com/calvinbui).