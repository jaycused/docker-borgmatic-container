# Borgmatic Docker container
[![](https://images.microbadger.com/badges/version/monachus/borgmatic:v2.1.1.svg)](https://microbadger.com/images/monachus/borgmatic:v2.1.1 "Get your own version badge on microbadger.com")

[Borg](https://borgbackup.readthedocs.io/) and [borgmatic](https://torsion.org/borgmatic/) in an Alpine container for your backup needs.

To see which version of borg this container is using, check this page: https://pkgs.alpinelinux.org/packages?name=borgbackup and choose the highest `vx.x` available (i.e. not edge). Or run this container with `docker pull monachus/borgmatic && docker run --rm monachus/borgmatic borg --version`.

Docker image tags follow the borgmatic version, with `latest` corresponding to the `master` branch of the repository.

# Usage

Typically, you'd want to use this image as the base image for your backup container and add a volume for your ssh keys, your known_hosts file, and you backup directories.

```
FROM monachus/borgmatic
VOLUME /data
VOLUME /root/.ssh
VOLUME /borgmatic
CMD ["-c", "/borgmatic/config.yaml"]
```

You would then run it with `docker run --rm -v data_volume:/data:ro -v ssh_volume:/root/.ssh:ro -v borgmatic_config:/borgmatic:ro monachus/borgmatic`.

The ssh_volume should contain the ssh keys to connect to the remote borg repo, and a known_hosts file to avoid the interactive message whether to accept the new ssh key on connecting to a remote host.

The image uses an entrypoint, so runtime command args are passed directly to borgmatic:

```
docker run --rm monachus/borgmatic --help
```

If the first argument does not start with a `-`, then it is called as an executable:

```
docker run --rm monachus/borgmatic borg --version
```

# Issues

Please open an issue describing the problem or requested feature. PRs are welcome.
