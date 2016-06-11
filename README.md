# boot-cp

[](dependency)
```clojure
[org.clojars.micha/boot-cp "0.1.5"] ;; latest release
```
[](/dependency)

Classpath task for boot.

## Demo

```shell
# see task docs
boot -d org.clojars.micha/boot-cp with-cp --help
```
```shell
# write a classpath file
boot -d org.clojars.micha/boot-cp -d ring:1.4.0 with-cp -l lib -wf cp
```
```shell
# start a REPL with the claspath set up by the file
boot -d org.clojars.micha/boot-cp with-cp -f cp repl
```

## Usage

```clojure
;; build.boot or profile.boot

(set-env!
  :dependencies '[[org.clojars.micha/boot-cp "X.Y.Z" :scope "test"]])

(require
  '[micha.boot-cp :refer [with-cp]])

(task-options!
  with-cp {:safe          true
           :file          "deps.cp"
           :scopes        #{"compile"}
           :dependencies  '[[foo/bar "1.2.3"]
                            [baz/baf "4.5.6"]]})
```

Then, in the terminal in your project directory:

```shell
# write the classpath file `deps.cp'
boot with-cp -w
```
```shell
# load dependencies from the classpath file
boot with-cp task1 task2 ...
```
```shell
# use the classpath file to configure Java classpath
java -cp $(cat deps.cp) -jar target/project.jar
```

### Example &mdash; `deps.edn`

Suppose you want to be able to use `boot-cp` without a `build.boot` file etc.
for simple projects.

```clojure
;; profile.boot

(set-env!
  :dependencies '[[org.clojars.micha/boot-cp "X.Y.Z" :scope "test"]])

(require
  '[micha.boot-cp :refer [with-cp]])

(task-options!
  with-cp {:safe          true
           :file          "deps.cp"
           :scopes        #{"compile"}
           :local-repo    "lib"
           :dependencies  (when (-> "deps.edn" java.io.File. .isFile)
                            (read-string (slurp "deps.edn")))})
```

Then, in your project directory you can have just a `deps.edn` file containing
project's dependencies, and you can create a classpath file anytime:

```shell
boot with-cp -w
```

## Hacking on boot-cp

```shell
boot build-jar # build JAR and install to local Maven repo
```
```shell
boot build-jar push-release # build JAR, install, and push to Clojars
```
