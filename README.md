# A JSON-schema $ref faker

[![Build Status](https://travis-ci.org/forivall/refaker-local-id.png?branch=master)](https://travis-ci.org/forivall/refaker-local-id) [![NPM version](https://badge.fury.io/js/refaker-local-id.png)](http://badge.fury.io/js/refaker-local-id) [![Coverage Status](https://coveralls.io/repos/forivall/refaker-local-id/badge.png?branch=master)](https://coveralls.io/r/forivall/refaker-local-id?branch=master)

Inspired on [json-schema-validator](https://github.com/fge/json-schema-validator) for JAVA, the `--fakeroot` option, actually.

This fork also allows non-standard references via `"id"`.

If you're validating some RAML (like me) then you should validate your JSON-schemas and examples also.

For this purpose I'm using [ramlev](https://github.com/cybertk/ramlev) which is using [tv4](https://github.com/geraintluff/tv4), but you're encouraged to download any `$ref` manually before validating.

## Solution?

```javascript
var tv4 = require('tv4'),
    refaker = require('refaker-local-id');

var data = { /* ... */ },
    schema = { /* ... */ };

refaker({
  schema: schema,
  fakeroot: 'http://example.com',
  directory: '/path/to/schemas'
}, function(err, refs, schemas) {
  if (err) {
    console.log(err);
  } else {
    for (var id in refs) {
      // register resolved refs
      tv4.addSchema(id, refs[id]);
    }

    // validates the first passed schema
    console.log(tv4.validateResult(data, schemas[0]));
  }
});
```

That's it.

## Options

- **schema** (object|aray)

  The JSON-schema to validate.

- **schemas** (object|array)

  Multiple JSON-schemas to validate at once.

  This is an alias for the **schema** option.

- **fakeroot** (string)

  If provided, any matching `$ref` will be resolved under the specified `directory`.

- **directory** (string)

  A local path containing the JSON-schemas.

  If missing, will use `process.cwd()` instead.

- **timeout** (number)

  Timeout when resolving remote schemas, default to 200ms.

Any `$ref` found will be downloaded or faked locally.

## Callback

The given callback will receive three arguments:

- **err** (mixed)

  Empty means success.

- **refs** (object)

  Hash of resolved `$ref`s.

- **schemas** (array)

  Normalized schemas if success (same order as input).


[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/forivall/refaker-local-id/trend.png)](https://bitdeli.com/free "Bitdeli Badge")
