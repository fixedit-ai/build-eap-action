# Build eap action for Github Actions
An action for Github Actions to build Axis ACAP applications in CI/CD pipelines.

This action assumes the FApp CLI is installed, see the [install-fappcli-action](https://github.com/fixedit-ai/install-fappcli-action).

## Example
This GitHUb Actions CI/CD job will build the ACAP application where the code is located in the `hello_world` directory. The application will be built both for the `armv7hf` and `aarch64` cameras. After the application is built the [test-eap-action](https://github.com/fixedit-ai/test-eap-action) is used to validate the .eap file that was produced by the build.

```yaml
name: Build hello world ACAP

on:
  push:

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        arch: ["armv7hf", "aarch64"]
    steps:
      - name: Checkout the code
        uses: actions/checkout@v3

      - name: Install FApp CLI and login
        uses: fixedit-ai/install-fappcli-action@main
        with:
          token: ${{ secrets.FAPP_TOKEN }}

      - name: Build ACAP application
        uses: fixedit-ai/build-eap-action@main
        with:
          arch: ${{ matrix.arch }}
          dir: hello_world

      - name: Validate the content of the ACAP .eap file
        uses: fixedit-ai/test-eap-action@main
        with:
          arch: ${{ matrix.arch }}
          app-path: "hello_world/*.eap"
```