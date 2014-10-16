titan-node
==========

Wrapper around [gremlin-node](https://github.com/inolen/gremlin-node) to provide out of the box support for [Titan graph database](https://github.com/thinkaurelius/titan).

notes
=====
My current config has titan.props in my app's working directory (app meaning not this module, I just put it here because it will probably en up here). cassandra.yaml should be in config/ ......

The examples that are given for gremlin node and titan-node aren't very good, so I'm not 100% that all the features work yet.

gremlin-node does not include the jar files for Titan, nor does it make any assumptions based on using Titan as a backend. This project customizes gremlin-node to make it easier to get up and running:

 * includes the appropriate Java jar files for Titan 0.4.1 in the classpath
 * adds Titan-specific enums and data types to the gremlin object (e.g. Geo and Geoshape)
 * adds `loglevel` option to the gremlin constructor to control Titan verbosity. see [logback's documentation](http://logback.qos.ch/manual/architecture.html) for all available levels
 * passes the recommended runtime flags to the JVM instance instantiated by gremlin-node

## Installation

```bash
$ npm install titan-node
```

## Quick start

```javascript
var Titan = require('titan-node');
var gremlin = new Titan.Gremlin({ loglevel: 'OFF' });

var TitanFactory = gremlin.java.import('com.thinkaurelius.titan.core.TitanFactory');
var graph = TitanFactory.openSync('titan.props');
var g = gremlin.wrap(graph);

var vertices = g.V().toJSONSync(g);
console.log(vertices);


var GraphOfTheGodsFactory = gremlin.java.import('com.thinkaurelius.titan.example.GraphOfTheGodsFactory');

var graph = GraphOfTheGodsFactory.createSync('testdirectory');
var g = gremlin.wrap(graph);

g.V('name', 'saturn').next(function (err, saturn) {
  g.start(saturn).in('father').in('father').next(function (err, grandchild) {
    grandchild.getProperty('name', function(err, name) {
      console.log(name);
    });
  });
});
```
