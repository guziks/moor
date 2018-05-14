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

## Shell

Before installing commands, it is convenient to install shell and explore what it is in that image. Quick way to install shell:

```
$ moor <image>
```

This will create launcher `<image>-shell` which will run `bash`. If an image does not have `bash`, you can always install any shell in the usual way, for example:

```
$ moor node:alpine node-shell ash 
```

## Portability

Once created, launchers have no dependencies but docker, so they could be copied to other systems and they'll work just fine.

## File system

Installed commands run inside a container, so file system access is limited. This directories are available:

* current working directory (determined using `$PWD`)
* directory specified by `$MOORHOME` (by default: `~/.moor/home`)

## Network

On Linux docker network type `host` is used, so all ports are automatically available. On Mac this type of network is not available, so you need to explicitly make port forwarding while launching installed command:

```
$ P="-p <port>:<port> [-p <port>:<port> ...]" <cmd> <args>
```

For example:

```
$ P="-p 8080:8080" python -m http.server 8080
```

Notice how docker ports syntax used inside `P` variable.

## Docker run

To supply any additional docker run options use `MOORRUN` variable, for example:

```
$ MOORRUN="--env URL=http://example.com" node app.js
```

There must be no spaces inside variables supplied this way.

**TIP:** If you need environment variables with spaces inside, use `MOORRUN="--env-file <file-name>"`.

## Troubleshooting

If installed command or even shell does not work as expected, try to install the command with one or several of these options:

1. `--keep-user`
2. `--keep-home`
3. `--privileged`
4. `--reset-entrypoint`

If `--keep-user` helped then consider installing the command with both `--keep-user` and `--chown` options. See `moor install --help`.