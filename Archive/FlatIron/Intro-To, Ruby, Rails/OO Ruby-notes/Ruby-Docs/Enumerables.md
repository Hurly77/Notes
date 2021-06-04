

**MIXIN** - A **mixin** is a **class** or **interface** in which some or all of its methods and/or properties are **unimplemented**, requiring that **another class** or **interface provide** the **missing implementations**. The **new class** or interface then i**ncludes both the properties and methods** from the **mixin** as well as those it defines itself

## Enumerable

The Enumerable - The [Enumerable](https://ruby-doc.org/core-2.7.0/Enumerable.html) mixin provides collection classes with several traversal and searching methods, and with the ability to sort. The class must provide a method each, which yields successive members of the collection. If [#max](https://ruby-doc.org/core-2.7.0/Enumerable.html#method-i-max), [min](https://ruby-doc.org/core-2.7.0/Enumerable.html#method-i-min), or [sort](https://ruby-doc.org/core-2.7.0/Enumerable.html#method-i-sort) is used, the objects in the collection must also implement a meaningful `<=>` operator, as these methods rely on an ordering between members of the collection.

### find_all  

Returns an array containing all elements of `enum` for which the given `block` returns a true value.