# phel-getopt

Natural and safe command-line argument parsing for phel CLI programs.

![Testing](https://github.com/smeghead/phel-getopt/actions/workflows/ci.yml/badge.svg?event=push) [![Latest Stable Version](https://img.shields.io/packagist/v/smeghead/phel-getopt.svg)](https://packagist.org/packages/smeghead/phel-getopt) [![Total Downloads](https://img.shields.io/packagist/dt/smeghead/phel-getopt.svg)](https://packagist.org/packages/smeghead/phel-getopt) [![License](https://img.shields.io/packagist/l/smeghead/phel-getopt.svg)](LICENSE)

A small and focused command-line option and argument parser for phel.

## Overview

`phel-getopt` is a library for parsing command-line options and arguments in phel CLI programs. It provides clear, predictable, and phel-friendly command-line parsing with a simple and explicit API.

## Installation

```bash
composer require smeghead/phel-getopt
```

## Usage

### Basic Usage

```clojure
(ns app\core
  (:require smeghead\getopt\getopt))

(def options ["-a"         # short option `a` without value
              "--output:"  # long option `output` with value(required)
             ])
(def result (getopt/getopt argv options))

(println "options:" (result :options))
(println "arguments:" (result :args))
```

```bash
$ vendor/bin/phel run examples/example.phel -a --output ./output.txt one two
options: {:a true, :output ./output.txt}
arguments: [one two]
```

### Option Definitions

Options are defined as strings with the following format:

| Format | Meaning | Example |
|--------|---------|---------|
| `"-x"` | Short option without value | `"-a"` |
| `"-x:"` | Short option with required value | `"-o:"` |
| `"--name"` | Long option without value | `"--verbose"` |
| `"--name:"` | Long option with required value | `"--output:"` |

The colon suffix (`:`) indicates that an option requires a value.

### Result Structure

The `getopt` function returns a `Result` struct with three fields:

- `:args` - Vector of positional arguments
- `:options` - Map of option keywords to their values
- `:errors` - Vector of error messages (empty if successful)

### Practical Examples

#### Multiple Arguments

```clojure
(def options [])
(def result (getopt/getopt ["one" "two"] options))
;; result: {:args ["one" "two"], :options {}, :errors []}
``` [4](#1-3) 

#### Short Options with Values

```clojure
(def options ["-a:"])
(def result (getopt/getopt ["-a" "param" "one" "two"] options))
;; result: {:args ["one" "two"], :options {:a "param"}, :errors []}
``` [5](#1-4) 

#### Combined Short Options

```clojure
(def options ["-a:"])
(def result (getopt/getopt ["-aparam" "one" "two"] options))
;; result: {:args ["one" "two"], :options {:a "param"}, :errors []}
``` [6](#1-5) 

#### Long Options with Values

```clojure
(def options ["--amazing:"])
(def result (getopt/getopt ["--amazing=man" "one" "two"] options))
;; result: {:args ["one" "two"], :options {:amazing "man"}, :errors []}
``` [7](#1-6) 

#### Error Handling

```clojure
(def options ["-a"])
(def result (getopt/getopt ["--unknown" "one" "two"] options))
;; result: {:args ["one" "two"], :options {}, :errors ["Unknown option: \"--unknown\""]}
``` [8](#1-7) 

#### Stop Parsing with --

```clojure
(def options ["-a"])
(def result (getopt/getopt ["--" "-a" "one" "two"] options))
;; result: {:args ["-a" "one" "two"], :options {}, :errors []}
``` [9](#1-8) 

## API Reference

### getopt/getopt

```clojure
(defn getopt [args options])
```

- `args` - Vector of command-line arguments (normalized `argv`)
- `options` - Vector of option definition strings
- Returns - `Result` struct with `:args`, `:options`, and `:errors` fields

## Notes

This library assumes a normalized `argv` provided by Phel v0.28.0+ and focuses solely on option and argument parsing. For development setup instructions, see the original README history.











## Composer Install

```bash
composer require smeghead/phel-getopt
```

## Usage

```clojure
(ns app\core
  (:require smeghead\getopt\getopt))

(def options ["-a"  # short option `a` without value.
              "-p:" # short option `p` with value(required).
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
*program*: src/phel/main.phel
argv: [-a --output ./output.txt one two]
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
