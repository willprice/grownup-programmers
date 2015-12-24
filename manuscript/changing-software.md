For many years, we've called it "software" because it's supposedly easy to change. The design of a house, once it's built, is literally set in stone (or concrete, or steel, or timber). Computer code isn't like that: it's not poured like concrete or welded like steel or nailed down like wood. It's much easier to change a line of code than it is to move the foundations of a house.

But it's not quite as easy as people think, and the larger the program gets, the harder it can be to change. So much so that many programs have to be abandoned and re-written all over again, because that's easier than making the changes users want. (And they will want them.)

Grown-up programmers design their programs to make them easier to change and to reduce the risk of breaking them when they do.

# Keep It Working

When we make a change to our program code – no matter how small – it's vitally important to check as soon as possible that it still works.

We do this by testing the program to look for any new bugs we might have introduced when we made the change. One way to test our program would be to run it and "take it for a spin", but this takes time. A large program may have dozens of features and may require us to perform hundreds or even thousands of different combinations of inputs and actions to properly test it. It could take us hours, days or even weeks to test it well enough to be confident it still works.

This is why Grown-up Programmers Write Programs To Test Their Programs. What might take me and you a long time to do, a computer can do in minutes or seconds. By writing fast-running unit tests that check most of our code quickly, we can test our program over and over again, as often as we need. This can give us much higher confidence that it still works after we've changed it.

# Write Code That Other Programmers Can Understand

After making sure our code still works, the next most important thing to think about is how easy our code is to understand. Grown-up programmers spend a lot of time reading code so they can make changes to it. If we can make our code easier to understand, we'll be saving a lot of time later.

Take a look at this Python class:

```python
class WUtils:
    def temp(self, t, p):
        a = sum(t) / len(t)
        return (a + p) / 2
```

What is it doing? It's hard to tell, because we've written it in a way that doesn't make it obvious. In code, we can use the names of things – classes, methods/functions, variables, constants and so on – to tell the story of what the code does and make it easier for other programmers (and ourselves when we come back to it) to understand.

Let's start with the name of the class, `WUtils`. What does this class actually do? Maybe if we renamed it `WeatherPrediction`, that would make things clearer?

```python
class WeatherPrediction:
    def temp(self, t, p):
        a = sum(t) / len(t)
        return (a + p) / 2
```

But it's still not clear what this code does. What does the method actually do? It predicts today's temperature using a list of temperatures on the same date in previous years, as well as yesterday's temperature. Let's rename it so that's clear.

```python
class WeatherPrediction:
    def todays_temperature(self, t, p):
        a = sum(t) / len(t)
        return (a + p) / 2
```

Now let's rename the parameters so it's obvious what they represent.

``python
class WeatherPrediction:
    def todays_temperature(self, previous_years, yesterday):
        a = sum(previous_years) / len(previous_years)
        return (a + yesterday) / 2
```

The code is easier to understand now, but is it obvious that what we're doing is predicting today's temperature by calculating the average of the temperature in previous years and yesterday's? The code `sum(previous_years) / len(previous_years)` calculates the historical average: let's make that more obvious by putting it in its own function, and using the function's name to tell the story of what it does.

```
class WeatherPrediction:
    def todays_temperature(self, previous_years, yesterday):
        a = average(previous_years)
        return (a + yesterday) / 2

def average(numbers):
    return sum(numbers) / len(numbers)
```

Lastly, let's reuse `average()` to make it obvious that we are predicting today's temperature to be the average of previous years' temperatures and yesterday's.

```python
class WeatherPrediction:
    def todays_temperature(self, previous_years, yesterday):
        return average([average(previous_years), yesterday])

def average(numbers):
    return sum(numbers) / len(numbers)
```

The code we have now does exactly the same thing as the original, which we can check using our unit tests. But now it also tells the story of what it's doing, so other programmers can more easily understand it.

If we want to write code that's easier to understand, here are some things we need to think about:

## Classes & Modules

Do their names describe what they're for?

A class name should make it clear what its job is. For example, if we write a class that calculates how much rocket fuel a spaceship would need to reach Mars, the name `FuelCalculator` would tell us more than, say, just `Calc`. Don't be afraid to use whole words, and don't worry about making it too obvious!

## Methods & Functions

Do their names describe what they do?

If we have a method on a class called `Email` that sends an email, then `email.send()` makes more sense than something like `email.do()` or `email.execute()`.

## Parameters, Variables & Fields

Do their names describe what they represent?

If we have a parameter of `send()` that let's us queue the email up to send later instead of sending it straight away, choose a name for that parameter that makes it clear what it means (e.g., `email.send(queue=True)` ). The same is true for variables and fields. name them so it's obvious what that piece of data represents in our program. Again, don't be afraid to use whole words; `email.send(q=True)` isn't as clear.

## Special data values

Are numbers, strings and other specially important data values clear?

Take a look at this code that records book loans to members of a library:

```python
class Book:
    def __init__(self):
        self.on_loan_to = None

    def borrow(self, libraryMember):
        if(len(libraryMember.get_borrowed_books()) == 5):
            raise Exception("Loan limit reached")

        self.on_loan_to = libraryMember;
        libraryMember.loan(self)
```

Library members can borrow a maximum of 5 books. If they already have 5 books on loan and try to borrow another one, the program will raise an exception. That number 5 means something to our program; it's the maximum number of book loans. We could make it more obvious by creating a constant that tells us what the value 5 means here.

```python
MAX_LOANS = 5

class Book:
    def __init__(self):
        self.on_loan_to = None

    def borrow(self, libraryMember):
        if(len(libraryMember.get_borrowed_books()) == MAX_LOANS):
            raise Exception("Loan limit reached")

        self.on_loan_to = libraryMember;
        libraryMember.loan(self)
```

## Comments

Do we rely too much on comments to explain our code?

Many programming teachers and books recommend writing lots of comments to help other programmers understand our code. Grown-up programmers, though, know that comments are often not helpful, and are a sign that the code itself needs to be made clearer. Comments can get out of step with the code as it changes, and become misleading. Lots of comments can clutter up our code, actually making it harder to read. For this reason, we recommend trying not to rely on comments. When you see comments – or code you think needs commenting – look for ways to include that information in the code itself, using class and module names, method and function names, parameter, variable and field names, and constants.

```python
class Book:
    ...

    def borrow(self, libraryMember):
        # check member can borrow more books
        if(len(libraryMember.get_borrowed_books()) == MAX_LOANS):
            raise Exception("Loan limit reached")

        # record the loan
        self.on_loan_to = libraryMember;
        libraryMember.loan(self)
```

Here, we've used comments to describe what's going on in our `borrow()` method. Let's take the information in the comments and put it in the code, so the code describes itself.

```python
class Book:
    ...
    def borrow(self, libraryMember):
        if(not libraryMember.can_borrow_more()):
            raise Exception("Loan limit reached")

        self.record_loan(libraryMember)

    def record_loan(self, libraryMember):
        self.on_loan_to = libraryMember;
        libraryMember.loan(self)


class LibraryMember:
    def __init__(self):
        self.borrowed_books = []

    def can_borrow_more(self):
        return not (len(self.get_borrowed_books()) == MAX_LOANS)
```

All we've done is take the code that needed explaining, and put it in methods with names that explain what that code does. No more need for those pesky comments!

Are there any special cases where we might still need to use comments? Sometimes – rarely – we need to explain not what the code does, but why it does it.

```python
cats = ["Mr Dinkles", "Buffy", "Tibbles", "Suki"]
cats.sort()     # binary search works on sorted lists
index_of_Mr_Dinkles = binary_search(cats, "Mr Dinkles")
```

In this silly example, we want to search for the index of a cat's name in a list. Because we've chosen to use a binary search algorithm for speed on bigger lists (Google "binary search" for more information) , we need to sort the list first. The comment explains why we're sorting the list before we call `binary_search()`.

# Keep It Simple

Code that's simple is often easier to understand. Not always, but often. And code that's simple is less likely to go wrong, because there are less things that might break. For both these reasons, simpler code tends to be easier to change.

You may think that simpler code is easier to write, but that's not always true. Programmers of all ages have a habit of writing the first solution to a problem that pops into their head, and that isn't always the simplest solution.

Take this function that tells us if a list contains any even numbers:

```python
def contains_even_numbers(numbers):
    even_numbers = [n for n in numbers if n % 2 == 0]
    if len(even_numbers) > 0:
        return True
    else:
        return False
```

It could be simplified to:

```python
def contains_even_numbers(numbers):
    return any(n % 2 == 0 for n in numbers)
```

Likewise, something like this:

```
def monthly_sales_total(monthly_sales):
    monthly_total = 0

    for daily_sales in monthly_sales:
        daily_total = 0

        for item_sale in daily_sales:
            daily_total += item_sale

        monthly_total += daily_total

    return monthly_total
```

...could be simplified to:

```python
def monthly_sales_total(monthly_sales):
    return sum(map(lambda daily_sales: sum(daily_sales), monthly_sales))
```

But some Python programmers might find this second simplified version a little harder to understand. Yes, it's less code (and less code is usually a good thing), but arguably there's more to understand.

Writing code that's easy to understand is more important than writing code that's simpler. If you're in any doubt, show both versions – the original and the simplified one – to another programmer and get their input on which makes more sense to them.

# You Ain't Gonna Need it

Imagine you're going on a trip and you need to pack for it. Some people get the biggest suitcase they can find and cram as much as possible into it: clothes for every occasion, bathroom stuff to last a month, medicines, gadgets (and their chargers), books, maps, First Aid kits, towels, toilet paper, even food and drink. Now matter how long the trip: they pack everything think they might need.

Then they have to drag that massive suitcase around with them. No wonder some people come back from holiday more exhausted than when they left!

And, of course, they never need it all.

Smarter people pack only what they know they'll need. You can usually spot them: they whiz through airports looking calm and un-frazzled.

A common mistake programmers make is to add things to their programs that aren't really needed, thinking it might be needed later.

For example, a programmer creating a web application might be in the habit of adding a MySQL database right at the start. Because web applications have databases, right? Well, not necessarily. A simple file might do the job. And some web applications don't really need to store data at all.

By adding the MySQL database before we know we need it, we've made our program more complicated than it needs to be.

So before you add anything to your program, stop and ask yourself: do I know I need this? Is this the simplest solution to the problem? If not, then leave it out until a need arises.

# Use Pieces That Do One Thing

A café serves:

- Steak & Kidney Pudding with Green Beans & New Potatoes
- Gammon with Fried Egg, Chips & Beans
- Lasagne with a Green Salad
- Cheese on Toast

What happens if the customer wants steak & kidney pudding with chips? Or green beans with their gammon? Or a fried egg on toast? We can give diners more choices by breaking down the menu into individual items:

- Steak & Kidney Pudding
- Green Beans
- New Potatoes
- Gammon
- Fried Egg
- Chips
- Baked Beans
- Lasagne
- Green Salad
- Cheese (melted)
- Toast

We can make many more dishes with these individual options than the four on offer on the original menu – many more combinations are possible.

The same is true in our programs; if we write a function that:

Calculates a video renter's age AND checks they are old enough to rent that video AND records the rental

```python
def rent(self, renter):
    age_years = relativedelta(datetime.now(), renter.get_date_of_birth()).years

    if(age_years < self.min_age):
        raise RenterTooYoungException()

    self.on_loan_to = renter
    renter.add_rented_video(self)
```

...we are left with a single function that can only do those three things, always together, in exactly that order.

If we split that into three functions, each of which has one specific job:

- Calculate video renter's age
- Check they're old enough to rent that video
- Record their video rental

```python
def rent(self, renter):
    age_years = self.calculate_age_years(renter)
    self.check_can_rent(age_years)
    self.record_rental(renter)

def record_rental(self, renter):
    self.on_loan_to = renter
    renter.add_rented_video(self)

def check_can_rent(self, age_years):
    if (age_years < self.min_age):
        raise RenterTooYoungException()

def calculate_age_years(self, renter):
    age_years = relativedelta(datetime.now(), renter.get_date_of_birth()).years
    return age_years
```

...then we have many more choices as to how we can combine these functions to do different things. Perhaps we just want to know the customer's age, for example, so we can make appropriate recommendations for videos they might enjoy.

Notice that `rent()` now no longer does any of the work. Instead, it tells the story of what work is done, using the new method names to describe each step. This helps us to make our code easier to understand.

Apply a similar idea to the design of your classes: make it so that each class has one specific job.

For example, this `Renter` class records details of videos rented by a person, but it also prints a report of those rentals.

```python
class Renter:
    def __init__(self, date_of_birth, name):
        self.name = name
        self.date_of_birth = date_of_birth
        self.rented_videos = []

    def get_rented_videos(self):
        return self.rented_videos

    def get_date_of_birth(self):
        return self.date_of_birth

    def get_name(self):
        return self.name

    def add_rented_video(self, video):
        self.rented_videos += [video]

    def print_rentals(self):
        print("Renter Name: " + self.get_name())
        print("Renter D.O.B.: " + str(self.get_date_of_birth()))
        print("Rented Videos...")
        for video in self.rented_videos:
            print(video.get_title())
```

What if we want to generate different styles of report for rentals? If we split the work up into two classes – each with one specific job – then we can combine different styles of report with the same rental information.

```python
class RentalsReport:
    def print_rentals(self, renter):
        print("Renter Name: " + renter.get_name())
        print("Renter D.O.B.: " + str(renter.get_date_of_birth()))
        print("Rented Videos...")
        for video in renter.get_rented_videos():
            print(video.get_title())
```

# Don't Repeat Yourself

Take a look at pretty much anyone's code, and you'll see bits of it that might look very similar.

```python
def calculate_weather_statistics(temperature_readings, rainfall_readings, cloud_cover_readings):
    statistics = {}

    average_temperature = sum(temperature_readings) / len(temperature_readings)
    max_temperature = max(temperature_readings)
    min_temperature = min(temperature_readings)
    statistics["temperature"] = WeatherStatistic(average_temperature, max_temperature, min_temperature)

    average_rainfall = sum(rainfall_readings) / len(rainfall_readings)
    max_rainfall = max(rainfall_readings)
    min_rainfall = min(rainfall_readings)
    statistics["rainfall"] = WeatherStatistic(average_rainfall, max_rainfall, min_rainfall)

    average_cloud_cover = sum(cloud_cover_readings) / len(cloud_cover_readings)
    max_cloud_cover = max(cloud_cover_readings)
    min_cloud_cover = min(cloud_cover_readings)
    statistics["cloud cover"] = WeatherStatistic(average_cloud_cover, max_cloud_cover, min_cloud_cover)

    return statistics
```

In this example, there are three blocks of code that are almost the same. If we wanted to change how a `WeatherStatistic` is calculated (e.g., adding an extra statistic like standard deviation), we would have to change the code in three places.

Repeating code multiplies the work we might have to do to change it. This is why grown-up programmers try not to repeat code (except when it makes the code easier to understand.)

```python
def calculate_weather_statistics(temperature_readings, rainfall_readings, cloud_cover_readings):
    statistics = {}
    statistics["temperature"] = calculate_statistic(temperature_readings)
    statistics["rainfall"] = calculate_statistic(rainfall_readings)
    statistics["cloud cover"] = calculate_statistic(cloud_cover_readings)
    return statistics

def calculate_statistic(readings):
    average = sum(readings) / len(readings)
    maximum = max(readings)
    minimum = min(readings)
    return WeatherStatistic(average, maximum, minimum)
```

Now that we've removed this repeated code, see how easy it is to add a new kind of statistic, because we only have to do it in one place:

```python
def calculate_statistic(readings):
    average = sum(readings) / len(readings)
    maximum = max(readings)
    minimum = min(readings)
    standard_deviation = stdev(readings)  #added
    return WeatherStatistic(average, maximum, minimum, standard_deviation)
```

And, because it's now just a call to a function, see how much easier it is to add a new set of weather readings:

```python
def calculate_weather_statistics(temperature_readings, rainfall_readings, cloud_cover_readings, humidity_readings):
    statistics = {}
    statistics["temperature"] = calculate_statistic(temperature_readings)
    statistics["rainfall"] = calculate_statistic(rainfall_readings)
    statistics["cloud cover"] = calculate_statistic(cloud_cover_readings)
    statistics["humidity"] = calculate_statistic(humidity_readings)      #added
    return statistics
```

# Put The Work Where The Knowledge Is

Our code is full of dependencies – functions that call functions, classes that use classes, methods that use fields, and so on. In all these cases, making even a tiny change to one bit of code can cause other parts of the program to break.

This can mean we have to do a lot more work to make even the smallest change, because of all the things that depend on the code we're changing.

The more the different parts of our code depend on each other, the more code will break when we change it. So, to make code easier to change, grown-up programmers make sure that these different parts of their programs depend as little as possible on each other.

```python
class Employee:
    def __init__(self, passport):
        self.passport = passport

    def is_eligible_to_work(self):
        age = relativedelta(datetime.now(), self.passport.get_date_of_birth()).years
        return self.passport.get_country_of_residence() == "United Kingdom" and age >= 18

class Passport:

    def __init__(self, country, dob):
        self.country_of_residence = country
        self.date_of_birth = dob

    def get_country_of_residence(self):
        return self.country_of_residence

    def get_date_of_birth(self):
        return self.date_of_birth
```

In this example, `Employee` depends on methods of `Passport` – `get_country_of_residence()` and `get_date_of_birth()` – to determine if an employee is eligible to work. The knowledge that `Employee` needs to do this work is actually held by Passport.

We could reduce the dependencies on `Passport` by putting the work where the knowledge is. Then, instead of having to call two different methods on `Passport` to get that knowledge, it can leave the work to `Passport` and only call a single method that does it all.

```python
class Employee:
    def __init__(self, passport):
        self.passport = passport

    def is_eligible_to_work(self):
        return self.passport.is_eligible_to_work()


class Passport:
    def __init__(self, country, dob):
        self.country_of_residence = country
        self.date_of_birth = dob

    def is_eligible_to_work(self):
        age = relativedelta(datetime.now(), self.date_of_birth).years
        return self.country_of_residence == "United Kingdom" and age >= 18
```

All we've done here is move `is_eligible_to_work()` from `Employee` to `Passport`, were it belongs. Notice how the methods for accessing the information on `Passport`, `get_country_of_residence()` and `get_date_of_birth()` have gone. Now that `Passport` is doing the work, we no longer need to share the information with `Employee`.

Generally, the less classes and methods, and modules and functions, know about each other, the better.

# Plug Objects Together From The Outside

Object Oriented Programming (OOP) is about dividing up the work our program needs to do to complete a task for the user, giving specific jobs to objects that have the knowledge to do them. Objects need to work together to complete a task, calling each other's methods when they need something done.

It's a little like the stomp boxes that guitar players use to change the sound of their guitar. Each stomp box does a specific job – like boosting the signal to make it louder, distorting it for a rock sound, adding a chorus effect or echoes, and so on. Guitar players plug their stomp boxes together in a chain to allow them to combine these different effects to get the overall sound they want.

![Pedal board](./media/images/pedalboard.jpg)

To vary the sound, players can plug their stomp boxes together in different orders. They can only do this because guitar stomp boxes all present a standard way of plugging them together – via a 1/4-inch cable that connects the output of one stomp box to the input of the next.

This pluggability is the key to a flexible stomp box set-up and getting a wide range of different kinds of sounds. As we'll see, if we design our code for pluggability, we can give ourselves greater flexibility by allowing different combinations of objects, too.

For example, this `Dvd` class works with `AmazonPricer` to get a price for a copy of that DVD for ordering through our application.

```python
class Dvd:
    def __init__(self, title):
        self.title = title

    def order(self, number_of_copies):
        pricer = AmazonPricer()
        price_per_copy = pricer.get_price(self.title)
        return Order(price_per_copy, number_of_copies)
```

So far, so good. But what happens if we stop buying our DVDs from Amazon? What happens if we'd like to give users a choice of DVD supplier (Amazon, Play.com, Zavvi, HMV etc?). The code as it is doesn't allow for this, because `Dvd` creates the `AmazonPricer` object itself. There's no easy way to swap an `AmazonPricer` with some other kind of DVD pricer. `AmazonPricer` is hardwired into `Dvd`.

Let's redesign the code so that we can swap pricers:

```python
class Dvd:
    def __init__(self, title, pricer):
        self.title = title
        self.pricer = pricer

    def order(self, number_of_copies):
        price_per_copy = self.pricer.get_price(self.title)
        return Order(price_per_copy, number_of_copies)
```

Notice that we create the pricer outside and plug it into `Dvd` through the constructor. So the code that creates a `Dvd` decides to use an `AmazonPricer`.

```python
dvd = Dvd("How To Train Your Dragon", AmazonPricer())
order = dvd.order(1)
```

We can give ourselves more flexibility when plugging objects together like this. `Dvd` expects to work with pricers that have this `get_price(title)` method. We can create different kinds of pricers that all have the method `get_price(title)`.

```python
class AmazonPricer:
    def get_price(self, dvd_title):
        # code to get price via Amazon API goes here
        ...

class HmvPricer:
    def get_price(self, dvd_title):
        # code to get price from HMV API goes here
        ...
```

So now we can create `Dvd` objects using two different kinds of Pricer, and `Dvd` only needs to know that it's talking to some kind of pricer that has the `get_price(title)` method.

```python
dvd = Dvd("How To Train Your Dragon", AmazonPricer())
order = dvd.order(1)

dvd = Dvd("How To Train Your Dragon", HmvPricer())
order = dvd.order(1)
```

By passing objects in from the outside, through the constructor or other methods – e.g., we could have passed in the Pricer as a parameter of `order()` – we get pluggability. It will be much easier now to add a different kind of pricer to our program, because we don't have to change any of the code in `Dvd`.
