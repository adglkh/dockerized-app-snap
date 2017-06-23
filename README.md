# Dockerized app snap

A dockerized app snap that demonstrates how to launch services by utilizing docker or docker compose command shared by docker snap. This is a proof of concept snap where I show snaps as a delivery mechanism for dockerized applications.
It's still under development and is subject to change.

## Snap

If you would like to build dockerized app snap, please make sure
you have snapd(> 2.25) and snapcraft(2.26) packages installed firstly.

```
sudo apt-get install snapd snapcraft
sudo snap install core
```

If you play around it on Ubuntu Core, you might as well refresh both core(> 1804) and pc-kernel(>67) snap to very latest version.
Otherwise you may encounter [problems](https://forum.snapcraft.io/t/tmux-screen-on-ubuntu-core/499/15) when launching dockerized services.

```
sudo snap refresh core --candidate
sudo snap refresh pc-kernel --edge
```

Then you need to 
1.checkout this branch [content-interface](https://github.com/docker/docker-snap/tree/content-interfaces) from docker-snap repo.
2.checkout master branch from dockerize app snap repo.
run the following command respectively under project root folder to generate two snap packages.

```
$ snapcraft
```

After it's done, please run the following commands to install them locally. 

```
$ sudo snap install --dangerous docker_[VER]_[ARCH].snap
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
```

## Launch services

Upon installation and relevant plugs are connected, for demonstration purposes, 
we can manage to launch two dockerized services by running the following command.
These two services are up and running by invoking docker-compose command from docker-app sandbox 
with content-interface connected.

```
$ sudo docker-app.ubuntu
```
```
$ sudo docker-app.redis-hello-world 
```
