Q: Mongoose: mpromise (mongoose's default promise library) is deprecated, plug in your own promise library instead: [http://mongoosejs.com/docs/promises.html](http://mongoosejs.com/docs/promises.html)

A:

__Built-in Promises__

```js
var gnr = new Band({
  name: "Guns N' Roses",
  members: ['Axl', 'Slash']
});

var promise = gnr.save();
assert.ok(promise instanceof require('mpromise'));

promise.then(function (doc) {
  assert.equal(doc.name, "Guns N' Roses");
});
```

__Queries are not promises__

```js
var query = Band.findOne({name: "Guns N' Roses"});
assert.ok(!(query instanceof require('mpromise')));

// A query is not a fully-fledged promise, but it does have a `.then()`.
query.then(function (doc) {
  // use doc
});

// `.exec()` gives you a fully-fledged promise
var promise = query.exec();
assert.ok(promise instanceof require('mpromise'));

promise.then(function (doc) {
  // use doc
});
```

## Promises for the MongoDB Driver

```js
var uri = 'mongodb://localhost:27017/mongoose_test';
// Use bluebird
var options = { promiseLibrary: require('bluebird') };
var db = mongoose.createConnection(uri, options);

Band = db.model('band-promises', { name: String });

db.on('open', function() {
  assert.equal(Band.collection.findOne().constructor, require('bluebird'));
});
```
