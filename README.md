# deb packaging for vaultwarden web vault

This debian source package builds `vaultwarden-web-vault` natively on your build environment from the [official upstream](https://github.com/bitwarden/clients) enriched with the vaultwarden patch. No annoying docker! This debian source is managed with [git-buildpackage](https://wiki.debian.org/PackagingWithGit) and is aimed to provide a pretty good quality debian source package (where possible so far). You can find the maintaining command summary in [debian/gbp.conf](debian/gbp.conf).

## Download prebuilt packages

Prebuild deb packages are available in the [releases section](https://github.com/dionysius/vaultwarden-deb/releases) for latest Ubuntu LTS. I'd liked to include also debian stable, but [there are no debian images available in github actions](https://github.com/actions/runner-images).

## Requirements

- Installed `git-buildpackage` `debhelper-compat`
- All build dependencies as defined in [debian/control](debian/control) are installed (it will notify you in the build process as well)
  - [`mk-build-deps`](https://manpages.debian.org/testing/devscripts/mk-build-deps.1.en.html) can help you automate the installation
- If `npm`/`nodejs` is not recent enough
  - Don't forget to look into your `*-updates` apt sources for newer versions
  - This debian source also supports those installed with help of [`nvm`](https://github.com/nvm-sh/nvm)
    - Requires preloaded `nvm use <version>` before invoking packaging

## Packaging

- Clone with help of git-buildpackage: `gbp clone https://github.com/dionysius/vaultwarden-web-vault-deb.git`
- Switch to the folder: `cd vaultwarden-web-vault-deb`
- Build with help of git-buildpacke: `gbp buildpackage`
  - There are many arguments to fine tune how it is built (see `gbp buildpackage --help` and `dpkg-buildpackage --help`)
  - Mine are usually: `-b` (binary-only, no source files), `-us` (unsigned source package), `-uc` (unsigned .buildinfo and .changes file), `--git-export-dir=<somedir>` (before building the package export the source there)

## Flaws

- For my use case I had to make a compatibility to allow `npm` to be provided with help of `nvm` since my distro didn't provide a recent enough version. Only using official apt sources would be cleaner.
- This is the first iteration of the package, thus the packaging is in alpha. Contributions are well appreciated.
