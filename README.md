# Code Along: An Intro to Inheritance 

## Objectives

1. Learn about inheritance in object oriented Ruby.
2. Write classes that inherit from another class. 

## Introduction: Why Inheritance?

In the real-world, different entities (people, animals, cars, you name it) are related in various ways. Within a single entity or group, there exist systems of classification. For example, the "dogs" entity or category includes pugs, corgis, labs, etc. All of these breeds share common features because they are all dogs. But they all have certain unique traits as well. 

Another example: you are writing a web application in which users are either admins, instructors or students. All of these entities are "users" and have common features, but they all have some unique traits as well. 

How can our code reflect that fact that these different categories of things all share some, or even many, characteristics but all of some unique attributes as well? Well, we could write separate admin, instructor and student class that each contain repitious code to lend each of these classes shared attributes and behaviors. We know, however, that repitious code always something to be avoided. Not only is it time consuming but, what happens when we need to make a change to this shared behavior? We'd have to code the same change in three places.  

Instead, we can use **inheritance**. The use of inheritance allows us to create a family of classes with shared behavior, while still differentiating those classes. With inheritance, we could *inherit* the admin, instructor and student classes from a user class. Then, any changes made to the user class would apply to the other class. 

While you may not write your own classes that use inheritance very frequently, you will encounter it frequently as a Ruby on Rails web developer. Once we introduce the use of databases and the challenge of connecting our programs do our database, you'll encounter inheritance in nearly every program you write for the web. More on that (much) later. 

## What is Inheritance?

In Ruby, classes can inherit from one another. This means that they adopt all of the attributes and behaviors (i.e. all of the methods) of the parent, also called the **super** class. In this exercise, we'll be building our own chain of inheritance. 

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