# Deployment setup

I'm not an ops person, but I like to build stuff, so here goes. Ideally, this would all be done with GitHub Actions, but for now, we're SSHing into stuff.

## Update submodules

Make sure both the client and server submodules reflect the changes you'd like to deploy.

## Set up a server

Amazon EC2 Linux has worked but oof it's expensive.

Trying Linode Ubuntu 24. https://www.youtube.com/watch?v=KEK-ZxrGxMA

Add an SSH key from the computer you're using.

## SSH in

ssh username@ip.add.re.ss

## Install Docker

https://docs.docker.com/engine/install/ubuntu/

## Connect your server to GitHub

Make another SSH key for the server. https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

`ssh-keygen -t ed25519 -C "your_email@example.com"`

`eval "$(ssh-agent -s)"`

`ssh-add ~/.ssh/id_ed25519`

Copy the output of the followwing:
`cat ~/.ssh/id_ed25519.pub`

Add it to GitHub by clicking your profile picture, Settings, SSH & GPG Keys.

`git clone git@github.com:jamidwyer/hord.git`

`git submodule update --init`

## One-time setup

`cp env_example .env`

Change the contents of .env as needed.

Add the IP address of your server to ALLOWED_HOSTS in server/cms/settings.py.

`cp client/env-example client/.env`

TEMPORARILY uncomment # python manage.py flush --no-input in server/entrypoint.sh.

Comment that line back out, because it wipes the database.

`docker compose down && docker volume rm hord_postgres-data && docker compose up --build`

Create a superuser:

`docker exec -it hord_server python manage.py createsuperuser`

## SSL (Let's Encrypt / certbot)

DNS for `hord.food` (and `www.hord.food`) must already point at the server before doing this, since Let's Encrypt verifies ownership over HTTP.

`docker-compose.yml` defines a `certbot` service that renews certs automatically every 12h (a no-op unless a cert is within 30 days of expiry), and the `nginx` service reloads itself every 12h to pick up renewed certs. Certs live on the host at `/etc/letsencrypt`, bind-mounted into both containers.

The 443 server block in `nginx/conf.d/nginx.conf` points at `/cert/live/hord.food/fullchain.pem`. nginx refuses to start if that file doesn't exist, so on a brand-new domain/server (or the first time you deploy this config) you have to get the cert before nginx can start with it:

1. Stop nginx so port 80 is free: `docker compose stop nginx` (brief downtime).
2. Get the first cert with certbot's standalone plugin, temporarily publishing port 80 for the one-off run:

   ```sh
   docker compose run --rm --publish 80:80 certbot certonly --standalone \
     -d hord.food -d www.hord.food \
     --email you@example.com --agree-tos --no-eff-email
   ```

3. `docker compose up -d` — nginx starts with the cert now in place, and the `certbot` sidecar starts its renewal loop.

No need to touch `nginx.conf` for this — it's already checked in with the 443 block enabled.

After that, renewal is automatic — nothing more to do. If you ever need to force a renewal check manually: `docker compose exec certbot certbot renew`.

## Notes

- If you run into a migration error involving content types (locally only, not in production!), you can remove the postgres volumes with `docker compose down && docker volume rm hord_postgres-data`.
- You can change DEBUG=1 in .env to troubleshoot Django.
