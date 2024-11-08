# Build eap action for Github Actions

An action for Github Actions to build Axis ACAP applications in CI/CD pipelines.

This action assumes the FixedIT CLI is installed, see the [install-fixeditcli-action](https://github.com/fixedit-ai/install-fixeditcli-action).

## Arguments

Most of the CLI arguments for the FixedIT CLI tool is exposed in this action, see the definition in the [action.yml file](./action.yml). Note that you can also specify the arguments in a `fixedit-manifest.json` file.

## Example

As an example, the following GitHub Actions CI/CD job will build the ACAP application where the code is located in the `hello_world` directory. The application will be built both for the `armv7hf` and `aarch64` cameras. After the application is built the [test-eap-action](https://github.com/fixedit-ai/test-eap-action) is used to validate the .eap file that was produced by the build.

```yaml
name: Build hello world ACAP

on:
  push:

jobs:
  build:
    runs-on: ubuntu-24.04
    strategy:
      matrix:
        arch: ["armv7hf", "aarch64"]
    steps:
      - name: Checkout the code
        uses: actions/checkout@v4

      - name: Install FixedIT CLI and login
        uses: fixedit-ai/install-fixeditcli-action@v2
        with:
          token: ${{ secrets.FIXEDIT_TOKEN }}

      - name: Build ACAP application
        uses: fixedit-ai/build-eap-action@v2
        with:
          arch: ${{ matrix.arch }}
          dir: hello_world

      - name: Validate the content of the ACAP .eap file
        uses: fixedit-ai/test-eap-action@v2
        with:
          arch: ${{ matrix.arch }}
          app-path: "hello_world/*.eap"
```

## Release notes

### v2

- Bumped the CLI to the new renamed `fixeditcli`. This action is compatible with `fixedit-ai/install-fixeditcli-action` v2 or later.
- Added more CLI options as arguments to the action.
- Changed the libraries list from comma separated to new line separated, e.g.

In V1:

```yml
with:
  libs: openssl,curl
```

In v2:

```yml
with:
  libs: |
    openssl
    curl
```

### v1

First stable release using `FApp CLI`.
