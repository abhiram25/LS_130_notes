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

The method being called is still `Array#each`

The block is an argument to the method call

<h2>Writing methods that take blocks</h2>

Every method you have written in Ruby already takes a block.

```
def hello
  "hello!"
end

hello  => "hello!"
```

Let's run this code

`hello {puts 'hi'} => "hello!"` 

This is unexpected.  Passing in a block to a method is not quite like passing in 
any other parameter.

<h2>Enter Yielding</h2>

One way that we can invoke the passed-in block argument from within the method 
is by using the `yield` keyword.

def echo_with_yield(str)
  yield
  str
end

```
echo_with_yield("hello!") { puts "world" }  # world
                                            # => "hello!"
```

```
echo_with_yield { puts "world" }                        # => ArgumentError: wrong number of arguments (0 for 1)
echo_with_yield("hello!") { puts "world" }              # world
                                                        # => "hello!"
echo_with_yield("hello", "world!") { puts "world" }     # => ArgumentError: wrong number of arguments (2 for 1)
```

Two points to remember

1. The number of arguments at method invocation needs to match the method definition, regardless of
  whether we are passing in a block.

2. The `yield` keyword executes the block.

```
echo_with_yield("hello!")                          # => LocalJumpError: no block given (yield)
```

The error is that there is no block given.

If we want to allow calling the method with or without a block, we must somehow wrap the `yield`
call in a conditional.

The conditional is only call `yield` when a block is given.

```
def echo_with_yield(str)
  yield if block_given?
  str
end
```

Now we can call `echo_with_yield` if a block is given

```
echo_with_yield("hello!")                          # => "hello!"
echo_with_yield("hello!") { puts "world" }         # world
                                                   # => "hello!"
```

<h2>Yielding with an argument</h2>

```
3.times do |num|
  puts num
end
```

`3` is the calling object
`times` is the method being called

```
do |num|
  puts num
end
```

The `do...end` is the block.  The `num` variable between the `|` is the 
argument to the block.  Within this block, `num`  is a _block local variable_.

This local variable is constrained to the block.

```Ruby
# method implementation
def increment(number)
  number + 1
end

# method invocation
increment(5)                            # => 6
```

1. Execution starts at method invocation on line 11
2. Execution then move to method implementation on line 2, which sets 5
   to local variable `number`, the block is not set to any variable, it's
   implicitly available.
3. Execution continues on line 3, which is a conditional.
4. The conditional is true because we did pass in a block.
5. On line 4, the block is called and we pass `number + 1` to the block.
   We are calling the block with 6 as the block argument.
6. Execution jumps to line 11, where the block local variable `num` is
   assigned to `6`.
7. Execution is now at line 12, we output the block local variable `num`
8. Execution continues to line 13, where the end of the block is reached.
9. Execution is now at method implementation, where we finished execution on
   line 4, then we bypass the else condition and continue to line 8 where the
   method ends.


What would happen if we had a wrong number of arguments passed into a block?

```Ruby
def test
  yield(1, 2)                           # passing 2 block arguments at block invocation time
end

# method invocation
test { |num| puts num }
```

The above code outputs `1`.  The extra block argument is ignored. What happens if we pass in one 
less block argument?

```
def test
  yield(1)                              # passing 1 block argument at block invocation time
end

# method invocation
test do |num1, num2|                    # expecting 2 parameters in block implementation
  puts "#{num1} #{num2}"
end
```
                                            
This still outputs `1`.  Why?

In this case, `num2`, the block local variable is `nil`.  So the string interpolation converted that
to an empty string.

`nil.to_s == "" => true`

Blocks are one way Ruby implements the idea of a _closure_.  The other two are instantiating a `Proc`
object and using `lambda`.  

The rules around enforcing the number of arguments you can call on a closure in Ruby is called
its _arity_.  In Ruby, blocks have lenient arity, which is why it doesn't complain when you pass in 
different number of arguments.

`Proc` objects and `lambda`S have different arity rules.

<h2> Return value of yielding to the block </h2>




