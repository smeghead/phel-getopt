# phel-getopt
parse CLI arguments and options.

![Testing](https://github.com/smeghead/phel-getopt/actions/workflows/ci.yml/badge.svg?event=push)

Programs that function as phel CLI commands execute as compiled PHP code at runtime, presenting unique challenges for handling command-line arguments.
Additionally, there are multiple ways to execute phel scripts (`phel run <scriptname>`, `phel run <namespace>`).

Therefore, simply using PHP's getopt does not work as expected in this context.

When creating CLI commands with phel, we developed a getopt library that provides natural command-line argument parsing.



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
