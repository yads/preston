# mongoose-restifier

Serves Mongoose models on an extensible RESTful API.

[![build status](https://secure.travis-ci.org/simplyianm/mongoose-restifier.png)](http://travis-ci.org/simplyianm/mongoose-restifier)

## Installation

This module is installed via npm:

```bash
$ npm install mongoose-restifier
```

## Example

The following example serves the `User` and `Page` models on a RESTful API.

```js
var express = require('express');
var restifier = require('mongoose-restifier');
var models = require('./models'); // Your mongoose models would be loaded here

var app = express();

app.use(require('body-parser').json()); // Required

app.use(restifier.initialize()); // Sets up restifier
app.use(restifier.model(mongoose.model('User')).middleware());
app.use(restifier.model(mongoose.model('Page')).middleware());

app.listen(3000);
```

## Querying

The path uses the MongoDB collection name, so the `User` model would create routes under `/users`.

```
GET /users
```

### Equality

Checking for equality is as simple as setting the parameter equal to the value.

```
GET /users?param=value
```

### Sort

The `sort` parameter takes a comma-delimited array of field names. A `-` prefix denotes a descending order.

```
GET /users?sort=field,-field2
```

`field` would be sorted ascending, and `field2` is sorted descending.

### Limit/Skip

The limit/skip parameters (useful for pagination) are pretty self-explanatory.

```
GET /users?limit=10&skip=10
```

### Population

Takes a comma-delimited array of fields to populate.

```
GET /users?populate=posts,comments
```

### Filters

Anything more complicated than the use cases above should use a filter. A filter is a function which modifies the query.
Filters are applied to the query before the parameters listed above are.
There are several filters built into `mongoose-restifier`, which will be listed below.

#### Comparison Filters
These are modeled off of the [MongoDB Comparison Query Operators](http://docs.mongodb.org/manual/reference/operator/query-comparison/).

* `gt <field> <value>` - Matches where the field is greater than the given value.
* `gte <field> <value>` - Matches where the field is greater than the given value.
* `in <field> <value>` - Matches where the field is greater than the given value.
* `lt <field> <value>` - Matches where the field is greater than the given value.
* `lte <field> <value>` - Matches where the field is greater than the given value.
* `ne <field> <value>` - Matches where the field is greater than the given value.
* `nin <field> <value>` - Matches where the field is greater than the given value.

