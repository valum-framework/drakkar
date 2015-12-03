# Drakkar

Drakkar is a package manager for Vala.

It is a mere tool to resolve dependencies, download packages, bundle sources
and resources, and generate appropriate `valac` flags that can be easily
integrated in any build system.

## Commands

Command | Effect
------- | ------
search  | search for packages from providers
install | install the packages described in `package.json`
update  | update the dependencies listed in `package.json`
flags   | generate `valac` flags
pack    | pack the files described in `package.json`
version | print Drakkar's version and exit

The package specification is compatible with npm's
[package.json](https://docs.npmjs.com/files/package.json).

```json
{
  "name": "view",
  "files":
  [
    "src/**/*.vala",
    "data/view.gresources.xml",
    "data/*"
  ],
  "dependencies":
  {
    "glib-2.0": ">=2.32",
    "libsoup-2.4": ">=2.50",
    "valum": "~0.2"
  }
}
```

### Install

The `install` command either download and install all packages described in
`package.json` dependencies or perform the operation on a specified package
from the providers

 1. resolve dependencies using backtracking
 2. download packages from providers (git, svn, bzr, ...)
 3. extract relevant sources and copy over resources locally in the `deps`
    folder

Similarly, the `update` command will update dependencies up to the latest
version that fullfill its predicate.

### Flags

The `flags` command generates the appropriate `valac` flags (`--pkg`,
`--vapidir` and `--resources`) suitable for any build system.

```bash
valac $(drakkar flags) src/*.vala
```

### Packing

The `pack` command is a convenient way to create a bundle containing all the
described resources in the `package.json` into a compact tarball suitable for
providers distribution.

## Files

The `files` object is used to describe what's being included in the package.

All `*.vala` will be bundled in a compact VAPI format and included with
a `--pkg` and `--vapidir` flag.

Resources described in `*.gresource.xml` files as well as the file itself will
be included in the package.

## Dependencies

Dependencies can be provided from different sources:

 - local libraries (glib-2.0, libsoup-2.4, ...)
 - git repository using a commit hash, a tag or a branch
 - GitHub repository using a `username/project` identifier
 - local or remote path (see [GVfs](https://wiki.gnome.org/Projects/gvfs)-supported protocols)
 - local or remote bundle, which are simple tarballs

```json
{
  "dependencies":
  {
    "valum-framework/drakkar": "~1.0.0",
    "nemequ/vala-extra-vapis": "@master",
    "/usr/share/vala/vapis": "*"
  }
}
```

Versions are extracted from tags following various conventions:

 - `v1.0.0`
 - `1.0.0`

Dependencies that does not provide a `package.json` will be considered as
a whole: all the sources will be downladed and no particular flags will be
generated.

