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


