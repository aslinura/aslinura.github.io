<div id="container" style="position:relative;">
<div style="float:left"><h1> Advanced Python</h1></div>
<div style="position:relative; float:right"><img style="height:65px" src ="https://drive.google.com/uc?export=view&id=1EnB0x-fdqMp6I5iMoEBBEuxB_s7AmE2k" />
</div>
</div>


```python
# importing packages to be used later
import numpy as np
import pandas as pd
import sys
```

### Classes and Object-Oriented Programming (OOP)

We have seen Python's basic built-in data types, and you have probably noticed a few other types, like the linear regression object from statsmodels and the NumPy array. You've likely also noticed that all types aren't created equal. For example:


```python
my_list = [1, 2, 3, 4, 5]
print(my_list.pop())  # <-- Why does this line work...

my_string = 'hello world'
print(my_string.pop())  # <-- ... but not this one?
```

    5
    


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_14716/1533790374.py in <module>
          3 
          4 my_string = 'hello world'
    ----> 5 print(my_string.pop())  # <-- ... but not this one?
    

    AttributeError: 'str' object has no attribute 'pop'


Python is an object-oriented programming language. We have objects, which are instances of a particular class. These objects can contain data or attributes, and can interact with each other. We can compare this to something like R or Scala which are more functional, we have objects or data, and we modify them by functions.

This gets a bit theoretical so we can use Pac-Man as an example. In Pac-Man, we can say we have the class of ghosts, and each ghost: Inky, Blinky, Pinky and Clyde are instances of the type. They share general attributes such as wanting to chase pacman and having similar appearances and general behavior. However, they are different when it comes to other attributes such as color, speed and behavior. 

<img src ="https://drive.google.com/uc?export=view&id=11ZQuip9lTodnAjNRXzHn79SHMOOW7Z4Y" />

If we treat these ghosts as objects it is easier to talk about them and how they work, rather than if they are arrays which are updated by functions in every frame. However, if we want to treat their behavior strictly mathematically, it is easier to treat them functionally.

#### What is a Class?

When we define a Class, we are defining a blueprint for creating similar objects. DataFrame is a Class of objects, and while any 2 instances of a DataFrame may seem different in terms of their content, all DataFrames share some fundamental structural and functional similarities, (e.g. they all have an attribute called `columns`, and they all have a method called `.head()`)

Once we define a class of objects, we can create as many instances of that class as we like. (There's no limit to the number of individual DataFrames or lists or dictionaries that we can create!)

### Case Study: Grocery Store Software

Suppose we are building software to digitize the operations of a chain of grocery stores. We'll want to define classes of objects that characterize a business of this type, like Stores, Shopping Baskets, and Customers. 

Let's start with a Store class.


```python
class Store: #convention is to use capitals
    pass
```

This is actually all we need to start creating instances of our Store class, which are called objects (although they won't be very interesting).


```python
s1 = Store()
s2 = Store()

print(s1) 
print(s2)
```

    <__main__.Store object at 0x00000209EA378430>
    <__main__.Store object at 0x00000209E81124C0>
    

An important bit of terminology here is the difference between **classes** and **objects**

* `Store` is a **class**, it is a blueprint telling us how to make Store objects
* `s1` and `s2` are **objects**, they are instances of the `Store` class. They use the blueprint we defined in the `Store` class

Let's change our class so that we can give these Stores some attributes when we create them. (Specifically a location, employee count, and a manager name). 

To set this up, we will need to add an _initialization method_ to our class. This is a method that takes in arguments when the user creates the object and assigns them to attributes.


```python
class Store:
    def __init__(self, location, employees, manager):

        self.location = location 
        self.employees = employees 
        self.manager = manager

# in the init method
# the arguments have the same name as the attributes they define
```

This initialization method must be called `__init__` and has a very specific syntax. Its arguments are the attributes that we'd like the Store to have when it is created, along with a special argument called `self`, which is always the first argument. The reason for having the `self` argument is a bit advanced, so for now we will just take it as fact that we need that keyword as the first argument in our functions. 

Let's recreate some Stores now that we can give them attributes.


```python
s1 = Store('downtown', 100, 'Josh Brown')
s2 = Store('rural', 25, 'Sally Fields')
```


```python
print(s1.location)
print(s2.employees)
```

    downtown
    25
    

Now that we can access the Item attributes, we can also modify them. 

The route 23 location just hired an apprentice fishmonger, the employees attribute for this store needs to be updated.


```python
s2.employees = 26
print(s2.employees)
```

    26
    

In addition to attributes, classes also have ___methods___. Recall that a method is similar to a function, but can only operate on objects in its class. (For example, lists have a method called `.append()`, OLS models have a method called `.summary()`.) 

Let's add a new method to our Store class:


```python
# Redefine the class with this new method
class Store:
    def __init__(self, location, employees, manager):

        self.location = location 
        self.employees = employees 
        self.manager = manager
        
    def print_job_ad(self,role = 'cashier'):
        '''
        Creates a job ad for a store
        '''
        print(f'Now Hiring {role} at our {self.location} location')
        print(f'No experience necessary! Ask to speak to {self.manager} for more information')
        
        


# Recreate our Item objects
s1 = Store('downtown', 100, 'Josh Brown')
s2 = Store('rural', 25, 'Sally Fields')
```


```python
s1.print_job_ad()
```

    Now Hiring cashier at our downtown location
    No experience necessary! Ask to speak to Josh Brown for more information
    


```python
s2.print_job_ad('baker')
```

    Now Hiring baker at our rural location
    No experience necessary! Ask to speak to Sally Fields for more information
    

Our `.print_job_ad()` method also takes in the `self` argument. (In fact, all methods will have `self` as their first argument.) This gives our method access to all of the attributes of the particular Store object that we call it on. 

We're able to print out unique advertisements for each object in our Store class, just like we can get unique summaries for each of our DataFrames, or unique movements for each ghost in our game of Pac-Man.

#### Exercise 1

Write a new method for our Store class called `change_manager()`. This method should take in a new manager and set it as the Store's manager.


```python
class Store:
    def __init__(self, location, employees, manager):

        self.location = location 
        self.employees = employees 
        self.manager = manager
        
    def print_job_ad(self,role = 'cashier'):
        '''
        Creates a job ad for a store
        '''
        print(f'Now Hiring {role} at our {self.location} location')
        print(f'No experience necessary! Ask to speak to {self.manager} for more information')
        
    def change_manager(self,new_manager):
        '''
        INPUT: new_manager
        OUTPUT: none
        
        takes in new manager, and overrides manager with it
        
        '''
    ### Your code goes here
        old_manager=self.manager
        self.manager=new_manager         # or self.manager = self.manager.replace(self.manager,new_manager)    
        print(f'The manager at {self.location} has changed from {old_manager} to {new_manager}.')
        
# Recreate our Item objects
s1 = Store('downtown', 100, 'Josh Brown')
s2 = Store('rural', 25, 'Sally Fields')
```


```python
s1.change_manager('asli')
```

    The manager at downtown has changed from Josh Brown to asli.
    


```python
print(s1.manager)
```

    asli
    

---

#### Classes that interact

Now that we've seen the basics, let's look at a more complex case. Classes rarely live in isolation. Typically, instances of one class will interact with instances of another class. (For example, we regularly create DataFrames from dictionaries, and we regularly pass arrays into OLS models.)

Now that we've laid the foundation for our grocery management software by creating the Store class, we will proceed to create a class called Basket, which represents a customer's shopping basket for transactions at one of our grocery stores. We will also work our way toward building a Customer class, which represents the different users who may be creating baskets while shopping.

Let's start with Basket:


```python
class Basket:
    '''
    We can use docstrings in objects, just like functions
    '''

    max_size = 100


# Create an instance
b = Basket()
print(b.max_size)
# help(Basket) # we have help implemented using our docstring and defined attributes
```

    100
    


```python
help(Basket)
```

    Help on class Basket in module __main__:
    
    class Basket(builtins.object)
     |  We can use docstrings in objects, just like functions
     |  
     |  Data descriptors defined here:
     |  
     |  __dict__
     |      dictionary for instance variables (if defined)
     |  
     |  __weakref__
     |      list of weak references to the object (if defined)
     |  
     |  ----------------------------------------------------------------------
     |  Data and other attributes defined here:
     |  
     |  max_size = 100
    
    

Notice here we created an attribute called `max_size` that doesn't use the `self` prefix. This means that this attribute is applied to *all* baskets. We've chosen to set a hard upper limit on the size of the transaction baskets.

Let's use the initialization method to create some object-specific attributes:


```python
class Basket:
    '''
    A Class to represent a shopping basket
    '''
    
    max_size = 100

    def __init__(self, items, amounts, costs):

        self.items = items
        self.amounts = amounts
        self.costs = costs


```


```python
b1 = Basket(['Razor', 'Milk', 'Bread'], 
            [1, 2, 3], 
            [12.99, 4.99, 2.99])
print(b1.items)

b2 = Basket(['apples'], [3], [0.70])
print(b2.items)
```

    ['Razor', 'Milk', 'Bread']
    ['apples']
    


```python
b1.max_size
```




    100




```python
b1.max_size = 200
```


```python
b1.max_size
```




    200



We now have a mildly useful Class. We can hold transactions as Baskets, and in the next exercise we will enforce the types and data structures we expect.

---
#### Exercise 2

1. We can use assertions inside classes and methods, just like in functions. Add assertions inside the `__init__` to ensure that items is a list with all strings, amounts is a list with all integers, and costs is a list with all floats.

2. Write an assertion, inside the `__init__`, which enforces the max_size attribute (hint, access it using `self.max_size`).

3. Write a new method called `add_item()`, which takes an (item, amount, price) tuple and adds the elements onto the appropriate basket attributes.


```python
class Basket: 
    '''
    A Class to represent a shopping basket
    '''
    
    max_size = 100   #this may not be under definition but still part of Basket blueprint so we don't need to define
                    # We could have done the same with costs by creating a costs list. costs=[]
    
    def __init__(self, items, amounts, costs):
        
        #check that the max_Size isn't broken  #Question 2
        
        assert max(len(items),len(amounts),len(costs)) <=self.max_size, "max size is 100"

        
        ### assert some piee of code that returns true/false, "error message if false" #Question 1
        assert isinstance(items,list),  "items must be a list"
        
        #loop through items list, checking each for its type
        for i in range(len(items)):     
            assert isinstance(items[i],str),"each item must be a string"
            
        assert isinstance(amounts,list),  "amounts must be a list"
        
        #loop through items list, checking each for its type
        for i in range(len(amounts)):    
            assert isinstance(amounts[i],int),"each amount must be a int"
            
        assert isinstance(costs,list),  "costs must be a list"
        
        #loop through items list, checking each for its type
        for i in range(len(costs)):     
            assert isinstance(costs[i],float),"each cost must be a float"
       
        self.items = items
        self.amounts = amounts
        self.costs = costs
        
        def add_item (self, new_item):
            # (item, amount, price) tuple
            item,amount,price=new_item
            
            self.items.append(item)
            self.amounts.append(amount)
            self.costs.append(cost)
            
            
```


```python
# alternative solution

class Basket: 
    '''
    A Class to represent a shopping basket
    '''
    
    max_size = 100
    
    def __init__(self, items, amounts, costs):
        
        assert max(len(items),len(amounts),len(costs)) <=self.max_size, "max size is 100"
        
        all(isinstance(items,float) for item in items)  #all
        all(isinstance(amounts,float) for amount in amounts)  
        all(isinstance(costs,float) for cost in costs)  
        
        self.items = items
        self.amounts = amounts
        self.costs = costs
        
        ### your code here
        
    def add_item (self, new_item):
            # (item, amount, price) tuple
        item,amount,price=new_item
            
        self.items.append(item)
        self.amounts.append(amount)
        self.costs.append(cost)

 

```

---

Let's create a new method for our Basket class that calculates the total cost of the basket:


```python
class Basket:
    '''
    A Class to represent a shopping basket
    '''

    max_size = 100            # we didn't say self.max_size. 

    def __init__(self, items, amounts, costs):

        self.items = items
        self.amounts = amounts
        self.costs = costs

    def cost(self):
        '''
        Calculates the total cost of a basket
        '''
        basket_cost = 0
        
        for i in range(len(self.amounts)):
            basket_cost += (self.amounts[i]*self.costs[i])
        
        return round(basket_cost,2)
```


```python
b1 = Basket(['Razor', 'Milk', 'Bread'], 
            [12, 1, 3], 
            [12.99, 4.99, 2.99])
print(b1.cost())

b2 = Basket(['apples'], [3], [0.70])
print(b2.cost())
```

    169.84
    2.1
    

Question: how can the cost method be implemented using numpy?

---

Now that we've built out a fairly robust Basket class, we can implement a class to represent our customers.

#### Exercise 3

Finish building the Customer class below. A customer can have multiple baskets in their history, which were either added as arguments in the `init` call or via the `add_basket` method. Complete the `total_spent` method.


```python
class Customer:
    
    def __init__(self, cust_id, baskets = None):
        self.cust_id = cust_id
        self.baskets = []
        if baskets is not None:
            self.add_basket(baskets)
    
    def add_basket(self, baskets):
        for i in baskets:
            self.baskets.append(i)
            
    def total_spent(self):
        
        '''
        calculates how much customer has spent
        '''
       
        ### your code here
        
        total_spent=0
        
        for basket in self.baskets:
            total_spent += basket.cost()
            
        return total_spent

    
```


```python
### Testing the Customer class
    
# Test 1:

# Create an instance of a Basket
b1 = Basket(['Razor', 'Milk', 'Bread'], 
            [1, 2, 3], 
            [12.99, 4.99, 2.99])

# Create a Customer with that Basket
customer1 = Customer(1, [b1])

print( customer1.total_spent() ) # should return $31.94

# Test 2:

# Create more baskets
b2 = Basket(['apples'], [3], [0.70])
b3 = Basket(['bananas'], [7], [0.33])

# Add it to this Customer's basket history 
customer1.add_basket([b2,b3])

print( customer1.total_spent() ) # should return $36.35
```

    31.94
    36.35
    

---

### Special Methods

Along with `__init__`, there are many other special methods. [You can see the docs here](https://docs.python.org/3/reference/datamodel.html#basic-customization). A useful method is `__str__` which determines how we print our object, as well as `__eq__`, which determines how equality is defined. You can imagine we could use `__eq__` to easily compare basket costs.


```python
class Basket: 
    '''
    A Class to represent a shopping basket
    '''
    max_size = 100
    
    def __init__(self, items, amounts, costs):

        self.items = items
        self.amounts = amounts
        self.costs = costs
    
    def cost(self):
        '''
        Calculates the total cost of a basket
        '''
        
        basket_cost = 0
        
        for i in range(len(self.amounts)):
            
            basket_cost += (self.amounts[i]*self.costs[i])
        
        return round(basket_cost,4)
    
    def __eq__(self, other_basket):
        
        return self.cost() == other_basket.cost()
    
```


```python
# We've modified how equality is defined for this class
b1 = Basket(['Bread'], [2], [2.99])
b2 = Basket(['Bread'], [3], [2.99])

print(b1 == b2)

b3 = Basket(['Honey'], [1], [5.98])
print(b1 == b3)
```

    False
    True
    

---

#### Exercise 4

Modify our Basket class to include a new `__str__` method. 


1. When we call `print()` on a basket, we want to see each item and its quantity displayed side-by-side.

    For example: <br>
        Apples, 5
        Bananas, 7
        ...
        
**hint**: you might find the "\n" new line character useful


```python
class Basket: 
    '''
    A Class to represent an online shopping basket
    '''
    
    max_size = 100
    
    def __init__(self, items, amounts, costs):

        self.items = items
        self.amounts = amounts
        self.costs = costs
    
    def cost(self):
        '''
        Calculates the total cost of a basket
        '''
        
        basket_cost = 0
        
        for i in range(len(self.amounts)):
            basket_cost += (self.amounts[i]*self.costs[i])
        
        return round(basket_cost,4)
    
    ### your code here
    def __str__(self):
    
        basketstring=''
    
        for i in range(len(self.items)):
            basketstring+=self.items[i]
            basketstring+=','
            basketstring+=str(self.amounts[i])
            basketstring+='\n'
        
        return basketstring
    


```


```python
#now define a basket:

# We've modified how the `print()` function works for this class
b4 = Basket(['Razor', 'Milk', 'Bread'], 
            [1, 2, 3], 
            [12.99, 4.99, 2.99])
print(b4)
```

    Razor,1
    Milk,2
    Bread,3
    
    

### Inheritance

Inheritance refers to objects which are 'subtypes' of other objects. When we want to inherit from another class type, we put the class we want to inherit from (called the **superclass**) in brackets

We keep all of the attributes and methods from our superclass, but add or overwrite any we include in our new class.

In our case, we'd like to build a class of objects that give our customers the ability to make shopping lists in a consumer facing app. Most of the functionality we need is already defined in Python's List. So we will make our Shopping List a **subclass** of List.


```python
class ShoppingList(list): # list is a superclass of ShoppingList, ShoppingList is a subclass of list
    def __str__(self):
        
        return 'Your shopping list contains: ' + super.__str__(self)

# Initializing is a bit different
sl1 = ShoppingList(['grapes','cookies','milk'])

#Just like an old list:
sl1.append('eggs')
print(sl1.pop())

#But with the cool print:
print(sl1)
```

    eggs
    Your shopping list contains: ['grapes', 'cookies', 'milk']
    

--------

### Advanced Functions

Let's write a function that would be useful for our grocery store software. We will define it in the usual way, using the `def` expression. 


```python
def price_plus_tax(price):
    return price*1.07 # hard coded 7% sales tax

price_plus_tax(3)
```




    3.21



If we only use this function once, we can use what is known as a `lambda` expression. `lambda` functions are a useful way of referring to a custom function without naming the function with the `def` expression. 


```python
(lambda price: price*1.07)(3)
```




    3.21



Another example, a function that takes in a customers name and outputs a welcome message, defined first the usual way, and then using a `lambda`:


```python
def customer_welcome(name):
    return print(f"Welcome {name}!")

customer_welcome("Adam")
```

    Welcome Adam!
    


```python
# print custom hello with a lambda function

(lambda name: print(f"Welcome {name}!"))("Adam")
```

    Welcome Adam!
    

`lambda` functions help when you need to pass a function to another method, like when using custom `pandas` transformations. But if you plan on naming and reusing your function or assigning it to a variable, you should use `def` instead:


```python
# good practice

def price_plus_tax(price, tax_rate): 
    return price*tax_rate

print(price_plus_tax(2.00,1.10))

# the following declaration also works but is considered an ANTI-PATTERN
# anti-patterns are bad coding practice that should be avoided
# using a syntactic pattern in a way other than the way it was designed to be used

price_plus_tax = lambda price,tax_rate: price*tax_rate
print(price_plus_tax(2.00,1.10))

```

    2.2
    2.2
    

You might already be familiar with lambdas as they are often used in conjunction with pandas DataFrames. Let's demonstrate this by making up a dataset with information about customer shopping baskets. In addition to a key field called basket_id, our dataset will have a column representing the size (number of items) contained in the basket.


```python
# define list of tuples with basket id and random numbers of items
baskets  = list(zip(range(1000),np.random.choice(6, 1000, p=[0.1, 0, 0.3, 0.5, 0,0.1]))) 
# put into a DataFrame
shopping_basket_df = pd.DataFrame(baskets,columns = ['basket_id','n_items'])
```


```python
shopping_basket_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>basket_id</th>
      <th>n_items</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



Now we want to label encode baskets depending on their size. Baskets with more than 5 items are considered big, and those with 5 or fewer are considered not big. We will see a few ways of doing this, all of which make use of the `apply` method. 


```python
# define a lambda and pass it through the apply method to decide if
# the size of the shopping basket is big or not
shopping_basket_df['n_items'].apply(lambda x: 'big' if x > 5 else 'not big')
```




    0      not big
    1      not big
    2      not big
    3      not big
    4      not big
            ...   
    995    not big
    996    not big
    997    not big
    998    not big
    999    not big
    Name: n_items, Length: 1000, dtype: object




```python
def big_basket(x):
    
    if x > 5:
        return 'big'
    else:
        return 'not big'
```


```python
shopping_basket_df['n_items'].apply(big_basket)
```




    0      not big
    1      not big
    2      not big
    3      not big
    4      not big
            ...   
    995    not big
    996    not big
    997    not big
    998    not big
    999    not big
    Name: n_items, Length: 1000, dtype: object




```python
shopping_basket_df['n_items'].apply(lambda x: big_basket(x))
```




    0      not big
    1      not big
    2      not big
    3      not big
    4      not big
            ...   
    995    not big
    996    not big
    997    not big
    998    not big
    999    not big
    Name: n_items, Length: 1000, dtype: object



`apply` is a method associated with the class DataFrame. A method is a function that can only operate on objects of the class it is defined for. But the apply method is a also an example of a functional, a function that takes functions as input. Take another glance at the examples above to see this clearly.

We can also write functions that return functions as output. These are sometimes called function factories.

Our grocery stores might be spread across jurisdictions with different rates of sales tax. We can generate a price-plus-tax function for a range of tax rates.


```python
def make_tax_calculator(tax_rate):
    '''
    Return a function that calculates price plus tax given .
    '''
    
    def price_plus_tax(price):
        return round(price*tax_rate,3)
    
    return price_plus_tax


# we want the function price + 7% sales tax
sales_tax_7p = make_tax_calculator(1.07)
# test it with 3**2
sales_tax_7p(3)
```




    3.21




```python
# make a list of price-plus-tax calculators

my_list_of_tax_calculators = [make_tax_calculator(y) for y in np.arange(1,1.1,.01)]
```


```python
my_list_of_tax_calculators
```




    [<function __main__.make_tax_calculator.<locals>.price_plus_tax(price)>,
     <function __main__.make_tax_calculator.<locals>.price_plus_tax(price)>,
     <function __main__.make_tax_calculator.<locals>.price_plus_tax(price)>,
     <function __main__.make_tax_calculator.<locals>.price_plus_tax(price)>,
     <function __main__.make_tax_calculator.<locals>.price_plus_tax(price)>,
     <function __main__.make_tax_calculator.<locals>.price_plus_tax(price)>,
     <function __main__.make_tax_calculator.<locals>.price_plus_tax(price)>,
     <function __main__.make_tax_calculator.<locals>.price_plus_tax(price)>,
     <function __main__.make_tax_calculator.<locals>.price_plus_tax(price)>,
     <function __main__.make_tax_calculator.<locals>.price_plus_tax(price)>,
     <function __main__.make_tax_calculator.<locals>.price_plus_tax(price)>]




```python
my_list_of_tax_calculators[2](3)
```




    3.06



What can we do with this list of functions? we can apply them each to a single item and store the outputs. You can think of this as the reverse of an apply method, instead of mapping a single function to a bunch of items (like rows in a DataFrame), we can map a bunch of functions to a single item.


```python
# make a function that takes an object
# applies a list of functions to the object
# and puts the outputs into a list

def map_funcs(obj, func_list):
    
    '''take and object and a list of functions and apply each function to the object and return a list'''
    
    return [func(obj) for func in func_list]
```

What is the final price of a 12 dollar item, depending on tax rate?


```python
map_funcs(12.0,my_list_of_tax_calculators)
```




    [12.0, 12.12, 12.24, 12.36, 12.48, 12.6, 12.72, 12.84, 12.96, 13.08, 13.2]



The examples in this section have been demonstrations of the 'functional style' of programming. We have seen functions that take functions as input, functions that generate other functions, and more. It is not easy or natural to do this in some languages like C or Java, but it is fairly straightforward to do in Python.

When we write functions, or any kind of code, we have to be mindful of the the names we give our variables so as to limit confusion and ensure our code executes as intended. This is where scoping comes in.



### Scoping

Scoping is the process of determining which value to use for a given variable.



```python
name = "Adam"
def customer_welcome(name):
    return print(f"Welcome {name}!")

customer_welcome("Larry")
```

    Welcome Larry!
    

We can see that we used `name` in **two contexts**. We used it outside the function as "Adam" and then inside the function where we reassigned it, **they did not interact with each other**.

This is because `name` is _scoped_ locally inside the function.

Python uses LEGB scoping. This means that if we try and find the value for a given variable, we first look **l**ocally (ie. inside the function), then in the **e**nclosing environment, then **g**lobally, and then at **b**uilt-ins:


```python
c = 3

def my_func(a, b):
    print(a, ' is local to the function')

    def my_func2():
        print(b, ' is in the enclosing environment')
    
    my_func2()
    
    print(c, ' is globally scoped')
    print(abs, ' is a built in function')
```


```python
my_func(1,2)
```

    1  is local to the function
    2  is in the enclosing environment
    3  is globally scoped
    <built-in function abs>  is a built in function
    


```python
print(c)
print(a) # a is not in global scope
```

    3
    


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_29112/1362798335.py in <module>
          1 print(c)
    ----> 2 print(a) # a is not in global scope
    

    NameError: name 'a' is not defined


Getting globally scoped objects is pretty risky! It is generally sensible to use them as arguments to your function. 

We can modify values from inside functions by using the `global` function, but again, this is not usually good practice:


```python
name = "Adam"

def customer_welcome(customer_name):
    global name
    name = customer_name
    return print(f"Welcome {customer_name}!")

customer_welcome("Larry")
print(name)
```

    Welcome Larry!
    Larry
    

### Iterators, Generators and Range

Recall our Basket class and its `cost` method. Here is how we defined it:

```
    def cost(self):
        '''
        Calculates the total cost of a basket
        '''
        basket_cost = 0
        
        for i in range(len(self.amounts)):
            basket_cost += (self.amounts[i]*self.costs[i])
        
        return round(basket_cost,4)
```


We made use of `range` to iterate over a pair of lists. You have probably used it many times yourself to write code similar to this, but what exactly is `range`? It sure does give a strange print out:


```python
range(10)
```




    range(0, 10)



Let's take a look at a more interesting example:


```python
range(10**100)
```




    range(0, 10000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000)



Given that there are about $10^{80}$ atoms in the universe, it is likely that we have not constructed a list which contains them all.

Actually, `range` is a class! A class with a method that allows it to perform lazy evaluation, meaning we get the next item in the range only when we need it. This is extremely useful for conserving memory and helps us write more efficient programs.

Here are a couple of other built in functions that work in a similar manner:


```python
print(map(lambda x: x*1.07, [1, 2, 3, 4, 5]))
print(zip(['banana', 'iced tea', 'shrimp'], [1, 2, 3]))
```

    <map object at 0x00000209EA378700>
    <zip object at 0x00000209EA404BC0>
    


`Map` here is mapping the given function onto each item in the list.

`Zip` zips together items - useful if we want to iterate over more than one item at a time.

Map and zip objects are examples of **iterators**. 

Iterator objects are characterized by two special methods, `__iter__` and `__next__`. The special method `__iter__` gives the object a state such that it "remembers where it is" during iteration, and `__next__` tells the object how to get its next value.



```python
item_quantity = zip(['banana', 'iced tea', 'shrimp'], [1, 2, 3])
```


```python
next(item_quantity)
```




    ('banana', 1)




```python
print(list(item_quantity))
```

    [('iced tea', 2), ('shrimp', 3)]
    

Is a list an iterator? Well, we can check the special methods it has implemented using the `dir` function. 


```python
print(dir(list()))
```

    ['__add__', '__class__', '__class_getitem__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
    

Nope, list is not an iterator. You'll notice `__iter__` is implemented but not `__next__`. This seems odd, since we iterate over lists all the time.

When you write a for loop on a list, what happens is that the special `__iter__` method is called and the list is made to be an iterator. We can force a list to be an iterator by calling the `iter` function on it.


```python
my_items = ['apple','bbq sauce']
my_items_iter = iter(my_items) # put list in iter function to make it an iterator
print(dir(my_items_iter)) # notice now we have __next__
```

    ['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__length_hint__', '__lt__', '__ne__', '__new__', '__next__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__setstate__', '__sizeof__', '__str__', '__subclasshook__']
    


```python
print(next(my_items_iter))
print(next(my_items_iter))
```

    apple
    bbq sauce
    

What happens when we call `next` once we've run out of items to iterate over?


```python
print(next(my_items_iter))
```


    ---------------------------------------------------------------------------

    StopIteration                             Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_29112/2273279305.py in <module>
    ----> 1 print(next(my_items_iter))
    

    StopIteration: 


Once an iterator has hit the _StopIteration_ error, it is empty, and we have to reinitialize it.

**Range is not 100% the same**, we cannot call `next` on it, and it has some extra attributes:


```python
print(dir(range(1)))
```

    ['__bool__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'count', 'index', 'start', 'step', 'stop']
    


```python
x = range(10)

#Get some elements from range
print(x.start)
print(x.step)

#But it's not an iterator...
next(x)
```

    0
    1
    


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_29112/3882953812.py in <module>
          6 
          7 #But it's not an iterator...
    ----> 8 next(x)
    

    TypeError: 'range' object is not an iterator


We can make iterators in one of two easy ways. The first is by wrapping an iterable in the `iter` function as we did in the list example above. Here we apply `iter` to a range object.


```python
# we can make a range object an iterator by feeding it to the iter function

x = range(10)
range_iter = iter(x)

print(next(range_iter))
print(next(range_iter))
```

    0
    1
    

The second is by making our own, by replacing the `return` keyword in a function with the `yield` keyword:


```python
# if you use the yield keyword instead of return
# your function will have the iterator property
prices = [1, 2, 3, 4, 5]

def price_plus_tax(prices):
    for price in prices:
        yield price*1.07 # 7% sales tax


```


```python
price_plus_tax(prices)
```




    <generator object price_plus_tax at 0x00000209EA431F90>



Technically, what we've created here is a generator, which is a special case of iterator. You can make your own iterators that handle special/complex state that go beyond the capability of a generator. But for most tasks generators are sufficient.

We mentioned earlier that one reason to use iterators is because of computational efficiency. Let's demonstrate this with an example.

Suppose we want to write a program to adjust prices on our goods due to changing market conditions.


```python
def list_new_prices(item_prices):
    new_item_prices = []
    for item_price in item_prices:
        new_price = item_price*1.01 # raise price by 1%
        new_item_prices.append(new_price)
        
    return new_item_prices
```


```python
old_prices = range(10000000)
new_prices = list_new_prices(old_prices)
```

How much memory does it take up to calculate all new prices and store in a list?


```python
print(f"{round(sys.getsizeof(new_prices)/(1e6),4)} MB")
```

    89.0952 MB
    

`get_new_prices` can be written as a generator instead as follows:


```python
def generate_new_prices(item_prices):
    for item_price in item_prices:
        yield item_price*1.01 # raise price by 1%
```


```python
new_prices = generate_new_prices(old_prices)
```


```python
new_prices
```




    <generator object generate_new_prices at 0x00000209EA43A430>



How does memory consumption of the generator compare to that of the list?


```python
print(f"{round(sys.getsizeof(new_prices)/(1e6),4)} MB")
```

    0.0001 MB
    

When we need a price update, we can call `next`.


```python
next(new_prices)
```




    0.0



Iterators do not need to be finite, the following iterator mints new customer id's by yielding the next natural number every time it is called.


```python
def customer_id_generator():
    cust_id = 0
    while True :
        yield cust_id
        cust_id += 1
        
mint_new_customer_id = customer_id_generator()
```


```python
# let's see

for i in range(5) :
        print(next(mint_new_customer_id))
    
```

    0
    1
    2
    3
    4
    

We can also use comprehension syntax to create a generator. Just replace the square brackets of a list comprehension with round brackets as shown below.


```python
new_prices = [old_price*1.01 for old_price in old_prices]
new_prices = (old_price*1.01 for old_price in old_prices)
```


```python
next(new_prices)
```




    0.0



another range example:


```python
list(itertools.combinations(range(1,4),2))
```




    [(1, 2), (1, 3), (2, 3)]



--- 

#### Exercise 5

In data science we will often have to create combinations of variables, columns or data points. Iterators give us a fast and efficient way of doing this.

The `itertools` package is part of the standard library, and contains many useful combinatorial functions, which mostly produce iterators. Take a look at the [documentation here](https://docs.python.org/3/library/itertools.html).

The Deli departments at our stores are launching a new 'Charcuterie Snack Pack' product line, where they will sell packaged pairings of meats and cheeses for on-the-go customers who need a quick but hearty treat. We need to create labels for each meat/cheese combination.

1. Create a nested for loop to generate all pairwise combinations of `meat_list` and `cheese_list`
2. Create the same using an itertools function


```python
meat_list = ['genoa salami',
             'oven gold turkey',
             'spicy cappicola',
             'mortadella',
             'roast beef']


cheese_list = ['mozzarella',
               'pepper jack',
               'vermont cheddar',
               'smoked gouda',
               'provolone']
```

1.


```python
charcuterie_pairs=[]

for meat in meat_list:
    for cheese in cheese_list:
        charcuterie_pairs.append((meat,cheese))

print(charcuterie_pairs)
```

    [('genoa salami', 'mozzarella'), ('genoa salami', 'pepper jack'), ('genoa salami', 'vermont cheddar'), ('genoa salami', 'smoked gouda'), ('genoa salami', 'provolone'), ('oven gold turkey', 'mozzarella'), ('oven gold turkey', 'pepper jack'), ('oven gold turkey', 'vermont cheddar'), ('oven gold turkey', 'smoked gouda'), ('oven gold turkey', 'provolone'), ('spicy cappicola', 'mozzarella'), ('spicy cappicola', 'pepper jack'), ('spicy cappicola', 'vermont cheddar'), ('spicy cappicola', 'smoked gouda'), ('spicy cappicola', 'provolone'), ('mortadella', 'mozzarella'), ('mortadella', 'pepper jack'), ('mortadella', 'vermont cheddar'), ('mortadella', 'smoked gouda'), ('mortadella', 'provolone'), ('roast beef', 'mozzarella'), ('roast beef', 'pepper jack'), ('roast beef', 'vermont cheddar'), ('roast beef', 'smoked gouda'), ('roast beef', 'provolone')]
    


```python
#alternative solution:
unique_combinations = []
 
# looping over list
for i in range(0,len(meat_list)):
    for j in range(0,len(cheese_list)):
       
        # checking if i and j indexes are not on same element (we do not need to use it in this example)
        #if (i!=j):
             
            # add to output
            unique_combinations.append((meat_list[i],cheese_list[j]))
 
# view output
print(unique_combinations)
```

    [('genoa salami', 'mozzarella'), ('genoa salami', 'pepper jack'), ('genoa salami', 'vermont cheddar'), ('genoa salami', 'smoked gouda'), ('genoa salami', 'provolone'), ('oven gold turkey', 'mozzarella'), ('oven gold turkey', 'pepper jack'), ('oven gold turkey', 'vermont cheddar'), ('oven gold turkey', 'smoked gouda'), ('oven gold turkey', 'provolone'), ('spicy cappicola', 'mozzarella'), ('spicy cappicola', 'pepper jack'), ('spicy cappicola', 'vermont cheddar'), ('spicy cappicola', 'smoked gouda'), ('spicy cappicola', 'provolone'), ('mortadella', 'mozzarella'), ('mortadella', 'pepper jack'), ('mortadella', 'vermont cheddar'), ('mortadella', 'smoked gouda'), ('mortadella', 'provolone'), ('roast beef', 'mozzarella'), ('roast beef', 'pepper jack'), ('roast beef', 'vermont cheddar'), ('roast beef', 'smoked gouda'), ('roast beef', 'provolone')]
    

2. Alternative solution is using product() which is called to find all possible combinations of elements.
And zip() is used to pair up all these combinations, converting each element into a list and append them to the desired combination list.


```python
import itertools
from itertools import product

# Extract Combination Mapping in two lists
# using zip() + product()

charcuterie_pairs=itertools.product(meat_list,cheese_list)

# printing in ist format

print(list(charcuterie_pairs))
```

    [('genoa salami', 'mozzarella'), ('genoa salami', 'pepper jack'), ('genoa salami', 'vermont cheddar'), ('genoa salami', 'smoked gouda'), ('genoa salami', 'provolone'), ('oven gold turkey', 'mozzarella'), ('oven gold turkey', 'pepper jack'), ('oven gold turkey', 'vermont cheddar'), ('oven gold turkey', 'smoked gouda'), ('oven gold turkey', 'provolone'), ('spicy cappicola', 'mozzarella'), ('spicy cappicola', 'pepper jack'), ('spicy cappicola', 'vermont cheddar'), ('spicy cappicola', 'smoked gouda'), ('spicy cappicola', 'provolone'), ('mortadella', 'mozzarella'), ('mortadella', 'pepper jack'), ('mortadella', 'vermont cheddar'), ('mortadella', 'smoked gouda'), ('mortadella', 'provolone'), ('roast beef', 'mozzarella'), ('roast beef', 'pepper jack'), ('roast beef', 'vermont cheddar'), ('roast beef', 'smoked gouda'), ('roast beef', 'provolone')]
    

---

### Bonus: Recursion (plus Caching and Decorators)

A common interview question is to create the nth number in the Fibonacci sequence:

$$ F_n = F_{n-1} + F_{n-2} $$

There are many, many ways of implementing this.

One of the main things they are looking for is clean, efficient code. One way to measure efficiency is run time. We will write several different solutions for the Fibonacci question above, and use run time to decide which solution is optimal.

Before we try implementing it, let's make sure we understand what is happening with the Fibonacci sequence.

How does the sequence grow?

$$ 1,1,? $$
$$ 1,1,2,? $$
$$ 1,1,2,3,? $$
$$ 1,1,2,3,5,? $$

We have a sequence of numbers where each number is the sum of the preceding two.


```python
# here's one way
def fib1(x):
    n = 2
    a, b = 1,1
    
    if x < 2:
        return a
    else:
        while n <= x:
            a, b = b, a + b
            n += 1
            
    return b
```


```python
fib1(3)
```


```python
%%timeit
[fib1(i) for i in range(10)]
```

This is nice code, but we can think a little harder, and make a recursive solution.

Recursion allows us to loop back, and use the same function inside itself. Fibonacci is the classic example of recursion:


```python
def fib2(x):
    if x < 2:
        return 1
    else:
        return fib2(x - 1) + fib2(x - 2)
```

How does this work?

To understand this recursion, think about `fib2` for smaller $ x $ and then bootstrap this logic. consider $ x $ = 3.

$$ fib2(3) = fib2(2) + fib2(1) = (fib2(1) + fib2(0)) + 1 = (1 + 1) + 1 $$

This works for any $ x $.


```python
fib2(3)
```

But how efficient is this implementation from a runtime perspective? In Jupyter notebooks, we have a convenient method to measure runtime.

We can use [IPython magic function](https://ipython.readthedocs.io/en/stable/interactive/magics.html) `%timeit` to see how long a line takes to run, or `%%timeit` to see the whole cell.


```python
%%timeit
[fib2(i) for i in range(10)] 
```

Cool, but what if we want a few more numbers:


```python
%timeit [fib1(i) for i in range(30)]
```


```python
%timeit [fib2(i) for i in range(30)]
```

Our performance is terrible! With a bit of thinking, we can see that if we call fib2(5), we have to calculate fib2(4) and fib2(3), but fib2(4) calls fib2(3) again, and it multiplies out.

One of the key parts of recursive programming is to make sure we do not run the same function on the same value more than once.

We can cache the results!


```python
# caching just means storing the results of the work we've already done
# so we don't have to re-do operations
# remember, when we call fib2(5), we do a lot of redundant work
```


```python
fib_cache = {}
def fib3(x):
    if x in fib_cache:
        return fib_cache[x]
    else:
        if x < 2:
            fib_cache[x] = 1
        else:
            fib_cache[x] = fib3(x - 1) + fib3(x - 2)
            
    return fib_cache[x]

print(fib3(3))
print(fib_cache)       
```


```python
# we store the results of our computation as we move along
# this function has memory
```

What have we done here? We have a function, which checks if the value already exists in a dictionary. If it is found, we return the value from the dict, otherwise we run the function, and put the results in the dict.

Let's time it:


```python
%%timeit

[fib3(i) for i in range(100)]
```

So, we can see this is super fast but it was a bit of work!

Can we generalize this?

The answer is yes, we have a way of adding this functionality to arbitrary functions:


```python
from functools import lru_cache

@lru_cache()
def fib4(x):
    if x < 2:
        return 1
    else:
        return fib4(x - 1) + fib4(x - 2)
```


```python
fib4(3)
```


```python
%%timeit 
[fib4(i) for i in range(100)]
```

What exactly have we done here? And what is that `@` symbol?

If a function returns a function, we can use it to modify our own functions. For example:


```python
def my_decorator(function):
    
    def my_func(x):
        print('running your function now')
        output_of_function = function(x)
        print('done running')
        
        return output_of_function * output_of_function
    
    return my_func

def my_adder(x):
    return x + 1

wrapped_function = my_decorator(my_adder)
wrapped_function(5)
```

A shortcut for this is to use the decorator notation:


```python
@my_decorator
def my_adder(x):
    return x + 1
```


```python
my_adder(5)
```

`lru_cache` stands for least recently used. We keep the n most recently used calls (by default 128) in our cache, and return them if found, rather than running the function. This used the same dict implementation as our naive cache.

Today we have had a whirlwind tour of some of the more pythonic features of the language. Don't worry too much if you didn't follow everything. Python skills come with practice, some good resources are: 
* [Stack Overflow](https://stackoverflow.com/questions/tagged/python), where you can see the most common, or most recent python questions from users around the world. 
* [Project Euler](https://projecteuler.net/) which is a series of math and programming problems with a focus on smart implementations.
* [Rosalind](http://rosalind.info/about/) which is a series of questions based on biology.
* [HackerRank](https://www.hackerrank.com/), which is a site used to screen applicants, but has practice question banks.

The best resource however is just practice. Work on your project, google around, and always be thinking of whether  there is a better way.

---
#### Exercise 6

##### Compound Interest
1. In finance, the formula for compound interest refers to the total amount of principal ($P$) invested, plus annual interest rate of $i$ compounded (usually annually) over  number of time periods $n$. Mathematically, this can be expressed as 
$$ A = P \cdot (1+i) ^ n $$   
where $A$ is the final balance. Using the above expression, calculate the final balance on a $100,000 investment with 5% interest compounding annually over a 12 year period.

2. Compound interest can also be calculated using the following [recurrence relation](https://en.wikipedia.org/wiki/Recurrence_relation):  
$$ A_0 = P $$
$$ A_{t+1} = (1 + i) \cdot A_t $$  
That is to say, the amount at time $t=0$ is just the starting principal, $P$, and at each future time $t+1$, is just the one plus the interest rate multiplied by the amount at the previous time period, $t$.

 Given the above, write a recursive function to calculate the final balance which takes the parameters $P$, $i$, and $n$ as input. Use this function to also provide the answer to question 1 above and check that they match.

##### Population Growth
https://www.cs.sfu.ca/~ggbaker/zju/math/recursion.html
3. Another recurrence relation is that of the [logistic map](https://en.wikipedia.org/wiki/Logistic_map), which describes population growth. The population at time $n+1$, expressed as a fraction of the maximum population that can be sustained by the environment, is the product of the rate of reproduction ($r$), and growth and starvation components based upon the population at previous time $n$:  
$$x_{n+1}=rx_{n}\left(1-x_{n}\right)$$

 where $0 < x_{n} < 1$ for all $n$, and some starting population proportion, $x_0$ is specified.
 
Using a `for` loop and/or numpy array, plot the population cycles for 100 periods with initial starting population $x=0.3$ for $r = 3.4$ and $r = 3.8$.

4. Write a recursive function, `logistic_map`, that takes the starting population, $x$, the rate of reproduction, $r$, and number of cycles, $n$, as parameters and returns the current population $x_n$.

 Use this to calculate the population proportion $x$ at cycle $n=100$ for a starting population of $x=0.3$ and rate of reproduction $r=3.4$ as in the previous question.

5. Using `map` and a lambda function using the above, calculate the population for times $n = 1$ to $n = 100$ as in Question 3 ($x_0 = 0.3$ and $r = 3.4$) and plot.

 Explore different values for the reproduction rate, $r$ for $1 \le r \le 4$. What effect does this have the population dynamics?
 
--- 


```python

```

<div id="container" style="position:relative;">
<div style="position:relative; float:right"><img style="height:25px""width: 50px" src ="https://drive.google.com/uc?export=view&id=14VoXUJftgptWtdNhtNYVm6cjVmEWpki1" />
</div>
</div>
