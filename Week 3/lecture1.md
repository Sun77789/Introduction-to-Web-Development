#Week 3 Lecture 1#

##Review: Arrays and Hashes##

Spend 5 minutes on this [quiz](http://www.codequizzes.com/learn-ruby/symbols-array-methods-hashes) from CodeQuizzes to review the data structures that we learned last week.

#General Overview#
Today we are going to take our first step in understanding Object-Oriented Programming. This material is critical not only for understanding Ruby but also understanding how to build web applications that follow a MVC (Model-View-Controller) framework.

##OOP##
An object-oriented program involves classes and objects. A class is the blueprint from which individual objects are created. In object-oriented terms, we say that your bicycle is an instance of the class of objects known as bicycles.

Take the example of any vehicle. It comprises wheels, horsepower, and fuel or gas tank capacity. These characteristics form the data members of the class Vehicle. You can differentiate one vehicle from the other with the help of these characteristics.

A vehicle can also have certain functions, such as halting, driving, and speeding. Even these functions form the data members of the class Vehicle. You can, therefore, define a class as a combination of characteristics and functions.

```
Class Vehicle
{
   Number no_of_wheels
   Number horsepower
   Characters type_of_tank
   Number Capacity
   Function speeding
   {
   }
   Function driving
   {
   }
   Function halting
   {
   }
}
```

So, if we use the parts of speech to understand how characteristics and functions are related,

- **object: noun**
- **method (function): verb**
- **attribute: adjective**

##Defining a Class in Ruby##
To implement object-oriented programming by using Ruby, you need to first learn how to create objects and classes in Ruby.

A class in Ruby always starts with the keyword class followed by the name of the class. The name should always be in initial capitals. The class Customer can be displayed as:

```
class Customer
end
```
You terminate a class by using the keyword end.

##Variables in a Ruby Class##
Ruby provides four types of variables. Each one depends on the scope or where in the program that exact variable is accessible.

* **Local Variables**: Local variables are the variables that are defined in a method. Local variables are not available outside the method. You will see more details about method in subsequent chapter. Local variables begin with a lowercase letter or _.

Ex.
In this example, `color` is a local variable in the method `print_color`
```ruby
def print_color
  color = "red"
  puts color
end
```

* **Instance Variables**: Instance variables are available across methods for any particular instance or object. That means that instance variables change from object to object. Instance variables are preceded by the at sign (@) followed by the variable name.

* **Class Variables**: Class variables are available across different objects. A class variable belongs to the class and is a characteristic of a class. They are preceded by the sign @@ and are followed by the variable name.

* **Global Variables**: Class variables are not available across classes. If you want to have a single variable, which is available across classes, you need to define a global variable. The global variables are always preceded by the dollar sign ($).

##Creating an Object##
Objects are instances of the class. You will now learn how to create objects of a class in Ruby. You can create objects in Ruby by using the method new of the class.

The method new is a unique type of method, which is predefined in the Ruby library. The new method belongs to the class methods.

Here is the example to create two objects cust1 and cust2 of the class Customer:

```ruby
cust1 = Customer.new
cust2 = Customer.new
```
Here, cust1 and cust2 are the names of two objects. You write the object name followed by the equal to sign (=) after which the class name will follow. Then, the dot operator and the keyword new will follow.

##Custom Method to create Ruby Objects with data##
You can pass parameters to the method new and those parameters can be used to initialize instance variables.

When you plan to declare the new method with parameters, you need to declare the method initialize inside the class.

The initialize method is a special type of method, which will be executed when the new method of the class is called with parameters.

Here is the example to create initialize method:
```ruby
class Customer
   def initialize(id, name)
      @id = id
      @name = name
   end
end
```

In this example, you declare the initialize method with id and name as local variables. Here, def and end are used to define a Ruby method initialize.

In the initialize method, you pass on the values of these local variables to the instance variables @id and @name. Here local variables hold the values that are passed along with the new method.

Now, you can create objects as follows:

```ruby
cust1 = Customer.new(1, "John")
cust2 = Customer.new(2, "Paul")
```

##Now you try##
>Define a class called Person.

>Write an initialize method that can accept 3 parameters: name, age, and gender.

To test this in IRB, run `irb` where your file is located. If your file is named `customer.rb`, then you can type
```ruby
>> load 'customer.rb'
=> true
>> c1 = Customer.new(1, "Shaan")
=> #<Customer:0x007fd8a123c7c0>
```

If you make changes to the customer.rb file, make sure to reload the file in the IRB.

##Instance Methods##
You'll notice that we have no methods defined for the Customer class to access the data. We will define a method to access the instance variable @name. This type of method is called a **getter** method.

```ruby
class Customer
  def initialize(id, name)
    @id = id
    @name = name
  end

  def name
    @name
  end
end
```

Now you can do
```ruby
my_customer = Customer.new(1, "John")
puts my_customer.name
```

If you want to be able to change (or set) the value of the instance variable @name, we need to define another method (setter method).

```ruby
class Customer
  def initialize(id, name)
    @id = id
    @name = name
  end

  def name
    @name
  end

  def name=(new_name)
    @name = new_name
  end
end
```

Now you can do
```ruby
my_customer = Customer.new(1, "John")
my_customer.name = "Sam"
puts my_customer.name
```

Now that you understand how to get and set the value of an instance variable, we can start to use Ruby shortcuts:

```ruby
class Customer
  attr_reader :name
  attr_writer :name

  def initialize(id, name)
    @id = id
    @name = name
  end
end
```

The attr_reader shortcut is the same as defining the getter method above. The attr_write shortcut is the same as defining the setting method above. We can take things one step further by using attr_accessor.

```ruby
class Customer
  attr_accessor :name

  def initialize(id, name)
    @id = id
    @name = name
  end
end
```

Now we can access the getter and setter method for the instance variable @name. If we want to make this shortcut work for both of the attributes, we can do this:

```ruby
class Customer
  attr_accessor :name, :id

  def initialize(id, name)
    @id = id
    @name = name
  end
end
```

Now we have four method defined for us using just one neat line: the getter and setter methods for @name and the getter and setter methods for @id.

If that isn't awesome, I don't know what is.
