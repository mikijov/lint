Golint is a linter for Go source code.

[![Build Status](https://travis-ci.org/golang/lint.svg?branch=master)](https://travis-ci.org/golang/lint)

## Fork Notes

This is a temporary fork which incorporates a hack to allow disabling of golint
checks. The change is a hack to minimize the change to the upstream golint code,
so that it can be merged easily on golint updates. If the idea of disabling
checks is accepted, the code can be made nicer.

First time install:
```bash
$ cd go/src/github.com/golang/lint/golint
$ git remote add mikijov git@github.com:mikijov/lint.git
$ git fetch mikijov
$ git checkout mikijov/master
$ go build
$ go install
```

Subsequent updates:
```bash
$ cd go/src/github.com/golang/lint/golang
$ git pull
$ go build
$ go install
```

The fork adds two new command line flags:
```
-disable_category value
    disable whole category of checks (naming, unary-op, arg-order, unexported-type-in-api, time, context, comments, imports, zero-value, type-inference, indent, range-loop, errors)
-disable_check value
    disable individual checks; specify beginning of the message; e.g. 'receiver name should'
```
They can be repeated multiple times to disable multiple checks or categories.

If using `gometalinter` you can use `.gometalinter.json` file:
```json
{
  "Linters": {
      "golint": {
        "Command": "golint -disable_category naming"
      }
  }
}
```

## Installation

Golint requires a
[supported release of Go](https://golang.org/doc/devel/release.html#policy).

    go get -u golang.org/x/lint/golint

## Usage

Invoke `golint` with one or more filenames, directories, or packages named
by its import path. Golint uses the same
[import path syntax](https://golang.org/cmd/go/#hdr-Import_path_syntax) as
the `go` command and therefore
also supports relative import paths like `./...`. Additionally the `...`
wildcard can be used as suffix on relative and absolute file paths to recurse
into them.

The output of this tool is a list of suggestions in Vim quickfix format,
which is accepted by lots of different editors.

## Purpose

Golint differs from gofmt. Gofmt reformats Go source code, whereas
golint prints out style mistakes.

Golint differs from govet. Govet is concerned with correctness, whereas
golint is concerned with coding style. Golint is in use at Google, and it
seeks to match the accepted style of the open source Go project.

The suggestions made by golint are exactly that: suggestions.
Golint is not perfect, and has both false positives and false negatives.
Do not treat its output as a gold standard. We will not be adding pragmas
or other knobs to suppress specific warnings, so do not expect or require
code to be completely "lint-free".
In short, this tool is not, and will never be, trustworthy enough for its
suggestions to be enforced automatically, for example as part of a build process.
Golint makes suggestions for many of the mechanically checkable items listed in
[Effective Go](https://golang.org/doc/effective_go.html) and the
[CodeReviewComments wiki page](https://golang.org/wiki/CodeReviewComments).

## Scope

Golint is meant to carry out the stylistic conventions put forth in
[Effective Go](https://golang.org/doc/effective_go.html) and
[CodeReviewComments](https://golang.org/wiki/CodeReviewComments).
Changes that are not aligned with those documents will not be considered.

## Contributions

Contributions to this project are welcome provided they are [in scope](#scope),
though please send mail before starting work on anything major.
Contributors retain their copyright, so we need you to fill out
[a short form](https://developers.google.com/open-source/cla/individual)
before we can accept your contribution.

## Vim

Add this to your ~/.vimrc:

    set rtp+=$GOPATH/src/github.com/golang/lint/misc/vim

If you have multiple entries in your GOPATH, replace `$GOPATH` with the right value.

Running `:Lint` will run golint on the current file and populate the quickfix list.

Optionally, add this to your `~/.vimrc` to automatically run `golint` on `:w`

    autocmd BufWritePost,FileWritePost *.go execute 'Lint' | cwindow


## Emacs

Add this to your `.emacs` file:

    (add-to-list 'load-path (concat (getenv "GOPATH")  "/src/github.com/golang/lint/misc/emacs"))
    (require 'golint)

If you have multiple entries in your GOPATH, replace `$GOPATH` with the right value.

Running M-x golint will run golint on the current file.

For more usage, see [Compilation-Mode](http://www.gnu.org/software/emacs/manual/html_node/emacs/Compilation-Mode.html).
