# phel-getopt

Natural and safe command-line argument parsing for phel CLI programs.

![Testing](https://github.com/smeghead/phel-getopt/actions/workflows/ci.yml/badge.svg?event=push) [![Latest Stable Version](https://img.shields.io/packagist/v/smeghead/phel-getopt.svg)](https://packagist.org/packages/smeghead/phel-getopt) [![Total Downloads](https://img.shields.io/packagist/dt/smeghead/phel-getopt.svg)](https://packagist.org/packages/smeghead/phel-getopt) [![License](https://img.shields.io/packagist/l/smeghead/phel-getopt.svg)](LICENSE)

# phel-getopt

A small and focused command-line option and argument parser for phel.

## Overview

`phel-getopt` is a library for parsing command-line options and arguments
in phel CLI programs.

Next versions(v0.28.0?) of phel normalize `argv` so that the same arguments are
available regardless of how a program is executed (file, namespace, or
compiled PHP).

With this improvement in phel itself, `phel-getopt` focuses solely on
providing **clear, predictable, and phel-friendly command-line parsing**.


## Why phel-getopt?

Even with normalized `argv`, parsing command-line options remains a
non-trivial task.

PHP’s built-in `getopt()` has several characteristics that make it
awkward to use from phel:

- It enforces PHP-specific conventions
- It has unintuitive behavior for long options
- It is tightly coupled to PHP’s global state
- It does not feel natural in a Lisp-style language

`phel-getopt` exists to offer a simpler and more explicit alternative
designed for phel programs.


## What This Library Does

`phel-getopt` provides:

- Parsing of short and long options
- Separation of options and positional arguments
- A clear and explicit data structure as the result
- Behavior suitable for CLI tools written in phel

It does **not** attempt to handle execution-method differences or `argv`
normalization — those concerns are handled by phel itself.


## Design Goals

- Simple and explicit parsing rules
- No reliance on global state
- Predictable behavior
- Easy to reason about in phel code
- Small surface area and minimal magic

The goal is not to replicate every feature of GNU `getopt`, but to provide
a practical and understandable tool for everyday phel CLI programs.


## When to Use phel-getopt

Use this library if you are:

- Writing CLI tools in phel
- Looking for an alternative to PHP’s `getopt()`
- Wanting explicit control over options and arguments
- Preferring a small, focused dependency


## When Not to Use It

You may not need `phel-getopt` if:

- Your program accepts no command-line options
- You are comfortable using PHP’s `getopt()` directly
- You need full GNU getopt compatibility


## Background

Earlier versions of this library also addressed differences in `argv`
caused by various phel execution methods.

As of recent phel releases, `argv` is normalized by phel itself.
This library has since narrowed its scope to option and argument parsing only.


## Summary

`phel-getopt` is a lightweight command-line parsing library for phel.

It assumes a normalized `argv` and focuses on doing one thing well:
turning command-line arguments into a clear and usable structure for
phel CLI programs.













## Composer Install

```bash
composer require smeghead/phel-getopt
```

## Usage

```clojure
(ns app\core
  (:require smeghead\getopt\getopt))

(def options ["-a" # short option `a` without value.
              "--output:" # long option `output` with value(required).
             ])
(def result (getopt/getopt argv options))

(println "*program*:" *program*)
(println "argv:" argv)
(println "options:" (result :options))
(println "arguments:" (result :args))
```

```bash
$ vendor/bin/phel run examples/example.phel -a --output ./output.txt one two
options: {:a true, :output ./output.txt}
arguments: [one two]
```




## Development

### Initial

```bash
composer init
composer require phel-lang/phel-lang:dev-main
vendor/bin/phel init
```
