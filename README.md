## About

Package manager that makes commands from docker image be available on the host.

## Install

Prerequisites: `docker`, `bash`, `curl`, `grep`.

Download `moor` and put it in your `PATH`, everything else is optional. Recommended command:

```
$ curl -o moor https://raw.githubusercontent.com/guziks/moor/master/moor && \
chmod +x moor && \
MOORBIN=~/.moor/bin && \
mkdir -p $MOORBIN && \
mv moor $MOORBIN/ && \
export PATH=$MOORBIN:$PATH && \
echo -e "\n# Added by moor" >> ~/.bashrc && \
echo "export MOORBIN=$MOORBIN" >> ~/.bashrc && \
echo 'export PATH=$MOORBIN:$PATH' >> ~/.bashrc
```

## Upgrade

If you installed using previous command, use this to upgrade:

```
$ curl -o moor https://raw.githubusercontent.com/guziks/moor/master/moor && \
chmod +x moor && \
mv moor $MOORBIN/
```

## Usage

Lets imagine you want to use `asciidoctor-pdf` application, but do not want to install all dependencies. Luckily there is an image for that. Here is how it can be installed with moor:

```
$ moor asciidoctor/docker-asciidoctor ad-pdf asciidoctor-pdf
```
 
This will create small launcher `~/.local/bin/ad-pdf` and finish immediately. This way `asciidoctor-pdf` is now awailable on the host as `ad-pdf`. As soon as `ad-pdf` is launched, docker image will get downloaded and `asciidoctor-pdf` will get run inside a container. Container runs with `--rm` option, so when `asciidoctor-pdf` exits, container is removed.

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
