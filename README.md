# KiCad 7 docker
There is no standard way to install and run multiple kicad versions on the same host.
Docker is one way to get around this. The dockerfile is mostly taken from
[devbisme's repo](https://github.com/devbisme/docker_kicad).

# Setup
1. Clone repository
2. Build it
``` bash
docker build \
    --build-arg UID=`id -u` \
    --build-arg GID=`id -g` \
    --build-arg USER_NAME=`id -nu` \
    --build-arg HOME=$HOME \
    -t kicad7 .
```

# Run it
The following command runs the docker container by forwarding the host's graphics capabilities (assuming
[integrated intel graphics](https://www.reddit.com/r/docker/comments/10dsn6p/how_do_you_pass_amd_gpus_and_intel_gpus_through/) and the host's home folder.

``` bash
docker run --device /dev/dri --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v $HOME:$HOME kicad7"
```

A tip is to add an alias (e.g. alias kicad7=...) for this command in your bash config.

# Note
* In later KiCad versions the standard KiCad library is installed along with KiCad, so it's available under
__/usr/share/kicad__ inside the docker container.
* If KiCad complains about OpenGL support it means GPU is not forwarded properly.
* If the same KiCad version is installed on your host computer, it's going to use the same local
configuration (.config/kicad).

# TODO
* Try to lift this to a later Ubuntu version
