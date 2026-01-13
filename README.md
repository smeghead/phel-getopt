# phel-getopt

Natural and safe command-line argument parsing for phel CLI programs.

![Testing](https://github.com/smeghead/phel-getopt/actions/workflows/ci.yml/badge.svg?event=push) [![Latest Stable Version](https://img.shields.io/packagist/v/smeghead/phel-getopt.svg)](https://packagist.org/packages/smeghead/phel-getopt) [![Total Downloads](https://img.shields.io/packagist/dt/smeghead/phel-getopt.svg)](https://packagist.org/packages/smeghead/phel-getopt) [![License](https://img.shields.io/packagist/l/smeghead/phel-getopt.svg)](LICENSE)

## Summary

Parsing command-line arguments in phel is fundamentally different from
traditional PHP CLI scripts.

phel-getopt embraces this reality and provides a dedicated solution
tailored to phel's execution model.

## The Problem

The contents of `phel/argv` depend on **how the phel program is executed**.

### 1. Executing a phel script file

```bash
vendor/bin/phel run src/main.phel -a one two
```

`phel/argv` becomes:

```clojure
["vendor/bin/phel" "run" "src/main.phel" "-a" "one" "two"]
```
User-defined arguments start at index 3.

### 2. Executing a phel namespace

```bash
vendor/bin/phel run app\\main -a one two
```

`phel/argv` becomes:

```clojure
["vendor/bin/phel" "run" "app\\main" "-a" "one" "two"]
```

The structure is similar, but whether the third argument is a file path or
a namespace cannot be reliably determined at runtime.

### 3. Executing compiled PHP directly

```bash
php ./out/main.php -a one two
```


`phel/argv` becomes:

```clojure
["./out/main.php" "-a" "one" "two"]
```

User-defined arguments start at index 1.


## Why PHP's getopt() Is Not Suitable

PHP's `getopt()` assumes that:

* `$argv[0]` is the current script

* All following arguments belong to the script itself

This assumption breaks down in phel, because:

* The phel CLI launcher (`vendor/bin/phel`) consumes part of the argument list

* The number of launcher arguments varies depending on execution style

* There is no safe, universal index at which user arguments begin

As a result, directly calling PHP's getopt() leads to fragile and error-prone
CLI tools.

## What phel-getopt Provides

phel-getopt abstracts away these differences and provides:

* Safe detection of user-defined command-line arguments

* Execution-method-independent argument parsing

* A natural and predictable CLI experience for phel programs

With phel-getopt, phel CLI tools can be written as if they were invoked
directly, regardless of how they are actually executed.

## Design Philosophy

* Do not rely on fixed argv indices

* Do not depend on execution style (file, namespace, or compiled PHP)

* Make phel CLI tools portable and robust

This library exists because phel is a compiled-to-PHP language with multiple
execution paths — and CLI tools should not have to care about that.










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
(def result (getopt/getopt argv options *file* *ns*))

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
