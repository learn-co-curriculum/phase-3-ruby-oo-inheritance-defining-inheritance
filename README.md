# Code Along: An Intro to Inheritance 

## Objectives

1. Learn about inheritance in object oriented Ruby.
2. Write classes that inherit from another class. 

## Introduction

We had a brief introduction to inheritance in the previous unit when we build and utilized our very own custom errors. In that instance, our custom error inherited from the `StandardError` class, taking on all of the attributes and behaviors of the `StandardError` class. In this exercise, we'll be taking a closer look at class inheritance. 

## What is Inheritance?

In Ruby, classes can inherit from one another. This means that they adopt all of the attributes and behaviors (i.e. all of the methods) of the parent, also called the **superh** class. In this exercise, we'll be building our own chain of inheritance. 

## Code Along: Basic Inheritance

***This is a code along exercise. Fork and clone this lab by clicking on the github link at the top of the page. There are no tests to pass, just follow along with the guide and get your code working***. 

In this domain model, we have class `Vehicle` that will act at the parent, or super, class. We will create child classes, also known as **subclasses** for different types of `Vehicle`s, such as car. 

### Step 1: Defining the Super Class

Open up `lib/super_vehicle.rb`. We're going to define some methods in this parent class so that our subclasses, when we make them, will have access to them. 

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

Instances of `Vehicle` initialize with a wheel size and number. We also have `go` and `fill_up_tank` instance methods that describe some common vehicle behavior. 

### Step 2: Defining the Subclass

Open up `lib/sub_car.rb` and define the class in the following way: 

```ruby
class Car < Vehicle

end
```

We use the `<` to inherit the `Car` class from `Vehicle`. Notice that there are *no methods defined in the `Car` class*. 

Open up `bin/car` and look at the following code: 

```ruby
honda = Car.new
puts honda.go
puts honda.fill_up_tank 
```

Run the code with the following line in your terminal: `ruby bin/car`. You should see the following output: 

```bash
"vrrrrrrrooom!"
"filling up!"
```

Wow! We didn't write *anything* in our `Car` class but instances of `Car` class *inherit* all of the `Vehicle` methods and therefore have access to them. What happens when the newest model Honda comes out? You know, that model that goes *super* fast. When *that* car uses the `go` method, it should say `"VRRROOOOOOOOOOOOOOOOOOOOOOOM!!!!!"` Let's overwrite the inherited `go` method with one specific to `Car`. 

### Step 3: Overwriting Inherited Methods

In `lib/sub_car.rb`, write the following method:

```ruby
class Car
  def go
    "VRRROOOOOOOOOOOOOOOOOOOOOOOM!!!!!"
  end
end
```

Now, run the `bin/car` file again and you should see the following output: 

```bash
VRRROOOOOOOOOOOOOOOOOOOOOOOM!!!!!
filling up!
```

#### Method Look-Up in Ruby

How does our above example work? Well, when your program is being executed, at the point at which the `.go` method is invoked, the compiler will first look in the class to which the instance of car that we are calling the method on belongs. If it finds a `.go` method there, it will execute *that method*. If it doesn't find such a method there, it will move on to look in the parent class that this class inherits from. 

### Step 4: The `super` Keyword

What if we don't want to totally overwrite the `go` method on the `Vehicle` class, but just augment it a little? That's where the `super` keyword comes in. Here's how it works: 


```ruby
class Car

  def go
    super + "VRRROOOOOOOOOOOOOOOOOOOOOOOM!!!!!"
  end
end
``` 

Make the above change to your `Car` class and run the `bin/car` file. You should see the following output in your terminal: 

```bash
vrrrrrrrooom!VRRROOOOOOOOOOOOOOOOOOOOOOOM!!!!!
filling up!
```

The use of the `super` keyword delegate the method look-up to the super class, while augmenting or adding to that method's behavior. 

## Why Use Inheritance?

The use of inheritance allows us to create a family of classes with shared behavior. While you may not write your own classes that use inheritance very frequently, you will encounter it frequently as a Ruby on Rails web developer. Once we introduce the use of databases and the challenge of connecting our programs do our database, you'll encounter inheritance in nearly every program you write for the web. More on that (much) later. 

