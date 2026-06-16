# Heroku Buildpack: untrunc

Installs a pinned `untrunc` binary for classic Heroku Rails apps.

This buildpack is currently intended for `heroku-22` apps. It downloads the
`untrunc-heroku-22-amd64` release asset and installs it at:

```text
/app/vendor/untrunc/bin/untrunc
```

It also adds `/app/vendor/untrunc/bin` to `PATH` and exports:

```text
UNTRUNC_BIN=/app/vendor/untrunc/bin/untrunc
```

## App Setup

Add the buildpack before the application buildpacks:

```bash
heroku buildpacks:add --index 3 https://github.com/itaca-pro/heroku-buildpack-untrunc -a itaca-server
heroku config:set UNTRUNC_BIN=/app/vendor/untrunc/bin/untrunc -a itaca-server
```

Deploy the app, then verify:

```bash
heroku run 'echo $UNTRUNC_BIN && test -x "$UNTRUNC_BIN" && echo OK' -a itaca-server
heroku run '$UNTRUNC_BIN || true' -a itaca-server
```

## Release Asset

The binary release asset is built by `.github/workflows/release.yml` on
`ubuntu-22.04` from `anthwlock/untrunc`, pinned by commit SHA in the workflow.

Create or replace the `v1` release asset by pushing tag `v1`:

```bash
git tag v1
git push origin v1
```

If the app moves to `heroku-24`, add a separate Ubuntu 24.04 build asset and
update `bin/compile`.
