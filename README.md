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
(which stores the file into the default cloudflared directory), or, if you already have a tunnel, retrieve the
credentials file via `cloudflared tunnel --cred-file <PATH> <NAME>`.

If you want to use docker (as I did) instead of installing cloudflared on your computer, don't forget to persist the certificate
and credentials files created by the container by adding an appropriate `-v ./somepath/:/home/nonroot/.cloudflared/` to your
`docker run` command.

## Which sites?
- istesdns.de:
  - URL: <https://istesdns.de>
  - Repo: <https://github.com/Alestrix/istesdns.de> (see subdir `k8s`)
- xy-problem.de:
  - URL: <https://xy-problem.de>
  - Repo: <https://github.com/Alestrix/xyproblem.de> (see subdir `k8s`)
