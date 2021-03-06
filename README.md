## clonfig ##

[![Build Status](https://secure.travis-ci.org/mccraigmccraig/clonfig.png)](http://travis-ci.org/mccraigmccraig/clonfig)


simple environment variable based configuration for clojure apps.

config attributes are defined in a map, along with defaults and post-processor functions.

* attribute values are read from corresponding environment variables
* default values can be specified which are used if there is no environment variable
* value post-processors can be specified with a keyword, and transform the value
* post-processor functions can be passed directly and are passed the config map and
  the value and can therefore produce config attributes which depend on the value
  of other config attributes

## Usage ##

add the dependency to your project.clj

    [clonfig "0.1.0"]

define any environment variables you want before running a clojure process

    ENVIRONMENT=production lein repl

the read-config function produces a simple map of config attributes

    (use 'clonfig.core)

    (def config-defaults {:environment "development"
                          :port ["8080" :int]
                          :database-url ["postgresql://localhost/"
                                         (fn [config val] (str val @(:environment config)))]})

    (def config (read-config config-defaults))

    (:environment config)  ;; "production"
    (:port config)         ;; 8080
    (:database-url config) ;; "postgresql://localhost/production"

## License ##

Copyright (C) 2011 mccraigmccraig

Distributed under the Eclipse Public License, the same as Clojure.
