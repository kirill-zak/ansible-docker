# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- Test matrix for AlmaLinux 8.x/9.x, Rocky Linux 8.x/9.x, Alpine 3.22/3.23,
  Debian 12/13, Fedora 43/44, Ubuntu 22.04/24.04/26.04.
- `Makefile` with `make create-venv`, `make update-venv`, `make activate` and
  `make test-molecule` targets (venv lives in the gitignored `.ansible/`).

### Changed

- Replaced the duplicated molecule test matrix in `ci.yml` and `release.yml`
  with a single source of truth in `.github/workflows/config.yml`.
- CI checks now use a stable `test-gate` job name, so branch protection rules
  no longer need updating when test versions change.
- Pinned test tool versions in `requirements.txt` and pinned
  `community.general` in `requirements.yml` to a version range compatible
  with the pinned ansible-core (`>=11.0.0,<12.0.0`).
- Trimmed the molecule test sequence in `molecule/default/molecule.yml` to the
  used phases (drop dependency/prepare/cleanup/side_effect), silencing
  "missing playbook"/"missing requirements file" noise.
- `ansible-lint` now runs with `profile: production` and no skipped rules.
- Debian/Ubuntu/Fedora/AlmaLinux/Rocky Linux install tasks now add the Docker
  repository with `get_url` instead of shelling out to `config-manager`.
- Debian/Ubuntu install tasks now fetch the Docker signing key with `get_url`
  into `/etc/apt/keyrings` and reference it with `signed-by` in the sources
  entry, replacing the deprecated `apt_key` module.

### Fixed

- Molecule tests for AlmaLinux, Rocky Linux and Fedora failing in CI on the
  first task with `sudo: PAM account management error`. Sudo is broken inside
  the systemd-based images under the rootless podman available on GitHub
  runners (PAM cannot retrieve authentication info in the user namespace),
  so the converge playbook now connects as the root user the podman driver
  already provides and no longer escalates via `become`, avoiding sudo
  altogether.
- Fedora 43/44 tests failing with `Could not import the libdnf5 python
  module`: the `dnf` tasks now pin `use_backend: dnf4`, since the test images
  ship the dnf4 (`python3-dnf`) bindings but not `python3-libdnf5`, which the
  default dnf5 backend requires.
- Debian 13 (trixie) and Ubuntu 26.04 tests failing with `apt-key` not found:
  apt-key is no longer shipped on those releases, so the key is fetched into
  `/etc/apt/keyrings` and used via `signed-by`.
- Alpine tests failing with `ERROR: cannot start docker as networking would
  not start`: docker's OpenRC init script `need`s `net`, but the `networking`
  service cannot start in the container because there is no
  `/etc/network/interfaces` and no provider for the `dev` virtual service.
  The role now writes a loopback-only interfaces file and a small init
  script that provides `dev` before starting the docker service.
- Fedora 43/44 tests failing with dockerd `failed to create NAT chain DOCKER:
  Table does not exist`: docker-ce pulls the legacy iptables backend, which
  cannot create a NAT table under rootless podman (no `ip_tables` kernel
  module in the container). The role installs `iptables-nft` and removes
  `iptables-legacy` so dockerd uses the nftables backend.

### Removed

- CentOS 8 from the test matrix (CentOS 8 reached EOL).
- Tests for EOL Alpine 3.16–3.18, Fedora 35–38, Debian 10/11 (moved to
  Debian 12/13), Ubuntu 18.04/20.04 and AlmaLinux/Rocky Linux 9.0–9.2-only
  coverage.