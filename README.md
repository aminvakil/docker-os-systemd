# docker-os
Multiple OSs Docker containers (systemd included).

Can be used for molecule testing.

Distros available:

- `quay.io/aminvakil/rockylinux8-systemd`
- `quay.io/aminvakil/centos8-systemd`
- `quay.io/aminvakil/centos7-systemd`
- `quay.io/aminvakil/debian10-systemd`
- `quay.io/aminvakil/debian11-systemd`
- `quay.io/aminvakil/ubuntu20.04-systemd`
- `quay.io/aminvakil/ubuntu18.04-systemd`
- `quay.io/aminvakil/ubuntu16.04-systemd`
- `quay.io/aminvakil/fedora34-systemd`
- `quay.io/aminvakil/fedora33-systemd`
- `quay.io/aminvakil/fedora32-systemd`

Some repositories using these images to test:

- [aminvakil/lamp](https://github.com/aminvakil/lamp)
- [aminvakil/ansible-role-docker-installation](https://github.com/aminvakil/ansible-role-docker-installation)
- ...

If you're interested just fork [aminvakil/ansible-role-template](https://github.com/aminvakil/ansible-role-template) and put your tasks in it, molecule tests and github actions are taken care of.
