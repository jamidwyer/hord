# hord

CMS to track personal and community food resources, in hopes of reducing food waste while keeping us all fed! 

I do not plan to make a profit on this labor of love, so if you're a hippie nerd like me, pull requests very gratefully accepted.

Currently very close to ready for some test users!

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
[Deployment setup](deployment_setup.md)

ssh into server.

Pull changes from this repo.

`docker compose up --build`

## Roadmap

### MVP

- /Fix accounts/profile 404 for non-admin
- Add note about signups soon to django login page
- Fix login page
- Redirect from Django to inventory after login
- If redirect still a no on expected ports, big giant link to inventory post-login page
- Test and fix show stoppers
- test restoring from backups
- Add signup link to django login page

### Nice to have

- make django part look better -- css submodule?
- Allow trusted users to work within CMS
- establish user trust rules
- figure out how to scale data before server costs bankrupt me
- make backup before merging server code
- upload backups to s3
- lose REST or build it out
- move not-found out of recipes
- different nginx for prod and dev
- recipes a completely separate app
- mock data
- activitypub integration?
- https://whitenoise.readthedocs.io/en/stable/
- non-docker server dev
- automate ssl renewal
- clean up nginx config
- clean up submodule code made redundant by unifying here
- aws secret manager
- adrs https://github.com/npryce/adr-tools
