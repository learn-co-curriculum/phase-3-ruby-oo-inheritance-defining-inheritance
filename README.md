# Defining Object Inheritance

## Learning Goals

- Learn about inheritance in object oriented Ruby.
- Write classes that inherit from another class.

## Introduction: Why Inheritance?

In the real-world, different entities (people, animals, cars, you name it) are
related in various ways. Within a single entity or group, there exist systems of
classification. For example, the "dogs" entity or category includes pugs,
corgis, labs, etc. All of these breeds share common features because they are
all dogs. But they all have certain unique traits as well.

Another example: you are writing a web application in which users are either
admins, instructors or students. All of these entities are "users" and have
common features, but they all have some unique traits as well.

How can our code reflect the fact that these different categories of things all
share some, or even many, characteristics but all have some unique attributes as
well? Well, we could write separate `Admin`, `Instructor` and `Student` classes
that each contain repetitious code to lend each of these classes shared
attributes and behaviors. We know, however, that repetitive code is always
something to be avoided. Not only is it time consuming, but what happens when we
need to make a change to this shared behavior? We'd have to code the same change
in three places.

Instead, we can use **inheritance**. The use of inheritance allows us to create
a family of classes with shared behavior, while still differentiating those
classes. With inheritance, we could _inherit_ the `Admin`, `Instructor` and
`Student` classes from a `User` class. Then, any changes made to the `User`
class would apply to the other class.

While you may not write your own classes that use inheritance very frequently,
you will encounter it frequently as a developer, particularly when working with other
libraries (such as Active Record, which you'll learn later in this phase). Once
we introduce the use of databases and the challenge of connecting our programs
to our database, you'll encounter inheritance in nearly every program you write
for the web. More on that later.

## What is Inheritance?

In Ruby, classes can inherit from one another. This means that they adopt all of
the attributes and behaviors (i.e. all of the methods) of the parent, also
called the **super** class. In this exercise, we'll be building our own chain of
inheritance.

## Code Along: Basic Inheritance

In this domain model, we have class `Vehicle` that will act as the parent, or
super, class. We will create child classes, also known as **subclasses** for
different types of `Vehicle`s, such as car.

### Step 1: Defining the Super Class

Open up `lib/vehicle.rb`. We're going to define some methods in this parent
class so that our subclasses, when we make them, will have access to them.

```ruby
class Vehicle

  attr_accessor :wheel_size, :wheel_number

  def initialize(wheel_size, wheel_number)
    @wheel_size = wheel_size
    @wheel_number = wheel_number
  end

  def go
    "vrrrrrrrooom!"
  end

  def fill_up_tank
    "filling up!"
  end

end
```

Instances of `Vehicle` initialize with a wheel size and number. We also have
`#go` and `#fill_up_tank` instance methods that describe some common vehicle
behavior.

Go ahead and paste the above Vehicle class code into your Vehicle class, and run
the test suite and you'll see that you are passing all of the tests for the
`Vehicle` class but none of the tests for the `Car` class.

### Step 2: Defining the Subclass

Open up `lib/car.rb`. Notice that we are requiring `lib/vehicle.rb`. That is
because our `Car` class will need access to the `Vehicle` class and will
therefore need access to the file that contains that class.

Go ahead and define the class in the following way:

```ruby
class Car < Vehicle

end
```

We use the `<` to inherit the `Car` class from `Vehicle`. Notice that there are
_no methods defined in the `Car` class_.

Run the test suite again and you'll see that you are passing a number of tests for the `Car` class.

Wow! We didn't write _anything_ in our `Car` class but instances of `Car` class
_inherit_ all of the `Vehicle` methods and therefore have access to them. We're
still failing the `#go` test however. Looks like the test is expecting the `#go`
method on an individual car to return the phrase:
`"VRRROOOOOOOOOOOOOOOOOOOOOOOM!!!!!"`. This is different than the return value
of the `#go` method that we inherited from the `Vehicle` class.

Let's overwrite the inherited `#go` method with one specific to `Car`.

### Step 3: Overwriting Inherited Methods

In `lib/car.rb`, write the following method:

```ruby
class Car < Vehicle
  def go
    "VRRROOOOOOOOOOOOOOOOOOOOOOOM!!!!!"
  end
end
```

Now, run the tests again and you should be passing all of them.

## Method Look-Up in Ruby

How does our above example work? Well, when your program is being executed, at
the point at which the `#go` method is invoked, the compiler will first look in
the class to which the instance of car that we are calling the method on
belongs. If it finds a `#go` method there, it will execute _that method_. If it
doesn't find such a method there, it will move on to look in the parent class
that this class inherits from.

You can see how Ruby classes inherit from one another by using Ruby to do some
_introspection_ on our classes.

Open up IRB, and start by requiring the files from the `lib` folder:

```rb
require_relative 'lib/vehicle'
# => true
require_relative 'lib/car'
# => true
```

This will let you interact with the code you've written in those files from
within IRB.

We can ask the `Car` class what its parent, or "superclass" is (what class the
`Car` class inherits from) with the `.superclass` method:

```rb
Car.superclass
# => Vehicle
```

We can even see what the parent class of `Vehicle` is, and up as far as we can
go on the inheritance chain to the very top (`BasicObject`):

```rb
Car.superclass.superclass
# => Object
Car.superclass.superclass.superclass
# => BasicObject
Car.superclass.superclass.superclass.superclass
# => nil
```

How does this work? How can we call the `.superclass` class method method on our
`Car` class, even though we didn't define it ourselves? The `.superclass` method
is available on all Ruby classes, even built-in ones like the `Integer` class:

```rb
Integer.superclass
# => Numeric
```

That's because all Ruby classes share the same class: the
[`Class` class](https://ruby-doc.org/core-2.7.3/Class.html)!

```rb
Car.class
# => Class
String.class
# => Class
```

## Conclusion

We've seen how to set up inheritance to share behavior from one class to another
using the `<` syntax in our class definition (`class Child < Parent`), which
lets the subclass use methods that are defined on the parent class. We also
discussed how **method lookup** works in Ruby, when multiple classes define the
same method.
