# Ansible Docker

An Ansible role that installs the latest version of Docker CE on the official installation instructions.

## Support distributive

* AlmaLinux 9
* CentOS 7
* CentOS 8
* Debian 10 (Buster)
* Debian 11 (Bullseye)
* Fedora 35
* Fedora 36
* Rocky Linux 9
* Ubuntu 18.04 (Bionic Beaver)
* Ubuntu 20.04 (Focal Fossa)
* Ubuntu 22.04 (Jammy Jellyfish)

## Requirements

N/A

## Role Variables

* `docker_debian_release`: Version of Debian release. Using `lsb_release -cs` If not present
* `docker_ubuntu_release`: Version of Ubuntu release. Using `lsb_release -cs` If not present
* `docker_users`: A list of users to add to the docker group. They will not be evaluated until a session is created.
* `docker_service_state`: Docker service state
* `docker_service_enabled`: Docker service enable status
* `docker_service_proxy`: Docker service proxy. Default value is empty.

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