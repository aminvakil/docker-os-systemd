# docker-os-systemd

Docker images for testing Linux services with `systemd` inside containers.

The images are intended for CI and Molecule/Ansible role testing where a regular distro container is not enough because services need to be started and managed by `systemd`.

## Available images

Images are published to Quay:

| Distribution | Image |
| --- | --- |
| Debian 13 | `quay.io/aminvakil/debian13-systemd` |
| Debian 12 | `quay.io/aminvakil/debian12-systemd` |
| Ubuntu 24.04 | `quay.io/aminvakil/ubuntu24.04-systemd` |
| Ubuntu 22.04 | `quay.io/aminvakil/ubuntu22.04-systemd` |
| Fedora | `quay.io/aminvakil/fedora-systemd` |

## Image contents

Each image includes:

- `systemd`
- Python 3 packaging tools used by common test frameworks (`pip`, `setuptools`, `wheel` where available)
- `sudo`

Current Dockerfiles use these base images:

| Dockerfile | Base image | Init command |
| --- | --- | --- |
| `images/Dockerfile.debian13` | `debian:trixie-20260421` | `/lib/systemd/systemd` |
| `images/Dockerfile.debian12` | `debian:bookworm-20260421` | `/bin/systemd` |
| `images/Dockerfile.ubuntu24.04` | `ubuntu:noble-20260410` | `/bin/systemd` |
| `images/Dockerfile.ubuntu22.04` | `ubuntu:jammy-20260410` | `/bin/systemd` |
| `images/Dockerfile.fedora` | `fedora` | `/usr/sbin/init` |

The Fedora image tracks the current default `fedora` image and removes several default `systemd` unit symlinks that are not useful in a container.

## Usage

Pull and run an image with the privileges and mounts needed by `systemd`:

```sh
docker pull quay.io/aminvakil/debian12-systemd

docker run --detach \
  --privileged \
  --cap-add=ALL \
  --volume /sys/fs/cgroup:/sys/fs/cgroup:ro \
  --volume /lib/modules:/lib/modules:ro \
  quay.io/aminvakil/debian12-systemd
```

For Molecule with the Docker driver, keep the image command enabled so `systemd` runs as PID 1, run the container privileged, and mount `/run` and `/tmp` as tmpfs. This works with cgroup v2 hosts.

Example `molecule/default/molecule.yml`:

```yaml
---
dependency:
  name: galaxy
driver:
  name: docker
platforms:
  - name: instance
    image: "quay.io/aminvakil/${MOLECULE_DISTRO}-systemd:latest"
    tmpfs:
      - /run
      - /tmp
    privileged: true
    pre_build_image: true
    override_command: false
provisioner:
  name: ansible
  env:
    ANSIBLE_PIPELINING: "True"
```

Run a scenario against one of the supported distro names:

```sh
MOLECULE_DISTRO=debian12 molecule test
MOLECULE_DISTRO=ubuntu24.04 molecule test
MOLECULE_DISTRO=fedora molecule test
```

The important Molecule settings are:

- `privileged: true`: allows `systemd` to manage services in the container.
- `override_command: false`: prevents Molecule from replacing the image entrypoint/command.
- `pre_build_image: true`: tells Molecule to use the published image as-is.
- `tmpfs: [/run, /tmp]`: provides writable runtime directories needed by `systemd`, especially on cgroup v2 hosts.

## Building locally

Build any supported distro from the matching Dockerfile:

```sh
docker build --no-cache --rm \
  --file images/Dockerfile.debian12 \
  --tag debian12-systemd:local \
  images
```

Then run it using the same privileged `systemd` container options shown above.

## CI/CD

GitHub Actions builds and tests every image on pull requests and pushes to `main`.

The workflow:

1. Builds each Dockerfile in the distro matrix.
2. Starts a privileged container from the built image.
3. Removes the container after a short startup check.
4. Runs Trivy vulnerability scanning.
5. On `main`, pushes the images to Quay as `quay.io/aminvakil/<distro>-systemd`.

## License

This project is licensed under GPL-3.0. See [LICENSE](LICENSE).
