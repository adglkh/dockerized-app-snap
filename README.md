# Dockerized app snap

This snap aims to demonstrate how to build and run a dockerzied app on Ubuntu Core through the content-interface shared by the docker snap. So people could just simply ship a piece of Dockerfile or a docker-compose.yml into their snap package and invoke docker from a dockerized snap sandbox to build images and run a container. This is also a proof of concept snap where I show snaps as a delivery mechanism for dockerized applications. 

It's still under development and is subject to change.

**Note**
It's highly recommended not to use this method in untrusted apps due to security escape since docker-support interface grants people high privilege to access OS. This approach is a reasonable and effective compromise for trusted apps in a brand store. For more details, please check [this](https://forum.snapcraft.io/t/monax-snap-launching-docker-containers/1629/3) out.


## Snap

If you would like to build dockerized app snap, please make sure
you have snapd(> 2.25) and snapcraft(2.26) packages installed firstly.

```
sudo apt-get install snapd snapcraft
sudo snap install core
```

If you play around it on Ubuntu Core, you might as well refresh both core(> 1804) and pc-kernel(>67) snap to very latest version. Otherwise you may encounter [problems](https://forum.snapcraft.io/t/tmux-screen-on-ubuntu-core/499/15) when launching dockerized services.

```
sudo snap refresh core --candidate
sudo snap refresh pc-kernel --edge
```

To make it work, you need to install the docker snap from corresponding tracks in advance. We add the content-interface support starting from both docker snap version 17.03.2-ce-1 and 17.06.0-ce-1

```
sudo snap install --channel=17.03/stable docker
```
or install the 17.06.0-ce-1 from latest track by default
```
sudo snap install docker
```

Then you need to checkout master branch from dockerize app snap repo.
run the following command under project root folder to generate dockerized snap packages.

```
$ snapcraft
```

After it's done, please run the following commands to install the snap locally. 

```
$ sudo snap install --dangerous docker-app_[VER]_[ARCH].snap
```

And connect relevant plugs afterward.

```
$ snap connect docker:privileged :docker-support
$ snap connect docker:support :docker-support
$ snap connect docker:firewall-control :firewall-control
$ snap connect docker:network :network
$ snap connect docker:network-bind :network-bind
$ snap connect docker:docker-cli docker:docker-daemon
$ snap connect docker:home :home
$ snap disable docker
$ snap enable docker
$ snap connect docker-app:privileged :docker-support
$ snap connect docker-app:support :docker-support
$ snap connect docker-app:docker-cli docker:docker-daemon
$ snap connect docker-app:docker-executables  docker:docker-executables
```

## Launch services

Upon installation and relevant plugs are connected, for demonstration purposes, we can manage to launch two dockerized services by running the following command.  These two services are up and running by invoking docker-compose command from docker-app sandbox with content-interface connected.

```
$ sudo docker-app.ubuntu
```
```
$ sudo docker-app.redis-hello-world 
```

