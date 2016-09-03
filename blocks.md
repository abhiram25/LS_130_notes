<h1>Closures</h1>

A **closure** is a programming concept which allows programmers to save a _chunk of code_ and use it at a later time.

It's called a **closure** because it's said to bind it's variables, methods, objects, and etc. and build an **enclosure** 
around everything, so that they can be referenced when the closure is later executed.

In Ruby, a closure is implemented through a `Proc` object. We can pass around a `Proc` object as a _chunk of code_
and execute it later.

The `Proc` object or _chunk of code_ retains references its surrounding artifacts -- its **binding**.

3 Main ways to work with closures in Ruby(or `Proc` class)

1. Instantiating an object from the `Proc` class
2. Using **lamdas**
3. Using **blocks**

```Ruby
[1, 2, 3].each do |num|
  puts num
end
```

The object we are working with is the collection or `Array` object
```
[1,2,3]
```

The method we are calling on the object is `Array#each`

The part that's a block is the `do..end` part

```
do |num|
  puts num
end
```


