# hord

Inventory tracking. Work in progress.

## Dev

`docker compose up --build`

If things go right, you'll see the following:

- a NextJS frontend at http://localhost
- Django admin at http://localhost/admin
- Swagger docs at http://localhost/api/docs

## Deploy

Pull changes.

`docker compose up --build`

## Roadmap

### MVP

- query django from nextjs

### Nice to have

- graphene
- persist data with rds
- other people can sign up
- different nginx for prod and dev
- https://whitenoise.readthedocs.io/en/stable/
- clean up static volumes
- move off amazon linux
- make server dev standalone
- make client dev standalone
- non-docker server dev
- non-docker client dev
- automate ssl renewal
- asdf
- clean up nginx config
- clean up submodule code made redundant by unifying here
