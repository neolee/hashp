# Hashp

[![Clojars Project](https://img.shields.io/clojars/v/com.github.neolee/hashp.svg)](https://clojars.org/com.github.neolee/hashp)

Hashp is a better `prn` for debugging Clojure code. Inspired by projects like [Spyscope][], Hashp (ab)uses data readers to make it easier to get useful debugging data sent to STDOUT.

[spyscope]: https://github.com/dgrnbrg/spyscope

## Usage

Once installed, you can add `#p` in front of any form you wish to print:

```clojure
(ns example.core)

(defn mean [xs]
  (/ (double #p (reduce + xs)) #p (count xs)))
```

It's faster to type than `(prn ...)`, returns the original result, and produces more useful output by printing the original form, function and line number:

```
user=> (mean [1 4 5 2])
#p[example.core/mean:4] (reduce + xs) => 12
#p[example.core/mean:4] (count xs) => 4
3.0
```

## Install

### Leiningen

Add the following to `~/.lein/profiles.clj`:

```edn
{:user
 {:dependencies [[com.github.neolee/hashp "0.2.2"]]
  :injections [(require 'hashp.core)]}}
```

### Clojure CLI/deps.edn

You need [user.cli](https://github.com/gfredericks/user.clj) for automatic code injection. Add the following to `~/.config/clojure/deps.edn`:

```edn
:deps {com.gfredericks/user.clj {:mvn/version "0.1.0"},
       com.github.neolee/hashp {:mvn/version "0.2.2"}}
```

Then add the following code to `~/.clojure/user.clj`:

``` clojure
(require 'hashp.core)
```

### Boot

Add the following to `~/.boot/profile.boot`:

```clojure
(set-env! :dependencies #(conj % '[com.github.neolee/hashp "0.2.2"]))

(require 'hashp.core)
(boot.core/load-data-readers!)
```

### Shadow-CLJS

Add the following to `shadow-cljs.edn`:
```clojure
{:dependencies [com.github.neolee/hashp "0.2.2"]
 :builds {:app {:devtools {:preloads [hashp.core]}}}}
```

Or alternatively via `~/.shadow-cljs/config.edn` and `--config-merge`:

`~/.shadow-cljs/config.edn`:
```clojure
{:dependencies [[com.github.neolee/hashp "0.2.2"]]}
```

Run:
```
shadow-cljs watch app --config-merge '{:devtools {:preloads [hashp.core]}}'
```

## License

Based on [work](https://github.com/weavejester/hashp) by [James Reeves](https://github.com/weavejester).

Released under the MIT license.
