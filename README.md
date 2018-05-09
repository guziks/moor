## About

Package manager that adds commands from docker images to the host.

## Install

Prerequisites: `docker`, `bash`, `curl`, `grep`.

```
$ eval "$(curl -fsSL https://raw.githubusercontent.com/guziks/moor/master/install)"
```

## Update

```
$ moor selfupdate
```

## Usage

Lets imagine you want to use `asciidoctor-pdf` application, but do not want to install all dependencies. Luckily there is an image for that. Here is how it can be installed with moor:

```
$ moor asciidoctor/docker-asciidoctor ad-pdf asciidoctor-pdf
```
 
This will create small launcher `$MOORBIN/ad-pdf` and finish immediately. This way `asciidoctor-pdf` is now awailable on the host as `ad-pdf`. As soon as `ad-pdf` is launched, docker image will get downloaded and `asciidoctor-pdf` will get run inside a container. Container runs with `--rm` option, so when `asciidoctor-pdf` exits, container is removed.

Updating is a breeze:

```
$ moor up ad-pdf
```

When you no longer need an app, just:

```
$ moor rm ad-pdf
```

Launcher and corresponding docker image will be removed.

For more usage information see:

```
$ moor --help
```

## Specificities

### File system

Installed commands run inside a container, so file system access is limited. This directories are available:

* current working directory (determined using `$PWD`)
* directory specified by `$MOORHOME` (by default: `~/.moor/home`)

### Network

On Linux docker network type `host` is used, so all ports are automatically available. On Mac this type of network is not available, so you need to explisitly make port forwarding while launching installed command:

```
$ P="-p <port>:<port> [-p <port>:<port> ...]" <cmd> <args>
```

For example:

```
$ P="-p 8080:8080" python -m http.server 8080
```

Notice how docker ports syntax inside `P` variable.

### Signals

`CTRL-C` works only for processes which explicitly react on `SIGINT`. If `CTRL-C` does not work for a command then it can be stopped with `CTRL-\` (`SIGQUIT` signal). The last resort:

```
$ docker ps # look for container name
$ docker rm -f <container name>
```
