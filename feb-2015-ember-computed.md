# Understanding Ember.computed

## Basics

We probably all know what computed properties are, so I wont talk about that.

Though unlikely, it is possible that some might not know **computed properties can be chained**, meaning `computedPropertyA` can depend on `propertyA` and `propertyB` while `computedPropertyB` could then depend on `computedPropertyA` (and something else).

What I suspected, but wasn't aware how it's done up to this point is that **computed properties can be written to**. Here's how:

```JavaScript

computedProperty: function(key, value, previousValue) {
  // this is the getter
  if (arguments.length > 1) {
    this.set('propertyA', parseInt(value) - 1)
  }
  
  // this is the setter
  return parseInt(this.get('propertyA')) + 1;
}.property('propertyA')
```

Note that you **have to check for number of arguments**. This is how you determine that the property was called as a setter.

In case anyone is interested in how other libraries and frameworks do it, Knockout, as an example, defines a computed as `ko.computed(argument)`. If the `argument` is a function, then it's a read-only observable. If it's an object with a pair of functions called 'read' and 'write', then it's a writable computed. A knockout computed is is read from with `computedProperty()`, while it's written to with `computedProperty(value)`.

## @each

`@each` is used when we want to create a computed property that relies on all the items in an array. Here's how it works in an example stolen from the Ember site.:

```JavaScript
App.TodosController = Ember.Controller.extend({
  todos: [
    Ember.Object.create({ isDone: true }),
    Ember.Object.create({ isDone: false }),
    Ember.Object.create({ isDone: true })
  ],

  remaining: function() {
    var todos = this.get('todos');
    return todos.filterBy('isDone', false).get('length');
  }.property('todos.@each.isDone')
});
```

Take note of the `.property('todos.@each.isDone')`. It's important. The `remaining` computed property will update if any of the following happens:
* `todos` is assigned a whole different array (because of the `todos` part in `.property('todos.@each.isDone')`)
* `todos` has an item added or removed (because of the `@each` part in `.property('todos.@each.isDone')`)
* `isDone` changes value for any item in the `todos` array (because of the `@each.isDone` part in `.property('todos.@each.isDone')`)

That's about as far as we can go, though. **We cannot go more than one level deep** when using `@each`. That means `todo.@each.isDone` works, but a hypothetical `todo.@each.subtask.isDone` or `todo.@each.subtask.@each.isDone` will absolutely not work.

### What about '[]'

Other than `@each`, there's also the `[]` 'selector'.

To explain the difference, some history:

Initially both were pretty much the same, with the difference that `[]` did not support observing properties of array elements, meaning `todos.@each.isDone` works, but `todos.[].isDone` does not. 

For a short time between Ember 0.9.4 and 0.9.8, `[]` was gone completely. The reason it was [re-added in 0.9.8](https://github.com/emberjs/ember.js/commit/d5e659bc8a8abaa3524bf66b7eb9b8f1bcdd18a3) was because, I quote:

>This were previously disabled due to over-zealous firing of firstObject
and lastObject. However, [] was still useful for non-array enumerables.
Additionally, firstObject and lastObject were made smarter so as to only
fire notifications when necessary.

So, basically, `[]` can be used with non-array enumerables, such as Ember.set for instance. For array enumerables, `@each` should probably be used.

## Observers

Observers are used when we want to fire an action when some property changes, without actually requiring a value to be returned. Simply put, we could consider them event handlers for `propertyChanged` events. **They can observe both basic as well as computed properties.**

```JavaScript
propertyA: null,

onPropertyAChanged: function () {
  // do something here
}.observes('propertyA')
```

### Observers are synchronous

This might cause problems in some cases. Again, stolen from the Ember site...

**The observer could fire at the wrong time**:

```JavaScript
lastNameChanged: function() {
  // The observer depends on lastName and so does fullName. Because observers
  // are synchronous, when this function is called the value of fullName is
  // not updated yet so this will log the old value of fullName
  
  // Personal note: I think the usage here is wrong anyway, but the auther 
  // probably had trouble finding an example with a correct usage that would still
  // create an issue, as do I :)
  console.log(this.get('fullName'));
}.observes('lastName')
```
**The observer could also fire multiple times when it really isn't necessary**:

```JavaScript
partOfNameChanged: function() {
  // Because both firstName and lastName were set, this observer will fire twice.
}.observes('firstName', 'lastName')
```

The solution to this is to make use of `Ember.run.once`:

```JavaScript
Person.reopen({
  partOfNameChanged: function() {
    // we schedule a proper callback instead of executing the required actions directly
    Ember.run.once(this, 'processFullName');
  }.observes('firstName', 'lastName'),

  processFullName: function() {
    // This will only fire once if you set two properties at the same time, and
    // will also happen in the next run loop once all properties are synchronized
    console.log(this.get('fullName'));
  }
});

person.set('firstName', 'John');
person.set('lastName', 'Smith');
```

Note to self: I should read up on the Ember run loop a bit more. I still don't understand it fully.

### Observers do not fire during object initialization, only after. 

This means that if we also want them to do something initally, when the properties they observe are being defined, we need to also bind the observer to the 'init' event:

```JavaScript
myObserver: function () {
  //do something
}.observes('propertyA', 'propertyB').on('init')
```

### If a computed property is not consumed (with 'get'), it's observers will not fire.

This is a consequence of the fact that computed properties are evaluated when consumed, so without them being consumed, there is no evaluation, so there is no change detection.

In the vast majority of cases, this is not an issue, but it can be an issue during object initialization, as with regular property observers, so we again make use of `on('init')`.


### Alternative ways to define an observer

* **If prototype extensions are switched off**, we can use `Ember.observer()`:
```JavaScript
onPropertyAChanged = Ember.observer('propertyA', function () {/* do something */})
```

* **From outside the class definition**, we can use `.addObserver()`

```JavaScript
propertyA.addObserver(function () {/* do something */})
```

### `observesBefore()` has been deprecated

Prior to Ember 1.7, there was a special observer defined with `observesBefore('propertyName')`. This one would fire when a property is about to change, but hasn't changed yet.

It has been deprecated in 1.7 and should not be used anymore. There is no specific alternative, but there are probably other solutions for most instances where it was used. Worst case scenario, once ould make a manual observesBefore by storing the new value of a property on each change into a separate variable and then looking up the value of that variable on future changes.

## Bindings

Bindings are sort of like computeds, except several of them work both ways by default (as getters and setters).

They can work on properties of the same object, across different objects or basically in any other way.`Ember.computed.alias` is the simplest two-way binding.

There are some optimizations in place so that bindings don't update immediately and instead wait until the application code is done running, so **it's not an issue to update bindings many times in a short period**.

`Ember.computed.oneWay` is an explicit way to define a one-way binding. In this instance, oneWay means that in the case of 
```JavaScript
  propertyA: Ember.computed.oneWay('propertyB');
```

If we now set the value of 'propertyB', the value of 'propertyA' will also change. If, however, we do it the other way around and set the value of 'propertyA', only 'propertyA' will change, while 'propertyB' will not. As a consequence, this will also **permanently diverge the values of the two properties**.

### Some less known, but possibly useful bindings

`Ember.computed.deprecatingAlias` is the same as `Ember.computed.alias` except it also prints a deprecation warning in the console when used. This is new since Ember 1.7 and could be useful in some cases.

`Ember.computed.readOnly` works sort of like `Ember.computed.oneWay`. The difference is that, instead of permanently diverging the value of the upstream property, it will instead throw an error when we attempt to set that value.

`Ember.computed.reads` is an alias for `Ember.computed.oneWay` which is "semanthically more sound", according to the ember documentation.




