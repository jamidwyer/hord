# hord

Inventory tracking. Work in progress.

## Dev

`git submodule init && git submodule update`

`docker compose up --build`

If things go right, you'll see the following:

- a NextJS frontend at http://localhost
- Django admin at http://localhost/admin
- Swagger docs at http://localhost/api/docs
- GraphQL at http://localhost/graphql

Create a superuser:

`docker exec -it hord_server python manage.py createsuperuser`

## Deploy

Pull changes.

`docker compose up --build`

## Roadmap

### MVP

- log user out in django from fe

### Nice to have

- move not-found out of recipes
- different nginx for prod and dev
- recipes a completely separate app
- mock data
- https://whitenoise.readthedocs.io/en/stable/
- non-docker server dev
- automate ssl renewal
- clean up nginx config
- clean up submodule code made redundant by unifying here
- aws secret manager
- adrs https://github.com/npryce/adr-tools
