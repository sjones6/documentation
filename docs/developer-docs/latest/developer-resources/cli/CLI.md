---
title: CLI - Strapi Developer Docs
description: Strapi comes with a full featured Command Line Interface (CLI) which lets you scaffold and manage your project in seconds.
canonicalUrl: https://docs.strapi.io/developer-docs/latest/developer-resources/cli/CLI.html
---

# Command Line Interface (CLI)

Strapi comes with a full featured Command Line Interface (CLI) which lets you scaffold and manage your project in seconds.

## strapi new

Create a new project.

```bash
strapi new <name>

options: [--no-run|--use-npm|--debug|--quickstart|--dbclient=<dbclient> --dbhost=<dbhost> --dbport=<dbport> --dbname=<dbname> --dbusername=<dbusername> --dbpassword=<dbpassword> --dbssl=<dbssl> --dbauth=<dbauth> --dbforce]
```

- **strapi new &#60;name&#62;**<br/>
  Generates a new project called **&#60;name&#62;** and installs the default plugins through the npm registry.

- **strapi new &#60;name&#62; --debug**<br/>
  Will display the full error message if one is fired during the database connection.

- **strapi new &#60;name&#62; --quickstart**<br/>
  Use the quickstart system to create your app.

- **strapi new &#60;name&#62; --quickstart --no-run**<br/>
  Use the quickstart system to create your app, and do not start the application after creation.

- **strapi new &#60;name&#62; --dbclient=&#60;dbclient&#62; --dbhost=&#60;dbhost&#62; --dbport=&#60;dbport&#62; --dbname=&#60;dbname&#62; --dbusername=&#60;dbusername&#62; --dbpassword=&#60;dbpassword&#62; --dbssl=&#60;dbssl&#62; --dbauth=&#60;dbauth&#62; --dbforce**<br/>

  Generates a new project called **&#60;name&#62;** and skip the interactive database configuration and initialize with these options.

  - **&#60;dbclient&#62;** can be `postgres`, `mysql`.
  - **--dbforce** Allows you to overwrite content if the provided database is not empty. Only available for `postgres`, `mysql`, and is optional.

## strapi develop

**Alias**: `dev`

Start a Strapi application with autoReload enabled.

Strapi modifies/creates files at runtime and needs to restart when new files are created. To achieve this, `strapi develop` adds a file watcher and restarts the application when necessary.

```
strapi develop
options: [--no-build |--watch-admin |--browser ]
```

- **strapi develop**<br/>
  Starts your application with the autoReload enabled
- **strapi develop --no-build**<br/>
  Starts your application with the autoReload enabled and skip the administration panel build process
- **strapi develop --watch-admin**<br/>
  Starts your application with the autoReload enabled and the front-end development server. It allows you to customize the administration panel.
- **strapi develop --watch-admin --browser 'google chrome'**<br/>
  Starts your application with the autoReload enabled and the front-end development server. It allows you to customize the administration panel. Provide a browser name to use instead of the default one, `false` means stop opening the browser.

:::warning
You should never use this command to run a Strapi application in production.
:::

## strapi start

Start a Strapi application with autoReload disabled.

This command is there to run a Strapi application without restarts and file writes (aimed at production usage).
Certain features are disabled in the `strapi start` mode because they require application restarts.

Allowed environment variables:
| Property | Description | Type | Default |
| --------- | ----------- | ----- | ------- |
| STRAPI_HIDE_STARTUP_MESSAGE | If `true` then Strapi will not show startup message on boot. Values can be `true` or `false` | string | `false` |
| STRAPI_LOG_LEVEL | Values can be 'fatal', 'error', 'warn', 'info', 'debug', 'trace' | string | `debug` |
| STRAPI_LOG_TIMESTAMP | Enables or disables the inclusion of a timestamp in the log message. Values can be `true` or `false` | string | `false`|
| STRAPI_LOG_FORCE_COLOR | Values can be `true` or `false` | string | `true` |
| STRAPI_LOG_PRETTY_PRINT | If pino-pretty module will be used to format logs. Values can be `true` or `false` | string | `true` |

## strapi build

Builds your admin panel.

```bash
strapi build

options: [--no-optimization]
```

- **strapi build**<br/>
  Builds the administration panel and delete the previous build and .cache folders
- **strapi build --no-optimization**<br/>
  Builds the administration panel without minimizing the assets. The build duration is faster.

## strapi watch-admin

Starts the admin server.  Strapi should already be running with `strapi develop`.

```sh
strapi watch-admin
options: [--browser <name>]
```

## strapi configuration:dump

**Alias**: `config:dump`

Dumps configurations to a file or stdout to help you migrate to production.

The dump format will be a JSON array.

```
strapi configuration:dump

Options:
  -f, --file <file>  Output file, default output is stdout
  -p, --pretty       Format the output JSON with indentation and line breaks (default: false)
```

**Examples**

- `strapi configuration:dump -f dump.json`
- `strapi config:dump --file dump.json`
- `strapi config:dump > dump.json`

All these examples are equivalent.

:::caution
When configuring your application you often enter credentials for third party services (e.g authentication providers). Be aware that those credentials will also be dumped into the output of this command.
In case of doubt, you should avoid committing the dump file into a versioning system. Here are some methods you can explore:

- Copy the file directly to the environment you want and run the restore command there.
- Put the file in a secure location and download it at deploy time with the right credentials.
- Encrypt the file before committing and decrypt it when running the restore command.

:::

## strapi configuration:restore

**Alias**: `config:restore`

Restores a configuration dump into your application.

The input format must be a JSON array.

```
strapi configuration:restore

Options:
  -f, --file <file>          Input file, default input is stdin
  -s, --strategy <strategy>  Strategy name, one of: "replace", "merge", "keep". Defaults to: "replace"
```

**Examples**

- `strapi configuration:restore -f dump.json`
- `strapi config:restore --file dump.json -s replace`
- `cat dump.json | strapi config:restore`
- `strapi config:restore < dump.json`

All these examples are equivalent.

**Strategies**

When running the restore command, you can choose from three different strategies:

- **replace**: Will create missing keys and replace existing ones.
- **merge**: Will create missing keys and merge existing keys with their new value.
- **keep**: Will create missing keys and keep existing keys as is.

## strapi admin:reset-user-password

**Alias** `admin:reset-password`

Reset an admin user's password.
You can pass the email and new password as options or set them interactively if you call the command without passing the options.

**Example**

```bash
strapi admin:reset-user-password --email=chef@strapi.io --password=Gourmet1234
```

**Options**

| Option         | Type   | Description               |
| -------------- | ------ | ------------------------- |
| -e, --email    | string | The user email            |
| -p, --password | string | New password for the user |
| -h, --help     |        | display help for command  |

## strapi generate

Run a fully interactive CLI to generate APIs, [controllers](/developer-docs/latest/development/backend-customization/controllers.md), [content-types](/developer-docs/latest/development/backend-customization/models.md), [plugins](/developer-docs/latest/development/plugins-development.md#creating-a-plugin), [policies](/developer-docs/latest/development/backend-customization/policies.md), [middlewares](/developer-docs/latest/development/backend-customization/middlewares.md) and [services](/developer-docs/latest/development/backend-customization/services.md).

```sh
strapi generate
```

## strapi templates:generate

Create a template from the current strapi project

```bash
strapi templates:generate <path>
```

- **strapi templates:generate &#60;path&#62;**<br/>
  Generates a Strapi template at `<path>`

  Example: `strapi templates:generate ../strapi-template-name` will copy the required files and folders to a `template` directory inside `../strapi-template-name`

## strapi routes:list

Display a list of all the available [routes](/developer-docs/latest/development/backend-customization/routes.md).

```sh
strapi routes:list
```

## strapi policies:list

Display a list of all the registered [policies](/developer-docs/latest/development/backend-customization/policies.md).

```sh
strapi policies:list
```

## strapi middlewares:list

Display a list of all the registered [middlewares](/developer-docs/latest/development/backend-customization/middlewares.md).

```sh
strapi middlewares:list
```

## strapi content-types:list

Display a list of all the existing [content-types](/developer-docs/latest/development/backend-customization/models.md).

```sh
strapi content-types:list
```

## strapi hooks:list

Display a list of all the available hooks.

```sh
strapi hooks:list
```

## strapi controllers:list

Display a list of all the registered [controllers](/developer-docs/latest/development/backend-customization/controllers.md).

## strapi services:list

Display a list of all the registered [services](/developer-docs/latest/development/backend-customization/services.md).

```sh
strapi services:list
```

## strapi install

Install a plugin in the project.

```bash
strapi install <name>
```

- **strapi install &#60;name&#62;**<br/>
  Installs a plugin called **&#60;name&#62;**.

  Example: `strapi install graphql` will install the plugin `strapi-plugin-graphql`

:::caution
Some plugins have admin panel integrations, your admin panel might have to be rebuilt. This can take some time.
:::

## strapi uninstall

Uninstall a plugin from the project.

```bash
strapi uninstall <name>

options [--delete-files]
```

- **strapi uninstall &#60;name&#62;**<br/>
  Uninstalls a plugin called **&#60;name&#62;**.

  Example: `strapi uninstall graphql` will remove the plugin `strapi-plugin-graphql`

- **strapi uninstall &#60;name&#62; --delete-files**<br/>
  Uninstalls a plugin called **&#60;name&#62;** and removes the files in `./extensions/name/`

  Example: `strapi uninstall graphql --delete-files` will remove the plugin `strapi-plugin-graphql` and all the files in `./extensions/graphql`

:::caution
Some plugins have admin panel integrations, your admin panel might have to be rebuilt. This can take some time.
:::

## strapi console

Start the server and eval commands in your application in real time.

```bash
strapi console
```

## strapi version

Print the current globally installed Strapi version.

```bash
strapi version
```

## strapi help

List CLI commands.

```
strapi help
```
