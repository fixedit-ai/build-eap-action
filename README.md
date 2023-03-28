# Build eap action for Github Actions
An action for Github Actions to build Axis ACAP applications in CI/CD pipelines.

Example:

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

      - name: Build ACAP application
        uses: fixedit-ai/build-eap-action@main
        with:
          token: ${{ secrets.FAPP_TOKEN }}
          arch: ${{ matrix.arch }}
          dir: hello_world

      - name: Validate the content of the ACAP .eap file
        uses: fixedit-ai/test-eap-action@main
        with:
          token: ${{ secrets.FAPP_TOKEN }}
          arch: ${{ matrix.arch }}
          app-path: "hello_world/*.eap"
```

See also:
* [install-fappcli-action](https://github.com/fixedit-ai/install-fappcli-action)
* [test-eap-action](https://github.com/fixedit-ai/test-eap-action)