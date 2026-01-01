# Container

Both `docker` and `podman` are containerization tools. The docker image is hosted at [https://hub.docker.com/r/curlimages/curl](https://hub.docker.com/r/curlimages/curl)

You can run the latest version of curl with the following command:

Command for `docker`:

[PRE0]

Command for `podman`:

[PRE1]

## Running curl seamlessly in container

It is possible to make an alias to seamlessly run curl inside a container as if it is a native application installed on the host OS.

Command to define curl as an alias for your containerization tool in the Bash, ZSH, Fish shell:

### Bash or zsh

Invoke curl with `docker`:

[PRE2]

Invoke curl with `podman`:

[PRE3]

### Fish

Invoke curl with `docker`:

[PRE4]

Invoke curl with `podman`:

[PRE5]

And simply invoke `curl www.example.com` to make a request

## Running curl in kubernetes

Sometimes it can be useful to troubleshoot k8s networking with curl, just like :

[PRE6]