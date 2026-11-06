# Ansible Docker

An Ansible role that installs the latest version of Docker CE on the official installation instructions.

## Support distributive

* AlmaLinux `8.x` (`8.5` – `8.10`), `9.x` (`9.0` – `9.8`)
* Alpine Linux `3.22.x`, `3.23.x`
* Debian `12` (`Bookworm`), `13` (`Trixie`)
* Fedora `43`, `44`
* Rocky Linux `8.x` (`8.5` – `8.9`), `9.x` (`9.0` – `9.8`)
* Ubuntu `22.04` (`Jammy Jellyfish`), `24.04` (`Noble Numbat`), `26.04`

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