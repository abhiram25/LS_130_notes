<h1>Closure and binding</h1>

**Closure** - chunk of code you can pass around and execute at some later time.

In Ruby, this "chunk of code" or closure is represented by a `Proc` object

```
name = "Robert"
chunk_of_code = Proc.new {puts "hi #{name}"}
```

We can now pass around this local variable, `chunk_of_code` and execute it at any time later.

```Ruby
def call_me(some_code)
  some_code.call            # call will execute the "chunk of code" that gets passed in
end

name = "Robert"
chunk_of_code = Proc.new {puts "hi #{name}"}

call_me(chunk_of_code)
```

The output is `"hi Robert` and the return value is `nil`

Question?

How did the `chunk_of_code` handle the variable `name`?

```
def call_me(some_code)
  some_code.call
end

name = "Robert"
chunk_of_code = Proc.new {puts "hi #{name}"}
name = "Griffin III"        # re-assign name after Proc initialization

call_me(chunk_of_code)
```

The output is `hi Griffin III` and the return value is `nil`.

After reassigning the variable after the `Proc` is initialized updates the `chunk_of_code`.

This implies `Proc` keeps track of its surrounding context and drags it around wherever the chunk of code is passed to.

In Ruby, this is called `binding`, or surrounding environment/context.

A closure must keep track of its surrounding context in order to have all the information
it needs to be executed later.

This not only includes local variables, but also method references, constants, and other
artifacts in your code.  It will drag all of it around.

<h2>Symbol to Proc</h2>

When working with collections, we often want to transform all items in that collection.

We are given an array of integers and want to convert them into strings, we could do this.

```
[1,2,3,4,5].map do |num|
   num.to_s
end

# => ["1", "2", "3", "4", "5"]
```

You can see the return value for the above is a new array with every element now a string.

What If I told you there is a shortcut for this?

```
[1, 2, 3, 4, 5].map(&:to_s)                     # => ["1", "2", "3", "4", "5"]
```

The above code iterate through every element in the array and calls `to_s` on it, then
saves the result in a new array. After it's done iterating, the new array is returned.

After it returns another array you can chain another transformation.

```
[1, 2, 3, 4, 5].map(&:to_s).map(&:to_i)         # => [1, 2, 3, 4, 5]
```

Note: The `&:` must be followed by a valid method that can be invoked on each element.

<h2>Symbol#to_proc</h2>

How does it work?

`(&:to_s) => {|n| n.to_s}`

First we add `&` in front of an object, it tells Ruby to try to convert this object into
a block. To do so, it is expecting a `Proc` object.  If it is not a `Proc` object, it will call
`to_proc` on the object.

First Ruby see if the object after `&` is a `Proc`.  If it is not, it will try to call a `to_proc` on
the object, which should return a `Proc` object.  If not, this won't work.

Then the `&` will turn the Proc into a block.

This means that Ruby is trying to turn `:to_s` into a block, but it's not a `Proc`, it's a Symbol.

Ruby will then try to call `Symbol#to_proc` method -- and there is one.

This will return a Proc object, which will execute the method based on the name of the symbol.

Here are some examples

```
def my_method
  yield(2)
end

# turns the symbol into a Proc, then & turns the Proc into a block
my_method(&:to_s)               # => "2"
```

