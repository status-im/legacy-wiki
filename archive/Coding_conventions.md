This document details coding style followed by `status-react` project.

It is based on following documents:

  - <https://github.com/bbatsov/clojure-style-guide>
  - <http://tonsky.me/blog/readable-clojure/>

Some points particularly relevant are refined bellow.

## Namespaces

No `:use`, `:refer` or `:refer :all`, only `:as`

    (ns examples.ns
      (:require [clojure.zip :as zip]))

Use consistent aliases across namespaces

Align keywords

    (ns examples.ns
      (:require [clojure.string :as str]
                     [clojure.zip     :as zip]))

Use long aliases when needed

## Comments

Comments start with `;;`

    ;; some comment

Comments should be added before form

    ;; public key of current logged in account
    (s/def ::current-public-key (s/nilable string?))

    ;; used in modal list (for example for wallet)
    (s/def :contacts/contacts-click-params (s/nilable map?))

## Re-frame

Declare keys on first line

```
 (reg-sub
    :photo-path
    :<- [:get :contacts]
    :<- [:get-contacts]
    (fn [contacts [_ id]]
      (:photo-path (contacts id))))

 (register-handler :request-discoveries
   ...)

+(reg-fx ::init-store
 +  (fn []
 +    (data-store/init)))
```

*TODO* Keywords and namespaces?

## Specs

Use meaningful namespaced keywords

    (s/def :contact/last-updated (s/nilable int?))