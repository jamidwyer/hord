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

## Notes

- If you run into a migration error involving content types, you can remove the postgres volumes with `docker compose down && docker volume rm hord_postgres-data`.
- You can change DEBUG=1 in .env to troubleshoot Django.