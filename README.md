# My simple NAS media stack

I am posting this publicly in case someone might find it as a good reference. This
probably won't work for others without major/minor tweaks to services, etc. A
media / nas server is a very personal system in my opinion.

There might be README's in the subdirectories, have a look at them as well
for any specifics or notes or feelings.

## Pre-requisites

Ideally, you will be familiar with linux and docker usage. Heck, maybe even windows would
work with some tweaks. I also assume you have your system configured, timezone, packages,
etc before embarking on services.

I setup my own storage with ZFS. It's great. For good performance make sure your big
torrent downloads and your media serving happen from the same filesystem. This will
make cataloging your media with the *arr's much faster as they can use hardlinks.

I use a storage pool mounted at /lake (somebody else used that and I liked the name).

### Create media group and user

See the example variables in `env.example` for command reference. You'll want system users
with very limited access. Copy `env.example` to `.env` in the top level of this repo and
change the variables to fit your situation.

Media and download services should be in the shared system group (I call mine media).

## Setup filesystem

You'll see in my `docker-compose.yml` files how I lay everything out. But for nearly
every service there is a `/lake/media-server-configs/<service>` folder for configuration
storage and that is typically owned by the service user and media group. UMASK 002
will ensure newly created files are owned and writable by both user and group members
which is what we generally need. Not so much for configuration, but for sharing
files between containers.

## Deploying

Setup the user you SSH as to be in the `docker` group or similar.
Then when you run `docker-compose` you'll first set the host:

```
export DOCKER_HOST="ssh://<media.server>"
```

Run a `docker ps` or something similar, if all goes well and you have passwordless
authentication going you'll see the containers of the remote system. Ensure you have
a `.env` file, and deploy with `docker-compose up -d`.

## Firewall

I always run a firewall, even internally. I don't bother with ssl but it wouldn't be hard
to add to the traefik container. Setup `ufw` or whatever to allow ports 22, 80, 443 and
whatever else you want to expose.

You'll notice the gluetun container is bound to the loopback `127.0.0.1` instead of
`0.0.0.0`. That's intentional as docker iptables rules take precedence over ufw and
if you bind a container to a host port with docker ufw won't be able to block it.

## Everything works?

Enjoy! Setup links to your services in `apps.${DOMAIN}` and make that
your homepage.
