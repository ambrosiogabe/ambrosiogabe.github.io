---
layout: post
title:  "How to Code a List in Java"
date:   2018-01-03 1:57:32 -0500
categories: [Data-Structures]
tags: [programming, java, lists, data-structures]
cover: /public/images/articleHeaders/codeAListInJava.jpeg
---
Intro
=====

This article is going to be a very in depth example of how to code a list in Java. A list is a data structure that holds a number of items that you can access in constant time. The advantage of a list in comparison to an array is that a list expands or shrinks as needed, so there is no need to know the number of elements in advance. This is helpful for programs in which you need a collection of items, but you are not sure how many there are going to be.

Now if you have programmed in Python than you know exactly what lists are. We will implement the same type of technique now in Java and it will function very similarly to the way the Java.util.List works.

Methods
=======

Before we begin programming our list it would be very nice to have a list of the methods that we will need. Now, we would like our list to do the following:

<ul>
  <li>Size(): Return the size of our list</li>
  <li>isEmpty(): Return a boolean of whether our list is empty or not</li>
  <li>get(i): Return whatever element is at the index, i</li>
  <li>add(i, e): Add an element e to the index i while maintaining all the other elements in the list and throws an out of bounds exception if needed</li>
  <li>add(e): Add an element e to the end of the list</li>
  <li>remove(i): Remove the element at index i</li>
</ul>

These are the main functions we would like our list ADT (Abstract Data Type) to do. I will add a bonus section at the end of this article that adds in a few functions that the Python version of lists have. In this bonus section I will include how to program methods that do the following:

<ul>
  <li>reverse(): Reverse the list's ordering</li>
  <li>extend(l): Append the list l to the current list</li>
  <li>indexOf(e): Return the index of element e and throw an empty list exception if the list is empty</li>
  <li>pop(): Returns and removes the element at the end of the list and throws an empty list exception if the list is empty</li>
  <li>pop(i): Returns and removes the element at index i and throws out of bounds exception or empty list exception</li>
  <li>print(): Prints out the array in the format [e1, e2, e3]</li>
</ul>

So follow along if you want and I hope you enjoy learning how to create a list in Java!

Size()
======

To start this off, we would like to declare an abstract class called List and initialize the array and everything. If you have a foreknowledge of Java this should all be fairly straightforward, in code it would look like this:

```java
public class List<Type>
{
  private int size = 0;
  private int totalSize = 1;
  private Type[] array = (Type[])new Object[totalSize];
}

```

All we have done here is created an abstract class called List. Inside this class we have initialized three private variables called size, totalSize, and array. The size simply contains the size of the list, the total size is how big our list is allowed to be, an the array is the data structure we will use to build our list upon.

Now you may be wondering, what the heck is total size? Since our list needs to expand, and contract, with the amount of elements in the list, we need to keep track of the maximum size available at any given moment. Then, when we try to add an element that exceeds the size of the list, we will call a function increaseArraySize() which expands the array by a factor of 2, and when the size is less than one fourth of the total size, we will shrink the list by a factor of 2 by calling a function decreaseSize(). We will get to all this in a very short amount of time here.

Next we will actually code the size() method, this method is very simple. We will add to the code above:

```java
  ...
  public int size() { return size; }
  ...
```

This is just one line of code that returns our private variable size.

IsEmpty()
=========

This next method is also very simple, we simply want to return a boolean of whether the list is empty or not. We can do this by just checking whether or not size = 0. Or, in other words:

```java
  ...
  public boolean isEmpty() { return size == 0; }
  ...

```

This just returns whether size is equal to 0 or not. Moving on...

Get(i)
======

Next up is the get method. This method is seemingly simple, but there is an added complexity which we will get to. Since our list structure is being made with an array we will simply access the element of the array at index i and return it! It really is that simple. Now, we can't add elements yet, but we will get there. In theory, our method should work like so:

```java
...
public Type get(int index)
{
  return array[index]
}
...
```

Well that was easy! But wait, let's think about this. What if our dumb user (as most programmers are ;) ), decides to try and access the element at index 25, but the list is only full of 10 elements! Now we have a problem. In Java, we can program in something to "catch" these exceptions. Instead of using one of Javas exceptions, we will build our own. An appropriate name for this exception would be OutOfBoundsException. So, we will create a new class within the same file as this program, and we will call this class OutOfBoundsException, and it will extend RuntimeException which is Java's way of letting us build our own exception. In code it looks like this:

```java
public class OutOfBoundsException extends RuntimeException
{
  String str1;

  public OutOfBoundsException(String str2)
  {
    str1=str2;
  }

  public String toString()
  {
    return ("MyException Occurred: "+str1) ;
  }
}

```

Now, since we are extending a class here we have to implement all the unused methods. That so happens to be toString(). So what we are simply doing is saying, whenever this exception gets called, initialize it with the string we pass it, and then output to the console "MyException Occured: String that we passed to the exception". So don't be afraid by the method and class declaration in there.

OK, now that we have an OutOfBoundsException we can throw it. Let's modify our get method to throw an error if somebody tries to access an element that doesn't exist:

```java
...
public Type get(int index) throws OutOfBoundsException
{
  if(index < size)
  {
    return array[index];
  }
  else
  {
    throw new OutOfBoundsException("List index " + index + " exceeds size " + size + " of list.");
  }
}
...
```

All we did is add in an if statement that says, if the user tries to get an element within our size, return it because it exists. Otherwise, throw a new out of bound exception that tells the programmer the index they tried to add in, and the size of the list. Also, note that we had to change our method and tell Java that it throws an out of bounds exception. This is just a syntactical requirement of the language. Let's move on.

Add(i, e)
=========

Next up we will go over how to add an element at index i. Now as you may have guessed this method is also going to throw an out of bounds exception for the same reason. Before we go coding this let's think about the most efficient way to do this. First of all, we are going to have to shift all the elements at the index one right, that way we have an empty spot where we can add our element. After that we can add the element.

The most efficient way to do this would be to loop through the array structure backwards starting from the end of our list, and stopping at the index. Then we can shift each element inside the loop. Let's program this:

```java
...
public void add(int index, Type element) throws OutOfBoundsException
{
  if(index < size)
  {
    for(int i=size; i > index; i--)
    {

    }

  } else
  {
    throw new OutOfBoundsException("List index " + index + " exceeds size " + size + " of list.");
  }
}
...

```

So first we check if the element is in bounds or not. Once we know that they can add the element in we begin our loop. This loop starts at size, which is the end of our list, then it continues until i is no longer greater than index, which means we have reached our stopping point, and it decrements by one each turn of our loop. Inside our loop we would like to shift the elements right one. So we will do that as follows:

```java
public void add(int index, Type element) throws OutOfBoundsException
{
  if(index < size)
  {
    for(int i=size; i > index; i--)
    {
      array[i] = array[i - 1];
    }
    array[index] = element;
    size++;
  } else
  {
    throw new OutOfBoundsException("List index " + index + " exceeds size " + size + " of list.");
  }
}
```

So inside the loop we shift the array right one by the line seen above. We simply set the array[i], which is our current element, too array[i - 1] which is the previous element. And since we are starting one past the final index of our array, we do not have to worry about overwriting any elements. After the loop is complete we set the array[index] to the element and increment our size variable.

Add(e)
======

The add function is very similar to adding an element at an index. It's actually a little bit simpler so I will spend very short time on it. Here is the code:

```java
...
public void add(Type element)
{
  size++;
  if(size > totalSize)
  {
    array = increaseArraySize();
  }
  array[size - 1] = element;
}
...

```

So it looks just how you would expect it. Except there's one difference, we increment the size first, then if the size is greater than the total size we increaseArraySize(). All that we are saying is that if the size of the list is now bigger than the size we have allotted for our array, we will have to increase our array size. The function for increasing the array size is pretty simple. All we have to do is copy our current array into a new array that is twice as big. Here is the code:

```java
...
private Type[] increaseArraySize()
{
  totalSize *= 2;
  Type[] newArray = (Type[])new Object[totalSize];
  for(int i=0; i < array.length; i++)
  {
    newArray[i] = array[i];
  }
  return newArray;
}
...
```

All we do above is declare a new array in the same way we declared our first array. We multiply the total size by two so that the new array will be big enough. Then we loop through the <i>original</i> array and copy each element to the new array. Finally we return the new array and set the original array equal to that new array. While we're at it let's do a couple more things. Let's include this functionality to our add(i, e) method, and let's create a decreaseSize() method that decreases the arrays size by half. So, modifying the first method would look like this:

```java
...
public void add(int index, Type element) throws OutOfBoundsException
{
  size++;
  if(size > totalSize)
  {
    array = increaseArraySize();
  }
  else
  {
    if(index < size)
    {
      for(int i=size; i > index; i--)
      {
        array[i] = array[i - 1];
      }
      array[index] = element;
    } else
    {
      throw new OutOfBoundsException("List index " + index + " exceeds size " + size + " of list.");
    }
  }
}
...

```

There we go, we have simply modified our method to mimic the other add method we created. Now let's create decreaseSize().

```java
...
private Type[] decreaseSize()
{
  totalSize /= 2;
  Type[] newArray = (Type[])new Object[totalSize];
  for(int i=0; i < size; i++)
  {
    newArray[i] = array[i];
  }
  return newArray;
}
...
```

The decreaseSize() method works in almost the exact same way as our other method! The only difference is that we decrease total size by a factor of 2 and then copy the elements and then return the array. We have finally finished this method.

Remove(i)
=========

The last function we would like to add to our list ADT is a remove method. This remove method simply removes the element at index i, then shifts the rest of the elements left one. We will implement this in close to the same we we implemented add(i, e). Here is the code:

```java
...
public void remove(int index) {
  for(int i=index; i < size; i++)
  {
    array[i] = array[i + 1];
  }
  size--;

  if(size < (1 / 4) * totalSize)
  {
    array = decreaseSize();
  }
}
...

```

So, this method simply loops through the array from the index to the end of the array and sets each element on the left to the element immediately to the right of it. This overwrites the element we wish to remove. Then, we decrement the size, and shrink our array structure if it needs it.

One way we could improve this is add an exception for an empty list. What if our user tries to remove an element from an empty list? We could also add an out of bounds exception. Let's do this real quick.

```java
...
public void remove(int index) throws OutOfBoundsException, EmptyListException {
  if(index > size)
  {
    throw new OutOfBoundsException("Index " + index + " exceeds the size " + size + " of the list.");
  }
  else if(isEmpty())
  {
    throw new EmptyListException("You cannot return element " + index + " of an empty list.");
  }
  else
  {
    for(int i=index; i < size; i++)
    {
      array[i] = array[i + 1];
    }
    size--;

    if(size < (1 / 4) * totalSize)
    {
      array = decreaseSize();
    }
  }
}
...
```

Right here we have simply added in two if statements that check to make sure the index is not out of bounds, and that our list is not empty. In both cases we throw an exception. Now, I will include the code for the EmptyListException at the bottom of this article, but I would like to challenge you to create it yourself.

And that's it! We have created our list ADT. I do not have time to continue the bonus section in this article, so I will write another article that you can find [here]({% post_url 2018-01-05-list-bonus-material %}) which will take you to the bonus material. I hope you have enjoyed learning how to create a list ADT!

Code
====

List.java

```java
public class List<Type> {
  int size = 0;
  int totalSize = 1;
  Type[] array = (Type[])new Object[totalSize];

  public void remove(int index) throws OutOfBoundsException, EmptyListException {
    if(index > size)
    {
      throw new OutOfBoundsException("Index " + index + " exceeds the size " + size + " of the list.");
    }
    else if(isEmpty())
    {
      throw new EmptyListException("You cannot return element " + index + " of an empty list.");
    }
    else
    {
      for(int i=index; i < size; i++)
      {
        array[i] = array[i + 1];
      }
      size--;

      if(size < (1 / 4) * totalSize)
      {
        array = decreaseSize();
      }
    }
  }

  public void add(Type element) {
    size++;
    if(size > totalSize) {
      array = increaseArraySize();
    }
    array[size - 1] = element;
  }

  public void add(int index, Type element) throws OutOfBoundsException {
    if(index < size) {
      for(int i=size; i > index; i--) {
        array[i] = array[i - 1];
      }
      array[index] = element;
      size++;
    } else {
      throw new OutOfBoundsException("List index " + index + " exceeds size " + size + " of list.");
    }
  }

  public Type get(int index) throws OutOfBoundsException {
    if(index < size) {
      return array[index];
    } else {
      throw new OutOfBoundsException("List index " + index + " exceeds size " + size + " of list.");
    }
  }

  public Type[] increaseArraySize() {
    totalSize *= 2;
    Type[] newArray = (Type[])new Object[totalSize];
    for(int i=0; i < array.length; i++) {
      newArray[i] = array[i];
    }
    return newArray;
  }

  public Type[] decreaseSize() {
    totalSize /= 2;
    Type[] newArray = (Type[])new Object[totalSize];
    for(int i=0; i < size; i++) {
      newArray[i] = array[i];
    }
    return newArray;
  }

  public boolean isEmpty() {return size == 0;}
  public int size() { return size; }
}

```

OutOfBoundsException.java

```java
public class OutOfBoundsException extends RuntimeException {
  String str1;

  public OutOfBoundsException(String str2) {
    str1=str2;
  }

  public String toString(){
    return ("MyException Occurred: "+str1) ;
  }
}
```

EmptyListException.java

```java
public class EmptyListException extends RuntimeException {
  String str1;

  public EmptyListException(String str2) {
    str1=str2;
  }

  public String toString(){
    return ("MyException Occurred: "+str1) ;
  }
}
```
