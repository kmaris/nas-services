# Media Services

Choose a root, for me it's `/lake`. Setup the user and group ownership:

Setup UMASK so that the `media` group has as much access as the owning user.
This is OS-specific, but generally something in `/etc` and we want
the `0022` mask.

```bash
chown -R root:media /lake/media # /lake/downloads etc...
find /lake -type d -exec chmod 775 {} \;
find /lake -type f -exec chmod 664 {} \;
```

Create media group and users:

```bash
addgroup --system --gid 321 media # make gid whatever you want.
adduser --system --gid media sonarr
# and so on: lidarr, radarr, etc...
```

