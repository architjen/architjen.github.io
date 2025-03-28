## Shallow copy vs Deep copy in python

When you use an assignment operator, it doesnt creates a new item instead it binds the new variable with the same memory address as of the old one. It wouldnt make any difference while dealing with the immutable data types, but while dealing with the mutable data types, you might want to reconsider using the assignment operator. In this post, I'll be addressing this issue, and how using shallow & deep copy can save us data overwriting. 

When you declare a ```new_list = old_list```, we are not creating a new list or a copy of it, instead we are allocating the same address to the new_list. lets understand with an example:

#### memory address of the b_list

```python
a_list = [1,2,3,4,5]
b_list = a_list

id(b_list)  
>>> 140530511099648
```

#### memory address of the a_list

```python
id(a_list)
>>> 140530511099648
```

Hence, we can see here that both of the list share the same address and any changes made to one list would directly affect the other list as well. To check this we can even use the equality "==" and "is" operator in python, where "==" compares if the list are equal irrespective of their addresses and "is" compares the list along with their addresses. More precisely, '==' is equality operator and 'is' an identity operator.

The variables in python are not the object buckets, but rather pointers to object land. Hence, when you assign a variable to another variable, we are simply pointing the to the same object instead of containing it.
```python
# check if both the list are the same
a_list == b_list
>>> True
```

```python
# check if they share the same memory address
a_list is b_list
>>> True
```

#### Another example 

```python
c_list = [1,2,3,4,5]
```

```python
#check lists
a_list == c_list
>>> True
```

#### check memory address
```python
a_list is c_list
>>> False
```

As, you can see defining a new list changes the address of the list, even if they exactly look alike. Lets get back to the main topic of shallow and deep copy. To avoid accidentally overwriting the other list, we use the deep or shallow copy depending on our requirement. We make shallow copy when we need 1 level of copy, and we make deep when we need recursive way of copying. Lets take an example for better understanding

#### Creating a list copy
```python
a_list = [1,2,3,4,5]

#creating copy
import copy
b_list = copy.copy(a_list)

#test if they have the same address
b_list is a_list
>>> False
```

```python
#Append an item to a_list
a_list.append(6)

a_list
>>> [1,2,3,4,5,6]
```
```python
b_list
>>> [1,2,3,4,5]
```

#### Copy on multidimensional array
```python
#Lets try multi dimensional array
a = [[1,2,3], [4,5,6]]

b = copy.copy(a)

print(a)
>>> [[1,2,3], [4,5,6]]
```
```python
b
>>> [[1,2,3], [4,5,6]]
```
```python
#adding something to a
a.append(7)
>>> [[1,2,3], [4,5,6], 7]
```
```python
#no change is b
b
>>> [[1,2,3], [4,5,6]]
```
```python
a[0][0] = 123
a
>>> [[123,2,3], [4,5,6], 7]
```
```python
b
>>> [[123,2,3], [4,5,6]]
```

Hence, the copy only works for 1 level, if you want to have a recursive copy technique go for deep copy.
#### Deep Copy
```python
#Lets try 2D dimensional array
a = [[1,2,3], [4,5,6]]

#changing it to deep copy
b = copy.deepcopy(a)

a
>>> [[1,2,3], [4,5,6]]
```
```python
b
>>> [[1,2,3], [4,5,6]]
```
```python
#adding something to a
a.append(7)
>>> [[1,2,3], [4,5,6], 7]
```
```python
#no change is b
b
>>> [[1,2,3], [4,5,6]]
```

```python
a[0][0] = 123
a
>>> [[123,2,3], [4,5,6], 7]
```

```python
b
>>> [[1,2,3], [4,5,6]]
```

Having a deep copy provides a complete isolation from the other list, but is slower than copy. We can use the idea of shallow and deep copy to create a class copy
```python
class Name:
    def __init__(self, fname, lname):
        self.fname = fname
        self.lname = lname

    def __repr__(self):
        return f'Name({self.fname}, {self.lname})'

#create a class object
a = Name('Archit', 'Jain')
a
>>> Name(Archit, Jain)
```

```python
b = copy.copy(a)
b
>>> Name(Archit, Jain)
```

```python
b == a
>>> False
```

```python
b is a
>>> False
```
Make sure you use deepcopy in case you are using multiple level of data, to avoid overwriting in the other object.

[REFERENCE]

https://docs.python.org/3/library/copy.html
