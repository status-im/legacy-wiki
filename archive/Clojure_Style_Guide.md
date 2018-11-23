This documents lists a set of conventions used by the development team,
refer to <https://github.com/bbatsov/clojure-style-guide> for anything
not addressed in here.

## Variable naming

### Use of '

Avoid using appending `'`(prime) to variables names, as this can be hard
to notice especially if visually impaired.

``` clojure

  (defn do-stuff [thing]
    (let [thing' (do-some-other-stuff thing)]))
```

Prefer threading, meaningful names or when not possible more readable
variable names.

## Imported namespaces

### Use of `:as` and `:refer`

Avoid using `:refer`, use `:as` instead

### Namespaces aliases

Use dot separated names when ambiguous, for example:
`status.ui.utils.clocks :as utils.clocks` as opposed to `:as clocks` or
`:as utils-clocks`

## Re-frame

### Prefer function composition instead of multiple dispatches

Prefer using function composition instead of dispatching when possible.
You can leverage the `merge-fx` macro, which assumes the function will
take `cofx` as the last positional parameter and returns a map of `fxs`
(or `nil`) which will be merged back.

In practice, functions/handlers should look like:

``` clojure

  (defn do-stuff [cofx]
    (merge-fx cofx
      db-fn
      some-other-fx-fn))
```

Opposed to:

``` clojure

(defn do-stuff [fx]
    (-> fx
      (update  :db db-fn)
      (merge {:dispatch [:some-other-fx]})
```

Therefore when writing new functions:

1\) Have them return a map of `fxs`.

2\) Have `cofx` as the last parameter, in order to be able for others to
use them with `merge-fx`.

3\) Operate on `cofx` rather than `db`, so that new functions can be
threaded.

You can read <https://github.com/status-im/ideas/issues/31> for a
rationale.