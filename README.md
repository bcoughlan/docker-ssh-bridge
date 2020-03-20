# docker-ssh-bridge

Run a command against a remote Docker daemon by tunneling over SSH.

# Use cases

Since Docker 18.09, the Docker CLI has supported using a remote Docker Daemon over SSH by setting `DOCKER_HOST=ssh://user@host`. It is usually much faster to build the image remotely than to build it locally and copy it with `docker save` and SSH.

However, some tools (build plugins etc.) do not yet support the ssh protocol. This offers an alternative until it is widely available.

# Installation

```
wget -q https://raw.githubusercontent.com/bcoughlan/docker-ssh-bridge/master/docker-ssh-bridge -O/usr/local/bin/docker-ssh-bridge && chmod +x /usr/local/bin/docker-ssh-bridge
```

# Usage

```
docker-ssh-bridge <ssh arguments e.g. <private key> user@host -- <command to execute>
```

e.g.

```
docker-ssh-bridge ~/.ssh/key.pem user@host -- docker build -f Dockerfile .
```

The SSH user must be a member of the `docker` group as described here: https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user

# How it works

1. Create an SSH port forward between the remote `docker.sock` UNIX socket and a temporary file.
2. Set `DOCKER_HOST` to point at the temporary file.
3. Run the supplied command.
4. Cleans up.
