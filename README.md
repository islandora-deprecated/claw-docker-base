# Islandora CLAW: Base Docker Image

[![Docker Stars](https://img.shields.io/docker/stars/islandora/claw-base.svg)](https://hub.docker.com/r/islandora/claw-base/)
[![Docker Pulls](https://img.shields.io/docker/pulls/islandora/claw-base.svg)](https://hub.docker.com/r/islandora/claw-base/)

## Introduction

The base Docker image from which all other Islandora CLAW Docker images are built, based on [Alpine Linux](http://alpinelinux.org/).

## Includes
 
* [s6-overlay](https://github.com/just-containers/s6-overlay)
* [glibc](https://github.com/andyshinn/alpine-pkg-glibc)
* [confd](https://github.com/kelseyhightower/confd)

## Build Arguments

| Variable      | Required |  Default |
|---------------|----------|----------|
| S6_VERSION    | no       | 1.17.1.2 |
| GLIBC_VERSION | no       |  2.22-r8 |
| CONFD_VERSION | no       |   0.11.0 |

**Example:**
```bash
docker build -t islandora/claw-base .
```

## Environment Variables

See the [s6-overlay](https://github.com/just-containers/s6-overlay) repository for what environment variables it exposes, we override the some values as is shown below:

| Variable                     | Required | Default |
|------------------------------|----------|---------|
| S6_BEHAVIOUR_IF_STAGE2_FAILS | no       |       2 |
| ETCD_HOST                    | no       |    etcd |
| ETCD_HOST_PORT               | no       |    2379 |
| ETCD_CONNECTION_TIMEOUT      | no       |       0 |

[etcd](https://github.com/coreos/etcd) can be used to store configuration information instead of environment variables.

**Example (foreground, auto-remove, interactive shell):**
```bash
docker run --rm -ti islandora/claw-base ash
```

## Commands

For convenience a number of commands are provided in the [commands](/commands) folder.

| Command | Arguments | Defaults | Notes                                                                 |
|---------|-----------|----------|-----------------------------------------------------------------------|
| build   |           |          | Build this image with the default settings.                           |
| run     |           | ash      | Start container, execute the given arguments as a command, then exit. |

## Notes

[Alpine Linux](http://alpinelinux.org/) is based on [musl libc](http://www.musl-libc.org/), so we've included [glibc](https://www.gnu.org/software/libc/) in the base image as there are many applications built on `glibc` which are not provided by Alpine Linux's package manager [apk](http://wiki.alpinelinux.org/wiki/Alpine_Linux_package_management). For us it's simpler to use `glibc` than to compile our dependencies against `musl libc`.

We get our version of `glibc` from [Andy Shinn](https://github.com/andyshinn/alpine-pkg-glibc), who has built an `apk` package of `glibc`.

If your considering building out a dependency against `musl libc` or `glibc` you may want to review the [functional differences](http://wiki.musl-libc.org/wiki/Functional_differences_from_glibc)
between these two libc implementations.

To simplify process management we've opted to use [s6](http://skarnet.org/software/s6/overview.html), with the [s6-overlay](https://github.com/just-containers/s6-overlay) package which allows us to run multiple processes in a single container. You may have read that this is bad practice, but we would encourage you to read about the [reasoning](https://github.com/just-containers/s6-overlay#the-docker-way) behind it.

## Maintainers/Sponsors

* UPEI
* discoverygarden inc.
* LYRASIS
* McMaster University
* University of Limerick
* York University
* University of Manitoba
* Simon Fraser University
* PALS
* American Philosophical Society
* common media inc.

Current maintainers:

* [Nigel Banks](https://github.com/nigelgbanks)
* [Nick Ruest](https://github.com/ruebot)

## Development

If you would like to contribute, please get involved by attending our weekly [Tech Call](https://github.com/Islandora-CLAW/CLAW/wiki). We love to hear from you!

If you would like to contribute code to the project, you need to be covered by an Islandora Foundation [Contributor License Agreement](http://islandora.ca/sites/default/files/islandora_cla.pdf) or [Corporate Contributor Licencse Agreement](http://islandora.ca/sites/default/files/islandora_ccla.pdf). Please see the [Contributors](http://islandora.ca/resources/contributors) pages on Islandora.ca for more information.

## License

[MIT](https://opensource.org/licenses/MIT)
