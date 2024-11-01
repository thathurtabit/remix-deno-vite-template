# NOTES

This is a fork from the
[remix-run/templates/classic-remix-compiler/deno](https://github.com/remix-run/remix/tree/6edd56211c5b256e2e78f781695fdb39a037463e/templates/classic-remix-compiler/deno)
template which modernizes it to use Remix v2 with Vite and no dependency on
Node.js, only [Deno](https://deno.com/).

This template has been tested with Deno v2.0.0+.
[Live deployment here](https://huge-badger-89.deno.dev/) hosted on
[Deno Deploy](https://deno.com/deploy).

## Using this template

Run the following command:

```zsh
deno run -A npm:create-remix@latest --no-install --template redabacha/remix-deno-vite-template
```

And then run `deno install` in the created directory.

## Using JSR and HTTPS imports

Thanks to the [@deno/vite-plugin](https://github.com/denoland/deno-vite-plugin),
it's possible to use packages from JSR and imports from HTTPS URLs (via the
[deno.json `imports` field](https://docs.deno.com/runtime/fundamentals/modules/#managing-third-party-modules-and-libraries))
within the `app/` directory which will get included in the server and/or browser
bundles as needed. This currently has a caveat where you need to manually
declare the imports using the `declare module` syntax as
[seen here](./app/env.d.ts#L11) in order to convince the Typescript type checker
it exists. This isn't necessary if the Deno type checker is used for the entire
project.

## Using the Deno type checker for the entire project

Currently the VSCode Deno extension has an issue with auto-import suggestions
for npm packages which makes not fully ideal for development at the moment, see
[this issue](https://github.com/denoland/vscode_deno/issues/1177) for more
information. However
[here's a working branch](https://github.com/redabacha/remix-deno-vite-template/tree/using-deno-typechecker)
that uses only the Deno type checker for all files with no direct dependency on
typescript.

# Remix + Deno

Welcome to the Deno template for Remix! 🦕

For more, check out the [Remix docs](https://remix.run/docs).

## Managing dependencies

- ✅ You should use `deno add` to add packages
  ```sh
  deno add npm:react
  ```
  ```ts
  import { useState } from "react";
  ```
- ✅ You may use inlined URL imports, JSR imports or NPM imports.
  ```ts
  import { pascalCase } from "https://deno.land/x/case/mod.ts";
  ```
- ✅ You may use Deno and Node built-ins on the server side.
  ```ts filename=app/entry.server.tsx
  Deno.env.get("DENO_DEPLOYMENT_ID");
  ```
  ```ts filename=app/entry.server.tsx
  import fs from "node:fs";
  ```

## Development

From your terminal:

```sh
deno task dev
```

This starts your app in development mode, rebuilding assets on file changes.

## Typegen

Generate types to use Deno APIs in server-only Remix code within `app/`:

```sh
deno task typegen
```

You should rerun typegen whenever you upgrade Deno version.

## Production

First, build your app for production:

```sh
deno task build
```

Then run the app in production mode:

```sh
deno task start
```

## Deployment

Building the Deno app (`deno task build`) results in two outputs:

- `build/server` (server bundle)
- `build/client` (browser bundle)

You can deploy these bundles to any host that runs Deno, but here we'll focus on
deploying to [Deno Deploy](https://deno.com/deploy).

### Setting up Deno Deploy

1. [Sign up](https://dash.deno.com/signin) for Deno Deploy.

2. [Create a new Deno Deploy project](https://dash.deno.com/new) for this app.

3. Replace `<your deno deploy project>` in the `deploy` script in `deno.json`
   with your Deno Deploy project name:

```json filename=deno.json
{
  "tasks": {
    "deploy": "deployctl deploy --prod --include=deno.json,deno.lock,build,server --project=<your deno deploy project> ./server.production.ts"
  }
}
```

4. [Create a personal access token](https://dash.deno.com/account) for the Deno
   Deploy API and export it as `DENO_DEPLOY_TOKEN`:

```sh
export DENO_DEPLOY_TOKEN=<your Deno Deploy API token>
```

You may want to add this to your `rc` file (e.g. `.bashrc` or `.zshrc`) to make
it available for new terminal sessions, but make sure you don't commit this
token into `git`. If you want to use this token in GitHub Actions, set it as a
GitHub secret.

5. Install the Deno Deploy CLI,
   [`deployctl`](https://github.com/denoland/deployctl):

```sh
deno install -Arfg jsr:@deno/deployctl
```

6. If you have previously installed the Deno Deploy CLI, you should update it to
   the latest version:

```sh
deployctl upgrade
```

7. If you haven't already, run the `build` task to create production build:

```sh
deno task build
```

### Deploying to Deno Deploy

After you've set up Deno Deploy, run:

```sh
deno task deploy
```
