# Understanding Ember.computed

## Basics

We probably all know what computed properties are, so I wont talk about that.

Though unlikely, it is possible that some might not know computed properties can be chained, meaning `computedPropertyA` can depend on `propertyA` and `propertyB` while `computedPropertyB` could then depend on `computedPropertyA` (and something else).

What I suspected, but wasn't aware how it's done up to this point is that **computed properties can be written to**. Here's how:

```JavaScript

computedProperty: function(key, value, previousValue) {
  // this is the getter
  if (arguments.length > 1) {
    this.set('propertyA', parseInt(value) - 1)
  }
  
  // this is the setter
  return parseInt(this.get('propertyA')) + 1;
}
```

Note that you **have to check for number of arguments**. This is how you determine that the property was called as a setter.

In case anyone is interested in how other libraries and frameworks do it, Knockout, as an example, defines a computed as `ko.computed(argument)`. If the `argument` is a function, then it's a read-only observable. If it's an object with a pair of functions called 'read' and 'write', then it's a writable computed. A knockout computed is is read from with `computedProperty()`, while it's written to with `computedProperty(value)`.
