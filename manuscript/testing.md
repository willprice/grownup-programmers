# Why do we need programs to test our programs?

Imagine you're tunnelling out a top secret underground base. You drill through the ground, carving out rooms and corridors that are all connected. There's great danger in this sort of work. The more tunnelling you do, the bigger the risk that vibrations will cause another part of the base to collapse.

Working on larger programming projects is a bit like this. Computer programs are made up of parts that, like the tunnels in our underground base, are all connected. Changing one part of the code can break other parts connected to it. As the program grows, so does the risk that even the tiniest change might cause it to collapse.

In our secret underground base, if an existing tunnel collapses, we will have to go back and dig it out again. While we're digging that tunnel out the second time, another connected tunnel might collapse. So we will have to dig that one out again, too. Which might cause other tunnels to collapse. And around and around we go!

The same can happen when we are programming. While we're writing one part of our code, we may break a connected part. While we fix that connected part, we may break other parts of the code.

If you were building an underground base, you'd reinforce the tunnels you'd dug out with strong supports to guard against collapse.

![Tunnel supports image](./media/images/tunnel_supports)

Grown-up programmers do something similar; they support the code they've already got working by re-testing it to check if anything is broken, catching new bugs before they can do any serious damage.

A large programming project may need hundreds – sometimes even thousands – of tests to be sure everything's working as it should. If we had to run the program and enter data and click buttons to perform all these tests, it could take days, or even weeks, to check everything is still working. We wouldn't get much new coding done – we'd be too busy testing!

This is why grown-up programmers get the computer to perform their tests for them. They write their tests as the program grows, making sure each new part of the program has tests to support it. They don't leave it to the end (which would be as dangerous as putting in all the supports for your underground base after you've dug out all the tunnels.)

What might take us days or weeks, the computer could do in minutes or even seconds.

# How do we write programs that test programs?

How you do it is quite simple: just write some code that calls the functions in your program and checks to see they're producing the results you expect.

```python
#!/usr/bin/env python3

def average(numbers):
    return sum(numbers) / len(numbers)

def test_average():
    # setup test data
    numbers = [1,2,3]

    # function to be tested
    avg = average(numbers)

    # check result
    if(avg == 2):
       print("Test passed")
    else:
        print("Test failed. Expected 2 but was " + str(avg))

# run test
test_average()
```

In this example, we wrote a test function that calls average() with a list of three numbers – 1, 2 and 3 – that we know have an average of 2. If average() does the calculation correctly, the test program outputs a message to say the test has passed. Otherwise, it tells us the test has failed.

Notice the three parts of the test:

1. We set up the test data
2. We call the function we want to test with the test data
3. We perform a check on the result to see if it matches what we expect

Tests like these tend to always have these three parts: the setup, the function call. and then the check.

It's just one test for a very simple function, though. A much bigger program might require many tests like this, and we would find ourselves repeating some of the code over and over again. For example, the code in the checking part of the test that outputs the results is probably going to be quite similar in every test.

And we'd also have to remember to add a call to each new test function to run it as part of the set of tests for your program. That can get a bit boring and repetitive after a while, and we might forget to do it from time to time, which means some tests won't get run. That's like having missing supports in our underground tunnels – dangerous!

# PyUnit to the rescue!

Luckily, somebody already thought of this. Python comes with a built-in module – called unittest (also known as "PyUnit") – for writing tests that can run all our test functions automatically.

```python
#!/usr/bin/env python3

import unittest

def average(numbers):
    return sum(numbers) / len(numbers)

class TestAverage(unittest.TestCase):
    def test_average(self):
        self.assertEqual(2, average([1,2,3]))

unittest.main()
```

Let's take a look at the key parts of this code to see what's going on.

```python
import unittest
```

This is the module that we can use to write and run what grown-up programmers call "unit tests". More about them in a minute.

```python
class TestAverage(unittest.TestCase):
```

When we run our tests, unittest will search through our code for classes that extend unittest.TestCase.

```python
def test_average(self):
```

Then it will look inside each test class for test methods: these are the methods that contain test code. Test method names begin with "test_" – that's how unittest finds them – and test methods must not accept any parameters or return any data. unittest will call every test method it finds in our code that matches that pattern, saving us the bother of having to remember to add those calls to our code.

If our test classes don't extend unittest.TestCase, and our test method names don't begin with "test_" and have no parameters, unittest won't know they are tests and won't run them.


```python
self.assertEqual(2, average([1,2,3]))
```

This is the body of our test. Although it's only one line of code, it has the three parts of a test that our old version had – just all rolled into one line.

We set up our test data – `[1,2,3]`
We call the function we want to test – `average()` – with the data
We check the result, using a method of `TestCase` called `assertEqual()`
"Assert" (like in `assertEqual`) is just another name for "check": it simply means "ask a question about the result, the answer to which can either be true or false". In this test, we ask "is the `average()` of `[1,2,3]` equal to 2?"

`TestCase` offers many kinds of built-in assertion methods for asking different kinds of questions. We'll see more of those later.

```python
unittest.main()
```

Finally, we call unittest's main() function to run our tests. The unittest module will search our code for test classes and run any test methods it finds in them, reporting the results like this:

```
.
———————————————————————-
Ran 1 test in 0.004s
OK
```

That tells us that our test has passed. Phew!

If any tests fail, we get a message like this:

```
F
======================================================================
FAIL: test_average (__main__.TestAverage)
———————————————————————-
Traceback (most recent call last):
File "C:\Users\jasongorman\Documents\python3_programs\grownup\averagetest.py", line 11, in test_average
self.assertEqual(2, average([1,2,3]))
AssertionError: 2 != 3.0
———————————————————————-
Ran 1 test in 0.008s
FAILED (failures=1)
```

This tells us that one of our test's assertions failed: that is, the answer to our question was false. Helpfully, it supplies useful information we can use to find and fix the problem.

- Which tests failed
- Which assertion(s) in those tests caused them to fail
- Where in the test code we can find them
- What the expected and actual results were
- Whether our tests pass or fail

It also tells us how long they took to run (in this case, 0.008 seconds, or 8 milliseconds.) It's worth keeping an eye on this number. As we write more tests, they will take longer to run. The longer they take to run, the less often we can run them. Time between test runs is like the space between supports in our underground base. The wider the spaces, the greater the risk that they won't offer enough support. That's why it's important to have tests that run quickly.

# What are "Unit Tests"?

When car manufacturers test cars: they don't just test the whole car once it's built, for example by taking it out on a test race track. They also test all the individual parts of the car to make sure they all do what they're supposed to.

This allows them to focus tests on specific things, like whether or not the brakes work, or whether or not the ignition system works, so when something goes wrong, it's much easier to spot where the problem is.

Similarly, when we test our program, we don't just need to know if it's broken; we also need to know where it's broken. Is it the function that calculates a pupil's age in years, months and days that's gone wrong, or is it the code that formats that age for display on the screen?

By writing unit tests, we can focus our questions on parts of the code that do specific work, making it far easier to figure out where the problem is, and saving us a lot of time debugging.

In fact, some programmers write such good unit tests that they rarely have to use a debugger at all.

# How do we organise our code for testing?

Grouping our tests together in a logical way helps us to find them when we need to.

If you've got a bunch of tests for a DiscountCalculator class, it makes sense to put them in a TestDiscountCalculator test class, so when someone asks "where are the tests for DiscountCalculator?", the answer is obvious.

```python
#!/usr/bin/env python3

import unittest

class TshirtOrder:
    def __init__(self, tshirts):
        self.tshirts = tshirts

    def get_tshirt_count(self):
        return len(self.tshirts)

    def get_total_cost(self):
        return self.get_tshirt_count() * 10 #every t-shirt costs £10


class DiscountCalculator:
    def __init__(self, tshirtOrder):
        self.tshirtOrder = tshirtOrder

    def calculate_discount(self):
        tshirts = self.tshirtOrder.get_tshirt_count()
        total = self.tshirtOrder.get_total_cost()

        discount = 0 # no discount for just 1 t-shirt

        if(tshirts > 1 and tshirts <= 3):
            discount = total * 0.1 #10% discount for 2-3 t-shirts

        if(tshirts > 3 and tshirts <= 5):
            discount = total * 0.2 #20% discount for 4-5 t-shirts

        if(tshirts > 5):
            discount = total * 0.3 #30% for more than 5 t-shirts

        return discount


class TestTshirtOrder(unittest.TestCase):
    def test_order_of_two_tshirts_has_totalcost_twenty(self):
        tshirts = ["White:S", "White:S"]
        order = TshirtOrder(tshirts)
        self.assertEqual(20, order.get_total_cost())


class TestDiscountCalculator(unittest.TestCase):
    def test_no_discount_on_single_tshirt(self):
        tshirts = ["White:S"]
        order = TshirtOrder(tshirts)
        self.assertEqual(0, DiscountCalculator(order).calculate_discount())

    def test_ten_percent_discount_on_two_tshirts(self):
        tshirts = ["White:S", "White:S"]
        order = TshirtOrder(tshirts)
        self.assertEqual(2, DiscountCalculator(order).calculate_discount())

    def test_twenty_percent_discount_on_two_tshirts(self):
        tshirts = ["White:S", "White:S", "White:S", "White:S"]
        order = TshirtOrder(tshirts)
        self.assertEqual(8, DiscountCalculator(order).calculate_discount())

    def test_thirty_percent_discount_on_six_tshirts(self):
        tshirts = ["White:S", "White:S", "White:S", "White:S", "White:S", "White:S"]
        order = TshirtOrder(tshirts)
        self.assertEqual(18, DiscountCalculator(order).calculate_discount())


unittest.main()
```

Grown-up programmers organise their code into separate modules, with the name of the module clearly describing what code it contains so it is easier to find. They also separate their tests from the code being tested, so it's easy to see what's test code and what isn't.

```
tshirts/
    |---- __init__.py
    |---- tests.py
    |---- main.py
    |---- root/
        |---- __init__.py
        |---- src/
            |---- __init__.py
            |---- discountcalculator.py
            |---- tshirtorder.py
        |---- test/
            |---- __init__.py
            |---- testdiscountcalculator.py
            |---- testtshirtorder.py
```

Notice how we provide two runnable modules, one for testing and one to run the program itself. The tests.py module contains code that runs all of the tests in all packages, using code like this:

```python
#!/usr/bin/env python3

import unittest

if __name__ == '__main__':
    testsuite = unittest.TestLoader().discover('.')
    unittest.TextTestRunner(verbosity=1).run(testsuite)
```

The code uses the unittest.TestLoader to discover all the test classes in all files that follow the pattern "test*.py", so all our tests must be contained in files starting with "test" or it won't find them. It adds all the tests it finds to the collection, which we can then use the unittest.TextTestRunner to run.

# Asking the right questions

In all the examples so far, we've used assertEqual() to check that a result matches the value we expect. PyUnit has other built-in assertion methods we can use to do other kinds of checks.

```python
def test_less_than_two_tshirts_do_not_qualify_for_discount(self):
    tshirts = ["White:S"]
    order = TshirtOrder(tshirts)
    self.assertFalse(DiscountCalculator().qualifies_for_discount(order))

def test_more_than_one_tshirt_qualifies_for_discount(self):
    tshirts = ["White:S", "White:S"]
    order = TshirtOrder(tshirts)
    self.assertTrue(DiscountCalculator().qualifies_for_discount(order))
```

In this example, we use assertTrue() and assertFalse() to check if a condition is met (or not met) as we expect.

```python
def test_empty_orders_are_not_allowed(self):
    with self.assertRaises(InvalidOrderException):
        order = TshirtOrder([])
```

And in this test, we check that an exception is raised when we try to create a t-shirt order without any t-shirts in it.

The kind of assertion we make will depend on the question we want to ask. And that will depend on what we expect the result to be. Do we expect a certain value to be returned, or for a variable to change? Do we expect some condition to be satisfied? Do we expect an exception will be thrown?

Tests that ask the wrong questions can leave gaps in the support our tests give us. Let's say, for example, we want to add t-shirts to our orders after they've been created:

```python
def test_tshirt_added_to_order(self):
    tshirts = ["White:S", "White:S"]
    order = TshirtOrder(tshirts)
    order.add("Black:XXL")
    self.assertEqual(3, order.get_tshirt_count())
```

At first glance, this test might seem reasonable. But how easy would it be for our code to pass this test but actually be doing the wrong thing?

```python
def add(self, tshirt):
    self.tshirts.append("") #obviously not right!
```

What we actually want to know is if "Black:XXL" has been added to the order, not just whether the count of t-shirts has gone up by one. A better test might be something like:

```python
def test_tshirt_added_to_order(self):
    tshirts = ["White:S", "White:S"]
    order = TshirtOrder(tshirts)
    order.add("Black:XXL")
    self.assertEqual(3, order.get_tshirt_count())
    self.assertTrue(order.contains("Black:XXL"))
```

But even this doesn't guarantee that our source code must be doing the right thing. Can you think of ways to pass this test without actually adding the t-shirt to the order?

# Testing your tests

Having tests that we can run quickly can give us a warm fuzzy feeling that our code is working. But perhaps our tests are lulling us into a false sense of security. Grown-up programmers know that, just because their tests all passed, that doesn't necessarily mean their program is definitely 100% working.

The tests you write are like a police force; their job is to detect crimes in your code ("bugs"), like real police detect crimes in your neighbourhood. How could we test how effective the police are at detecting crimes?

One way might be to stage crimes and see if the criminal gets caught. You know the crime happened, because you deliberately committed it. Did you get caught? If not, then maybe policing in your neighbourhood needs to be strengthened.

We can do this in code, too; deliberately introduce a bug into the program, then run the tests and see if any of them fail. If no tests catch the bug, then maybe they need to be stronger.

We could deliberately introduce a simple programming error into our code, like switching * with / in a mathematical expression:

```python
        if(tshirts > 1 and tshirts <= 3):
            discount = total / 0.1 #10% discount for 2-3 t-shirts
```

Then run our tests and see if any of them catch the bug:

```
…F…..
======================================================================
FAIL: test_ten_percent_discount_on_two_tshirts (root.test.testdiscountcalculator.DiscountCalculatorTests)
———————————————————————-
Traceback (most recent call last):
File "C:\Users\jasongorman\Documents\python3_programs\tshirts\root\test\testdiscountcalculator.py", line 27, in test_ten_percent_discount_on_two_tshirts
self.assertEqual(2, DiscountCalculator().calculate_discount(order))
AssertionError: 2 != 200.0
———————————————————————-
Ran 9 tests in 0.009s
FAILED (failures=1)
```

Phew! One of our tests caught it. So we can be confident that part of our code is being tested effectively – that is to say, if we broke it, a test would catch it.

We recommend you test your tests while you're writing them, so you can focus on one test at a time and only a small part of the code being tested. For every test, make sure it does fail when the result is wrong.

# Fake the stuff you don't want in your unit tests

Grown-up programs often use the functions of external programs. Your program might use a database like MySQL, or read and write files using the file system on the computer, or use the web services of sites like Facebook or PayPal.

All these things slow our code down, and dramatically increase the time it takes to properly unit test our program.

How can we write code that uses external programs, but can also be quickly unit tested? The answer lies in faking the bits of our code that know about these external programs.

Take this example, that reads a list of daily temperature readings from a file, and then calculates their average:

```python
class WeatherStatistics:
    def average_daily_temperature(self):
        with open("data/temperature.dat") as file:
            daily_entries = file.readlines()

        sum_of_temperatures = 0.0

        for temperature in daily_entries:

            sum_of_temperatures += float(temperature)

        return sum_of_temperatures / len(daily_entries)
```

To write a test for this, we'll need to create a file of temperature readings that we know the average of, and put it where our code expects to find it:

```
20
22
18
16
```

Our test might look like this:

```python
class TestWeatherStatistics(unittest.TestCase):
    def test_average_daily_temperature_is_sum_divided_by_count(self):
        self.assertEqual(19.0, WeatherStatistics().average_daily_temperature())
```

The problem here is that WeatherStatistics does both pieces of work – it reads the data from the file, and it calculates the average daily temperature. So we always get one with the other.

What if we split up the work, giving the job of fetching the data to one object, and calculating the average to another?

```python
class TemperatureData:
    def read(self):
        with open("data/temperature.dat") as file:
            return file.readlines()
```

We make it so that TemperatureData is passed as a parameter of a constructor (__init__() ) to WeatherStatistics. We'll see why this is important soon.

```python
class WeatherStatistics:
    def __init__(self, temperature_data):
        self.temperature_data = temperature_data

    def average_daily_temperature(self):
        daily_entries = self.temperature_data.read()

        sum_of_temperatures = 0.0

        for temperature in daily_entries:

            sum_of_temperatures += float(temperature)
```

The test code creates a TemperatureData object and plugs it into the WeatherStatistics object using the constructor.

```python
class TestWeatherStatistics(unittest.TestCase):
    def test_average_daily_temperature_is_sum_divided_by_count(self):
        data = TemperatureData()
        self.assertEqual(19.0, WeatherStatistics(data).average_daily_temperature())
```

So far, so good. But our code still reads from the file when we run the test. Now for the magic part:

```python
class FakeTemperatureData(TemperatureData):
    def read(self):
        return ["20", "22", "18", "16"]


class TestWeatherStatistics(unittest.TestCase):
    def test_average_daily_temperature_is_sum_divided_by_count(self):
        data = FakeTemperatureData()
        self.assertEqual(19.0, WeatherStatistics(data).average_daily_temperature())
```

We extend `TemperatureData` and override the method that returns the data entries so that we're just creating the list in code. The test then creates WeatherStatistics using this object instead of the real one that reads from a file, swapping a real TemperatureData object with a fake one.

This is good program design, generally. Now we can get temperature data from different sources (e.g., a MySQL database), and we won't have to change a line of code in WeatherStatistics.

As we see in the chapter "Grown-up Programmers Write Code That Is Easy To Change", it's a good idea to break our programs down into small objects that do one specific job, and plug them together like this so they can be easily swapped with objects that look the same from the outside, but do things differently inside.

# Different kinds of testing

So far, we've looked at unit testing, which focuses our questions on specific parts of our program.

But just because all the parts of our program are working correctly on their own, that doesn't mean when we plug them all together, the program will work as a whole. It's important to have some tests that check the different parts of our program work when we connect them together.

In our WeatherStatistics project, we might write a test for TemperatureData that checks daily data entries are read correctly from the file:

```python
class TestTemperatureData(unittest.TestCase):
    def test_reads_daily_entries_from_file(self):
        self.assertEqual(["20\n", "22\n", "18\n", "16"], TemperatureData().read()
```

This is testing that our code and the file system work together. Grown-up programmers call tests that check pieces of our program work together integration tests.

We might also want a test that checks that our WeatherStatistics program – as a whole – works when the data is being read from a file.

This is the equivalent of taking a new car out on to the track for a spin, just to be sure it works as a complete car. In actual fact, car manufacturers also automate their system tests, using machines that can simulate a variety of road conditions.

Grown-up programmers call tests that check the program works a whole system tests.

![Road simulator image](./media/images/roadsimulator.jpg)

Tests that involve things like reading files usually run far slower than unit tests that fake it. For this reason, it's a very good idea to rely more on fast-running unit tests to check that our code works, and have fewer integration and system tests.

It's also a very good idea to keep the different kinds of tests separate – by putting them in separate packages, and giving them their own test runner modules – so we can choose to run just the unit tests, or just the system tests, or all of the tests, and so on.

```
weatherstatistics/
    |---- __init__.py
    |---- alltests.py
    |---- integrationtests.py
    !---- systemtests.py
    |---- unittests.p
    |---- main.py
    |---- root/
        |---- src/
            |---- __init__.py
            |---- temperaturedata.py
            |---- weatherstatistics.py
        |---- test/
            |---- integration/
                |---- __init__.py
                |---- testtemperaturedata_integration.py
            |---- system/
                |---- __init__.py
                |---- testweatherstatistics_system.py
            |---- unit/
                |---- __init__.py
                |---- testweatherstatistics_unit.py
            |---- __init__.py
        |---- __init__.py
```

We can use the test module names to filter out the kinds of tests we want to run in each test runner module, like this:

```python
#!/usr/bin/env python3

import unittest

if __name__ == '__main__':
    testsuite = unittest.TestLoader().discover('.', "test*_unit.py")
    unittest.TextTestRunner(verbosity=1).run(testsuite)
```

Your unit tests will check most of the code – about 90% of it – in detail, and you'll run them many times in an hour.

Your integration tests will check only the code that talks directly to external programs – maybe about 10% of the code – and you might run them a few times every hour.

Your system tests will check key user features – maybe a few dozen for the whole program – and you might only run them as a final check before committing your code to version control (see chapter "Grown-up Programmers Can Go Back To Any Version Of Their Code"), just to be sure the program works as a whole.

# If Your Test Code Is Hard To Change, Your Programs will Be Hard To Change

[Add motivating example of why bad tests make code hard to change]

We must take good care to write tests that are:

As simple as we can make them
Easy to read and to understand
Free of duplication (unless that makes our test code harder to understand)
When writing tests, pay special attention to the names you choose for your tests. A good test name tells us what the test is. It's a bit like writing newspaper headlines: we want to give readers a clear sense of what the test is about.

```python
    def test_weather_stats_1(self):

        data = FakeTemperatureData()
        self.assertEqual(19.0, WeatherStatistics(data).average_daily_temperature())
```

In this example, the test name test_weather_stats_1 doesn't tell us very much. It's not very helpful for, for example, figuring out what went wrong if this test failed.

```python
    def test_average_daily_temperature_is_sum_divided_by_count(self):
        data = FakeTemperatureData()
        self.assertEqual(19.0, WeatherStatistics(data).average_daily_temperature())
```

A better name, like `test_average_daily_temperature_is_sum_divided_by_count`, would describe what the test is about. When this test fails, we will get a clear message about what was supposed to happen.

We should also take care to choose descriptive names for test data variables. Take a look at this test for a bank transfer between two accounts:

```python
class TestBankTransfer(unittest.TestCase):
    def test_transfer_1(self):
        a = BankAccount()
        b = BankAccount()
        a.credit(150.0)
        x = 50.0
        a.transfer(b, x)
        self.assertEqual(100.0, a.get_balance())
        self.assertEqual(50.0, b.get_balance())
```

We could make it more obvious what's going on here by choosing a better test name, and better names for our variables:

```python
class TestBankTransfer(unittest.TestCase):
    def test_transfer_credits_payee_and_debits_payer(self):
        payer = BankAccount()
        payee = BankAccount()
        payer.credit(150.0)
        transferAmount = 50.0
        payer.transfer(payee, transferAmount)
        self.assertEqual(100.0, payer.get_balance())
        self.assertEqual(50.0, payee.get_balance())
```

By choosing names that mean something in the context of our test (like "payer" and "payee"), we can make our test code easier to understand.

Finally, when we copy and paste tests to make new tests – maybe changing some of the data each time – we can end up having to rewrite many tests to accommodate a single change to our program. Consider the TestDiscountCalculator test class from before:

```python
class DiscountCalculatorTests(unittest.TestCase):
    def test_no_discount_on_single_tshirt(self):
        tshirts = ["White:S"]
        order = TshirtOrder(tshirts)
        self.assertEqual(0, DiscountCalculator().calculate_discount(order))

    def test_ten_percent_discount_on_two_tshirts(self):
        tshirts = ["White:S", "White:S"]
        order = TshirtOrder(tshirts)
        self.assertEqual(2, DiscountCalculator().calculate_discount(order))

    def test_twenty_percent_discount_on_two_tshirts(self):
        tshirts = ["White:S", "White:S", "White:S", "White:S"]
        order = TshirtOrder(tshirts)
        self.assertEqual(8, DiscountCalculator().calculate_discount(order))

    def test_thirty_percent_discount_on_six_tshirts(self):
        tshirts = ["White:S", "White:S", "White:S", "White:S", "White:S", "White:S"]
        order = TshirtOrder(tshirts)
        self.assertEqual(18, DiscountCalculator().calculate_discount(order))
```

All these tests are essentially the same, just with different test data and expected results. If we had to change how we create a `DiscountCalculator` object (e.g., add a constructor method), or how we tell it to calculate a discount (e.g., add a parameter to that method), then we'll have to make that change in every single test. This kind of duplication of code multiplies the effort of changing our program later.

A simple way to remove this duplication would be to have a single test method that we can give the data and expected results that vary from test to test as parameters. Grown-up programmers call this a parameterised test.

Unfortunately, unittest doesn't allow us to easily write parameterised tests. But there are Python packages available, like nose with nose-parameterized (with a "z", because that's how Americans spell it) that make it much easier. Here's TestDiscountCalculator re-written using them:

```python
#!/usr/bin/env python3

import unittest

from nose.tools import assert_equal
from nose_parameterized import parameterized

from ..src.tshirtorder import TshirtOrder
from ..src.discountcalculator import DiscountCalculator

class DiscountCalculatorTests(unittest.TestCase):

    @parameterized.expand([
       (["White:S"], 0),
       (["White:S", "White:S"], 2),
       (["White:S", "White:S", "White:S", "White:S"], 8),
       (["White:S", "White:S", "White:S", "White:S", "White:S", "White:S"], 18)

    ])
    def test_discounts_calculated_correctly_for_number_of_tshirts(self, tshirts, discount):
        order = TshirtOrder(tshirts)
        self.assertEqual(discount, DiscountCalculator().calculate_discount(order))
```

...which we can run simply from our tests.py module using nose:


```python
#!/usr/bin/env python3

import nose

if __name__ == '__main__':
    nose.main()
```
