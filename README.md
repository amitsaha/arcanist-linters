# Arcanist Linters

[![Build Status](https://travis-ci.org/pinterest/arcanist-linters.svg)](https://travis-ci.org/pinterest/arcanist-linters)

This is a collection of custom [Arcanist][] linters that we've written at
Pinterest.

## Linters

### Apache Thrift Linter

Lints for errors in [Apache Thrift][] IDL (schema) files using the `thrift`
compiler.

```json
{
    "type": "thrift",
    "include": "(\\.thrift$)",
    "flags": [
        "--allow-64bit-consts"
    ],
    "version": ">= 0.9.0",
    "thrift.generators": [
        "py:dynamic,utf8strings,new_style,slots",
        "java",
        "go",
        "erl"
    ],
    "thrift.includes": [
        ".",
        "common"
    ]
}
```

[Apache Thrift]: http://thrift.apache.org/

### Apache Thrift Generated Linter

Lints files generated by the [Apache Thrift][] compiler to ensure they were
generated using a supported Thrift compiler.

```json
{
    "type": "thrift-gen",
    "include": "(^schemas/.*\\.py$)",
    "thrift-gen.version": ">=0.9.3"
}
```

*Note:* Currently only generated Python files are supported.

### Checkstyle Linter

Uses the [Checkstyle](http://checkstyle.sourceforge.net/) tool to check Java
code against a coding standard.

```json
{
    "type": "checkstyle",
    "include": "(\\.java$)",
    "checkstyle.config": "google_check.xml"
}
```

### ESLint Linter

Lints JavaScript and JSX files using [ESLint](https://eslint.org/).

```json
{
    "type": "eslint",
    "include": "(\\.js$)",
    "bin": "./node_modules/.bin/eslint",
    "eslint.config": "~/my-eslint.json",
    "eslint.env": "browser,node"
}
```

### Go Vet Linter

Uses the [Go vet command](https://golang.org/cmd/vet/) to lint for suspicious
code constructs.

```json
{
    "type": "govet",
    "include": "(^src/example.com/.*\\.go$)"
}
```

### Python Imports Linter

Lints for illegal Python module imports.

```json
{
    "type": "python-imports",
    "python-imports.pattern": "(mock)",
    "include": "(\\.py$)",
    "exclude": "(^tests/)"
}
```

### Python Requirements Linter

Ensures Python package requirements in [requirements.txt files][req-txt] are
sorted, unique, and pinned to exact versions.

```json
{
    "type": "requirements-txt",
    "include": "(requirements.txt$)"
}
```

Individual requirement lines can be excluded by adding a `# noqa` comment:

```
six>=1.10.0  # noqa: allow any recent version of six
```

[req-txt]: https://pip.readthedocs.org/en/latest/user_guide/#requirements-files

## Installation

In short, you'll need to add this repository to your local machine and tell
Arcanist to load the extension. You either can do this globally or on a
per-project basis.

Once installed, the individual linters can be enabled and configured via the
project's `.arclint` file. See the [Arcanist Lint User Guide][lint-guide] for
details.

## Global Installation

Arcanist can load modules from an absolute path, but because it also searches
for modules one level up from itself on the filesystem, it's convenient to
clone this repository at the same level as `arcanist` and `libphutil`.

```
$ git clone https://github.com/pinterest/arcanist-linters.git pinterest-linters
$ ls
arcanist
pinterest-linters
libphutil
```

Then, tell Arcanist to load the module by editing `~/.arcconfig` (or
`/etc/arcconfig`):

```json
{
  "load": ["pinterest-linters"]
}
```

## Project Installation

You can also load `arcanist-linters` on a per-project basis. In that case,
using a [git submodule](https://git-scm.com/docs/git-submodule) is probably
the most convenient approach.

```
$ git submodule add https://github.com/pinterest/arcanist-linters.git .pinterest-linters
$ git submodule update --init
```

Then, enable the module in your project-level `.arcconfig` file:

```json
{
  "load": [".pinterest-linters"]
}
```

[Arcanist]: https://secure.phabricator.com/book/phabricator/article/arcanist/
[lint-guide]: https://secure.phabricator.com/book/phabricator/article/arcanist_lint/
