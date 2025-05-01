# Torrent setup

I download / seed a lot of OS iso's, and like using a vpn to learn
about docker networking.

Once these containers are up, you'll need to set the port in the
transmission web gui.

First, get the forwarded vpn port:

```
docker exec -ti transmission-vpn cat /tmp/gluetun/forwarded_port
```

It'll give you a number like 41686 or something. In the
transmission web gui, open 'Edit preferences' from the hamburger
menu. In the network tab you'll set the `Peer Listening Port`
to the number you got from the container.

If the container is restarted or recreated you'll need to re-do
this process as the port will change. I have it in my backlog
to automate this at some point.