# salyne
[![Build Status](https://drone.io/github.com/from-nibly/salyne/status.png)](https://drone.io/github.com/from-nibly/salyne/latest)

Salyne (saline) is a dependency injection library that is largely compatible with electrolyte but is lighter, simpler, and more flexible.

here is a quick example.
```js
// ~/project/index.js  
var Salyne = require('salyne'),
  injector = new Salyne();

injector.load('./lib/foobar.js')

var foobar = injector.create('foobar');

console.log('testing', foobar.foo, foobar.bar);

// ~/project/lib/foobar.js
exports = module.exports = function() {
  this.foo = 5;
  this.bar = 10;
};
```
This will create a new instance of the foobar dependency. It got its name from the file name. But like I said earlier salyne is flexible. Here are some of the ways you can name your dependencies.

```js
//salyne will grab from the function name if it's available
exports = module.exports = function Foobar() {
  // name will be 'Foobar'
};
// you can also pass a name at load time
injector.load('FooBar', './lib/foobar.js');
// name will be FooBar
```

I also said that this is a dependency injection library. Here are some ways you can define your dependencies inside your class.

```js
// salyne will take just the argument names just like angular (but they have to be exact)
exports = module.exports = function (foo, bar, bang) {
};
// saylne will also take properties on the function (the params no longer have to be exact or even close)
exports - module.exports = function (foo, bar, bang) {

};
exports.requires = [ 'Foo', 'Baur', 'Zip']
//you can also do the following
exports['@requires'] = [ 'Foo', 'Baur', 'Zip']
exports.require = [ 'Foo', 'Baur', 'Zip']
exports['@require'] = [ 'Foo', 'Baur', 'Zip']
```

You can also create a singleton really easily!

```js
var foo = 0;
exports = module.exports = function() {
  //foo will never change because you will always be given the same instance.
  this.foo = foo++;
};
exports.singleton = true;
//you can also do the following
exports['@singleton'] = true;
```

You can also bind constructors directly.

```js
injector.bind('bar', function() {
  this.item = 'testing';
});
var bar = injector.create('bar');
```

These are also flexible.
```js
injector.bind({ singleton : true }, function Bang() {
  this.bang = 'also testing';
});
var bang = injector.create('Bang');
```
