# LXQt remote desktop Docker image powered by Manjaro and xrdp

This repository provides the Docker image that runs an [LXQt](https://lxqt-project.org)
desktop on top of [Manjaro Linux](https://manjaro.org). The desktop is accessible via
[RDP (Remote Desktop Protocol)](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol)
clients such as [Remmina](https://remmina.org/), [FreeRDP](https://www.freerdp.com),
[Microsoft Remote Desktop](https://www.microsoft.com/en-us/p/microsoft-remote-desktop/9wzdncrfj3ps)
([for macOS](https://itunes.apple.com/app/microsoft-remote-desktop/id1295203466)).

Interested in GNOME? See [`manjaro-xrdp-gnome`](https://github.com/trustin/manjaro-xrdp-gnome).

## Step 1: Pull the image

Pull the Docker image from `ghcr.io/trustin/manjaro-xrdp-lxqt:latest`.

```shell
docker pull ghcr.io/trustin/manjaro-xrdp-lxqt:latest
```

## Step 2: Create a new container from the pulled image

Create a new container like the following. Note that `--privileged` option is required.

```
docker create \
  --name manjaro-xrdp-lxqt \
  --env "LANG=en_US.UTF-8" \
  --env "TZ=America/Los_Angeles" \
  --env "PUSER=user" \
  --env "PUID=1000" \
  --env "PASSWORD=password" \
  --tty \
  --interactive \
  --privileged \
  --shm-size 2G \
  --publish 23389:3389 \
  --publish 8022:22 \
  ghcr.io/trustin/manjaro-xrdp-lxqt:latest
```

### Parameters

| Parameter | Example | Function |
| :----: | --- | --- |
| PUSER | john | The username of the desktop user (default: `user`) |
| PUID | 1000 | The nummeric user ID of the desktop user (default: `1000`) |
| PASSWORD | secret | The initial login password of the desktop user (default: same as `$PUSER`) |
| TZ | Asia/Seoul | System timezone (default: `America/Los_Angeles`) |
| LANG | en\_US.UTF-8 | System locale (default: `en_US.UTF-8`) |

## Step 3: Start the container.

```
docker start manjaro-xrdp-lxqt
```

## Step 4: Connect to the desktop.

You should now be able to access your full-featured LXQt desktop using
the RDP client of your choice. For example, using [Remmina](https://remmina.org):

```
remmina -c rdp://127.0.0.1:23389
```

## Customizing and building the image

Clone this repository, edit `Dockerfile` and then run `docker build` as usual:

```
docker build --tag 'custom-manjaro-xrdp-lxqt:latest' .
```

### Specifying an alternative mirror

US mirrors are used by default to fetch the packages. You can specify the
`MIRROR_URL` build argument to overide:

```
docker build --tag 'custom-manjaro-xrdp-lxqt:latest' \
  --build-arg "MIRROR_URL=https://repo.ialab.dsu.edu/manjaro/" .
```

### Invalidating cache

Use `--no-cache` option:

```
docker build --tag 'custom-manjaro-xrdp-lxqt:latest' --no-cache .
```

## License

This repository is licensed under [Apache License 2.0](https://tldrlegal.com/license/apache-license-2.0-(apache-2.0)).
