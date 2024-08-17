# docker-os
Multiple OSs Docker containers (systemd included).

Can be used for molecule testing.

Distros available:

- `quay.io/aminvakil/debian12-systemd`
- `quay.io/aminvakil/debian11-systemd`
- `quay.io/aminvakil/debian10-systemd`
- `quay.io/aminvakil/ubuntu24.04-systemd`
- `quay.io/aminvakil/ubuntu22.04-systemd`
- `quay.io/aminvakil/ubuntu20.04-systemd`
- `quay.io/aminvakil/fedora-systemd`

Fedora image is using the latest Fedora release (currently 38).

Some repositories using these images to test:

- [aminvakil/lamp](https://github.com/aminvakil/lamp)
- [aminvakil/ansible-role-docker-installation](https://github.com/aminvakil/ansible-role-docker-installation)
- ...

If you're interested just fork [aminvakil/ansible-role-template](https://github.com/aminvakil/ansible-role-template) and put your tasks in it, molecule tests and github actions are taken care of.
