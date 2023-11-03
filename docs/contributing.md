# Contributing

## Process

If you want to contribute and don't know where to start, here is a step-by-step for a complete lifecycle, from idea to deployment.

1. If you have an idea that is not already in [discussion](https://github.com/webstudio-is/webstudio/discussions) or [issues](https://github.com/webstudio-is/webstudio-builder/issues), please start a discussion or reach out on [discord](https://discord.gg/UNdyrDkq5r)

1. We welcome everyone, developers and non-developers.

   - If you are a DEVLOPER, and you want to contribute to the Builder UI, your first step is to become familiar with our design system in figma. Start with the first page of our [Design Docs](https://www.figma.com/file/xCBegXEWxROLqA1Y31z2Xo/%F0%9F%93%96-Webstudio-Design-Docs?type=design&node-id=234%3A36754&t=w3VxT162RQF0gTrI-1). And for our design system & components see the [Webstudio Library](https://www.figma.com/file/sfCE7iLS0k25qCxiifQNLE/%F0%9F%93%9A-Webstudio-Library?type=design&node-id=2647%3A10046&t=f4xr8mcumXfXkHVh-1).
   Please read the docs carefully and throughly so you can use our design system properly to create a UI that looks like it's a natural part of Webstudio. If any part of the design is confusing or intimidating you can reach out to [Taylor Nowotny](https://github.com/taylornowotny) with questions or feedback. I'm happy to give a tour of the design system to anyone who asks.

   - If you are a DESIGNER, you can browse our design files with the above links, you can share your ideas in any discussion, or if you would like to get more deeply involved you can reach out to [Taylor Nowotny](https://github.com/taylornowotny), our product design lead. I'm happy to give a tour of the design system to anyone who asks.

1. After submitting an enhancement or a bug we need to agree on a solution. You can jump into implementation right away, but be aware that we have to agree on the solution in the end. Communicating the problem you are solving and why you think the approach you are taking is the best is key for a quick process.

   In fact, we are happy to help communicating your ideas, so if you are not sure - talk to us on discord.

1. When the implementation is in a mergeable state, a core team member will deploy it to production.

## Builder installation

1. Install [Node.js](https://nodejs.dev/learn/how-to-install-nodejs)
2. Install [PNpm](https://pnpm.io/) `npm i -g pnpm`
3. Install Postgres, in here there are three main options:

   1. Install the [official Postgres app](https://www.postgresql.org/download/) - Make sure to remember the username and password you choose since you will need it for the connection string
   2. On a mac you can also install [Postgres.app](https://postgresapp.com/) and in that case you will have no password and your username will be the same as your system username.
   3. Intall [Docker Engine](https://docs.docker.com/engine/install/), run `docker compose -f ./apps/builder/docker-compose.yaml up -d` to start Postgres _(use PGPORT environment variable in case of default is already used)_.

4. Clone the repository `git clone git@github.com:webstudio-is/webstudio-builder.git`
5. Connect to the [database](https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases/connect-your-database-typescript-postgres): add a database URL to the env variables by creating an .env file in the `apps/builder` and adding there `DATABASE_URL=postgresql://user:pass@localhost/webstudio` or `DATABASE_URL=postgresql://user@localhost/webstudio` if using Postgres.app.
6. Run `pnpm install` - install dependencies
7. Run `pnpm build`
8. Run `pnpm migrations migrate` - apply database migrations
9. Run `pnpm dev` - URL will be logged

## Creating database

If you are configuring the app for the first time. Create the database before running migrations.

```sh
> CREATE DATABASE webstudio;
```

## Login locally

When downloading the app you will have two options for login and they both work in different ways.

### Dev login

Dev login will create you a dev account so you don't need to be online or create a github app.
To enable you must have two lines in your `.env`:

```
DEV_LOGIN=true
AUTH_SECRET=a random value
```

To login in the app click on the "Dev Login" button and paste the **first four character** of your `AUTH_SECRET` in the input.

### GitHub Login

To login and use GitHub login you will need to create an OAuth app with the following values:

```
Name: Webstudio
Homepage URL: http://localhost:3000
Authorization callback URL: http://localhost:3000/auth/github/callback
```

> When creating the app with `http://localhost:3000` you will need to always open your app at `http://localhost:3000` for the GitHub login to work.

After the app is created you will need to create a client secret and then copy that value and your client id to the `.env` file:

```
GH_CLIENT_SECRET=
GH_CLIENT_ID=
```

You are done! 🎉

## Develop builder and SDK in parallel

For this we current use [relative-deps](https://github.com/mweststrate/relative-deps) and the way you can run both things is the following:

- Git clone the sdk at https://github.com/webstudio-is/webstudio-sdk
- In the builder run `pnpm dev` and in another window run `pnpm watch:sdk`

This will trigger a build of the SDK whenever make changes to it and that will in turn reload your builder.

## Builder deployment to Vercel

1. [import your Git repository](https://vercel.com/new) into Vercel, and it will be deployed.
2. Add DATABASE_URL to env variables
3. Redeploy

If you'd like to avoid using a Git repository, you can also deploy the directory by running [Vercel CLI](https://vercel.com/cli):

```sh
npm i -g vercel
vercel
```

It is generally recommended to use a Git repository, because future commits will then automatically be deployed by Vercel, through its [Git Integration](https://vercel.com/docs/concepts/git).

## Release new version of a package

Follow this guide if you would like to release a new version one of our package available to npm.

The flow looks like the following:

1. Modify the code in a branch other than `main`.
2. Every commit on any branch builds and publish to npm with `--dry-run` options.
3. If everything looks fine, bump the version in `package.json`, open a PR and if it's approved, merge it.
4. Every commit on `main` will try publish to npm in case of any files or release process (GitHub Action) changed inside the package directory.
