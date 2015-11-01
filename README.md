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
build   | issue a build command based on `valac`
flags   | generate `valac` flags
pack    | pack the files described in `package.json`

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

## Install

 1. resolve dependencies using backtracking
 2. download packages from providers (git, svn, bzr, ...)
 3. extract relevant sources (in a compact VAPI) and copy over resources locally
 4. produce CLI arguments (`--pkg`, `--vapidir` and `--resources` flags and more)

## Packing

The `pack` command is a convenient way to create a bundle containing all the
described resources in the `package.json` file so that they can be easily
distributed or cached in a tarball.

## Files

The `files` object is used to describe what's being included in the package.

All `*.vala` will be bundled in a compact VAPI format and included with
a `--pkg` and `--vapidir` flag.

Resources described in `*.gresource.xml` files as well as the file itself will
be included in the package.

## Dependencies

 - local libraries (glib-2.0, libsoup-2.4, ...)
 - version predicate
 - git repository using a commit hash
 - local or remote path or bundle

Versions are extracted from tags following various conventions:

 - `v1.0.0`
 - `1.0.0`

