# Envoy filter example

This project demonstrates the linking of `wallarm` filter with the Envoy binary.

## Building

To build the Envoy static binary with `wallarm` filter:

1. Copy libwallarm.a and libwallarmmisc.a to current dir
2. `git submodule update --init`
3. Run `ENVOY_DOCKER_BUILD_DIR=/tmp/envoy-wallarm ENVOY_BUILD_TARGET=//:envoy_wallarm ENVOY_SRCDIR=/source/envoy ./envoy/ci/run_envoy_docker.sh './envoy/ci/do_ci.sh bazel.release.server_only'`

Note: There can be errors like this:
```
ERROR: Skipping '//source/exe:envoy-static.dwp': no such package 'source/exe': BUILD file not found in any of the following directories. Add a BUILD file to a directory to mark it as a package.
ERROR: no such package 'source/exe': BUILD file not found in any of the following directories. Add a BUILD file to a directory to mark it as a package.
```
Ignore them for now. It is known limitations of envoy build system:
* https://github.com/envoyproxy/envoy/issues/3875
* https://github.com/envoyproxy/envoy/issues/7875
4. The build artifact can be found in `/tmp/envoy-wallarm/envoy/source/exe/envoy`

## How it works

The [Envoy repository](https://github.com/envoyproxy/envoy/) is provided as a submodule.
The [`WORKSPACE`](WORKSPACE) file maps the `@envoy` repository to this local path.

The [`BUILD`](BUILD) file introduces a new Envoy static binary target, `envoy_wallarm`,
that links together the new filter and `@envoy//source/exe:envoy_main_entry_lib`. The
`wallarm` filter registers itself during the static initialization phase of the
Envoy binary as a new filter.
