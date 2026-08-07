# Cloudhippie: Website

[![General Workflow](https://github.com/cloudhippie/website/actions/workflows/general.yml/badge.svg)](https://github.com/cloudhippie/website/actions/workflows/general.yml)

Website for the Cloudhippie project including some landing page and general
information about this project and the company behind it, you can find this
website at [cloudhippie.de][website].

## Prerequisites

We use [mise][mise] to manage all required tools and their versions. Install it
by following the [official installation instructions][mise-install], then run
the following commands inside the repository to activate mise and install all
tools defined in `mise.toml`:

```console
mise trust
mise install
```

## Install

This website uses the [Hugo][hugo] static site generator. If you are planning to
contribute you'll want to download and install Hugo on your local machine. If
you are not using [mise][mise] it is up to you to install all required tools.

## Development

To generate the website and serve it on [localhost:1313](http://localhost:1313)
just execute this command and stop it with `Ctrl+C`:

```console
npm install --ci
hugo server
```

When you are done with your changes just create a pull request, after merging
the pull request the website will be updated automatically.

## Security

If you find a security issue please contact
[security@cloudhippie.de](mailto:security@cloudhippie.de) first.

## Contributing

Generally we are following [conventional commits][commits] when we apply
changes. That way we are able to generate proper changelogs for every release.
Please use always pull requests to integrate new functionalities or to fix
issues.

For the release process we are following [semantic versioning][semver] which
clearly indicates if a new version just resolves bugs, includes new features or
even includes breaking changes.

After installing the tools via `mise install` as described above set up the
pre-commit hooks so they run automatically on every commit:

```console
pre-commit install --hook-type pre-commit --hook-type commit-msg
```

> `pre-commit` is managed by mise and will be available after `mise install`.

If you have changed something on the source you should simply commit following
the mentioned conventions:

```console
git checkout -b feat/new-feature
git add --all
git commit -m 'feat: added awesome new feature'
git push --set-upstream origin feat/new-feature
```

After pushing your changes into the Git repository you should create a pull
request on GitHub. If the pull request have been merged and everything built
fine it will also create automatically a new release at least once a week.

## Authors

-   [Thomas Boerger](https://github.com/tboerger)

## License

Apache-2.0

## Copyright

```console
Copyright (c) 2023 Cloudhippie <info@cloudhippie.de>
```

[mise]: https://mise.jdx.dev/
[mise-install]: https://mise.jdx.dev/getting-started.html
[website]: https://cloudhippie.de
[hugo]: https://github.com/spf13/hugo
[commits]: https://www.conventionalcommits.org/en/v1.0.0/
[semver]: https://semver.org/
