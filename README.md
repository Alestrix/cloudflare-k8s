# cloudflare-k8s
Cloudflare-config to forward my websites to their k8s services

## What?
This repo contains two ways of exposing the domains I'm running to the cloudflare tunnel:
- using the token
- using a credentials file

## What's the difference?
The token version is simpler to setup, but whoever gets hold of the token can fully manage all your account's tunnels.
This includes deleting existing tunnels and creating new tunnels for your domains.

The creds file version needs an extra step to create the credentials file. This is done by first logging in to
your cloudflare account using `cloudflared tunnel login`, then you can either create a new tunnel via `cloudflared tunnel create <NAME>`
(which creates the credentials file), or, if you already have a tunnel, retrieve the credentials file of the tunnel named `<NAME>` via
`cloudflared tunnel --cred-file <PATH> <NAME>`.

If you want to use docker (as I did) instead of installing cloudflared on your computer, don't forget to persist the certificate
and credentials files created by the container by adding an appropriate bind mount (files are written to `/home/nonroot/.cloudflared/` inside
the container) to your `docker run` command and allow the container (runs with UID 65532 by default) to write to it:
```
mkdir creds
chmod o+w creds
chmod g+s creds
docker run --rm -v ./creds/:/home/nonroot/.cloudflared/ cloudflare/cloudflared:latest tunnel login
docker run --rm -v ./creds/:/home/nonroot/.cloudflared/ cloudflare/cloudflared:latest tunnel token --cred-file /home/nonroot/.cloudflared/creds.json <TUNNEL-UUID or NAME>
sudo chmod g+r creds/*
```

Todo: describe how to configure (via web) the argo tunnel forwarding when using credentials files.

## Which sites?
So far I run these sites in a very competitively priced [rackspace spot](https://spot.rackspace.com/) kubernetes cluster and expose them via a cloudflare tunnel:
- istesdns.de:
  - URL: <https://istesdns.de>
  - Repo: <https://github.com/Alestrix/istesdns.de> (see subdir `k8s`)
- xy-problem.de:
  - URL: <https://xy-problem.de>
  - Repo: <https://github.com/Alestrix/xyproblem.de> (see subdir `k8s`)
