<h2>Vocabulary</h2>

**Test Suite**- The entire set of tests that accompanies your program or application.
This is basically _all the tests_ for your project.

**Test** - Situation or context in which tests are run.  

An example of this is when you type a wrong password, you should get an error. A test can
have multiple assertions.

**Assertion** -- This is the verification step to confirm that the data returned
by your program or application is what you expected.  You can make one or more 
assertions within a test.

What is actually happening below?

```
require 'minitest/autorun'

require_relative 'car'

class CarTest < MiniTest::Test
  def test_wheels
    car = Car.new
    assert_equal(4, car.wheels)
  end
end
```

On line 1 is `require 'minitest/autorun'`, which loads all the necessary files from the `minitest` gem.

This is what we need to use for Minitest. Next, on line 3 we require the file that we're testing, `car.rb`,
which contains the `Car` class.  We are using `require_relative` to specify the file name from the current file's 
directory.  Now when we make references to the `Car` class, Ruby knows where to look for it.

On line 5, we finally create our test class.  This class must subclass `MiniTest::Test`.  This will allow our test class to 
inherit all the necessary methods for writing tests.

