---
layout: post
title:  "How to Code a List in Java: Bonus Material"
date:   2018-01-05 1:57:32 -0500
categories: [Data-Structures]
tags: [programming, java, lists, data-structures]
---

Intro
=====
In my previous article I went over how to program a list in Java. A list is a commonly used data structure for an unknown amount of data that will need to be used. You can find the previous article [here]({% post_url 2018-01-03-how-to-code-a-list-in-java %}). If you haven't already read it and gone through it I would highly recommend reading that article before you continue with this one. So, as mentioned in the previous article the methods we will be adding to our list ADT in this article are as follows:

<ul>
  <li>reverse(): Reverse the list's ordering</li>
  <li>extend(l): Append the list l to the current list</li>
  <li>indexOf(e): Return the index of element e and throw an empty list exception if the list is empty</li>
  <li>pop(): Returns and removes the element at the end of the list and throws an empty list exception if the list is empty</li>
  <li>pop(i): Returns and removes the element at index i and throws out of bounds exception or empty list exception</li>
  <li>print(): Prints out the array in the format [e1, e2, e3]</li>
</ul>

I will go through each one in the order presented above and then show the entire code for each of the files at the end of this article. I will include the code from the previous article as well as this article.

Reverse()
=========
In Python whenever you are working with a list instead of having to manually reverse the list you can just type list.reverse(), and Python's built in function reverses it for you! This is actually very handy and we will implement a similar method for our list ADT.

So, for this method I just went with the most straightforward method that I could think of. What is the easiest way you can think of to reverse a list? Before you continue reading I want you to think about how you would do this, and if it is more efficient then my method, implement it and include in the comments below! The way I reversed my list is I start from the beginning and loop through the current list until we reach half the size. Then, as I am looping I switch the last element minus the amount of elements we have traversed with the current element. By doing this we will swap every pair until we reach halfway through the list, and then the entire list is reversed!

The time complexity for something like this isn't the best, but it isn't the worst. The exact time complexity of our function is O(n / 2) where n is the size of our list. So it is linear in nature. There are faster ways of doing this, they are just more complex and require more knowledge than we have gone over in these articles so far. If I cover one of the quicker methods I will write an article in which I revisit this and show the faster method. I will also try to include a link here to that article if that happens.

Anyways, how would we go about coding something like this? It's actually not too difficult in comparison to all of our previous methods. Here's the code:

{% highlight java %}
...
public void reverse()
{
  for(int i=0; i < size / 2; i++)
  {
    Type tmp = array[i];
    array[i] = array[size - i - 1];
    array[size - i - 1] = tmp;
  }
}
...
{% endhighlight %}

Now, the more astute reading this article may wonder, why don't we throw an empty list exception? Well, if you have an empty list and try to loop through it from 0 to 0, what is that for loop going to do? I would recommend writing a short program that loops from 0 to 0 and prints out the index inside the loop and seeing what happens. What you will see is that nothing prints! And the reason for this is that when Java sees a loop that exits where it starts it will just skip over the loop since the exit condition has already been met. So we do not have to worry about any errors when reversing an empty list. I mean, after all, reversing an empty list will just give you an empty list.

IndexOf(e)
==========
This is another useful method that Python includes. There are many cases where you do not know the index of the element you are searching for. So instead of searching for it, Python has this function called indexOf that finds the index of the element for you! Once again, we will use a very inefficient method for finding this. The method we use would take O(n) time complexity in the worst case scenario, the element we are searching for is at the end of the list. You could implement a faster search method that takes O(log(n)) time complexity if the list was sorted, but then you would have to create a sort method and the index would be thrown off.

Once again, there are other methods that could be used but are too advanced as of now. We will revisit them later. The way I went about this is very straightforward. First I check if the list is empty, if it is, I throw an empty list exception. Then, I loop through the list and if I find the element I return the index. If I <i>don't</i> find the element, I return -1 which is an invalid list index. This way our user will know that the element was not found. Let's code this:

{% highlight java %}
...
public int indexOf(Type element) throws EmptyListException
{
  if(isEmpty())
    throw new EmptyListException("Cannot return index of element " + e + " of empty list.");

  for(int i=0; i < size; i++)
  {
    if(array[i].equals(element))
      return i;
  }

  return -1;
}
...
{% endhighlight %}

As you can see the method is very straightforward. It declares a method called indexOf that takes as an input an element of type Type, which is the abstract data type we are using for this class, and it returns an integer. This method also throws an empty list exception if the list is empty. So, first we check if the list is empty, and we say if the list is empty throw a new empty list exception that tells our user what the problem is.

Next, we loop through the data and if we find an element that equals the element we are looking for, we will return the index. Otherwise, if we complete the loop without finding it, we will return -1 which is an invalid index.

Perhaps, throwing an error would be more useful in this case, and I encourage you to implement that if you find it more useful. I was just going for convenience rather than efficiency in this case. Also, you may wonder why we use .equals() instead of the double equals sign, ==. We use .equals() because we don't know what data type we are dealing with. In Java different data types will return different values for the double equals sign compared to .equals(). For instance, if you are dealing with strings or characters you would want to use .equals(). This just ensures that our program will output correct results no matter what data type we are using.

Pop()
=====
The pop method is another useful method from, you guessed it, Python. This method simply removes and returns the element at the end of our list. This can be accomplished in a constant time very easily using some of the private variables we already have. The pop method will also throw an empty list exception, because you can't remove and return an element in an empty list. Here is the code:

{% highlight java %}
...
public Type pop() throws EmptyListException
{
  if(isEmpty())
    throw new EmptyListException("You cannot return element 0 of an empty list.");

  size--;
  if( size < (0.25 * totalSize) )
    array = decreaseSize();

  return array[size];
}
...
{% endhighlight %}

So our method works exactly how you would expect it to. First, it checks if the list is empty, if it is then it throws an empty list exception. If not it decrements the size by one. Next it checks to see if our size has decreased substantially enough to have to shrink the array. If it has than the array shrinks to half the size and we set the new array equal to that. If not we simply return array[size].

Now this may be very confusing to you because, when did we remove the element? We didn't. Well, we did. See, the thing is we can have elements in our array that will never be accessed by our list because it exceeds the size of our list. This means that we can have elements inside our array data structure that simply cannot be seen by the list because of the way we have coded it. This is useful because instead of physically removing elements, we can simply decrease the size and that element "disappears". So, that is our pop method.

Pop(i)
======
This method is a little bit more complex, but basically works exactly the same as remove(i). The only difference is that it returns as well as removes the element at index i. Here is the code:

{% highlight java %}
public Type pop(int index) throws EmptyListException, OutOfBoundsException
{
  if(isEmpty())
    throw new EmptyListException("You cannot return element " + index + " of an empty list.");

  if(index > size - 1)
      throw new OutOfBoundsException("Index " + index + " exceeds the size " + size + " of the list.");

  Type tmp = array[index];
  for(int i= index; i < size; i++)
  {
    array[i] = array[i + 1];
  }
  size--;

  if(size < (1 / 4) * totalSize)
    array = decreaseSize();

  return tmp;
}
{% endhighlight %}

This method throws an empty list exception, or an out of bounds exception. First it checks if the list is empty or out of bounds and returns an error if this is true. Next, we go through and first, store the element we are looking for in a temporary variable. Then, we remove that variable from the list by overwriting it the same way we did in remove(i). Lastly, we return the temporary variable after checking if we need to decrease the array size. So, that's popping an element from an index. It works almost the same as remove.

Print()
=======
The last method we will go over is how to print the list in the format [e1, e2, e3]. This is very helpful so that we can see what our list looks like and debug it accordingly. We will simply loop through the list and build a string that prints it out in the correct fashion, then print that string out to the console. Let's see how this looks:

{% highlight java %}
...
public void print()
{
  String print = "";

  if(isEmpty())
  {
    print = "[]";
  } else
  {
    print = "[";
    for(int i=0; i < size - 1; i++)
    {
      print += "'" + array[i] + "'" + ", ";
    }
    print += "'" + array[size - 1] + "'" + "]";
  }
  System.out.println(print);
}
...
{% endhighlight %}

As you can see in the method above. We begin by decalring a string called print. Then we check if the list is empty, if it is we simply set print equal to empty brackets. If it is not we build the string accordingly. We loop through from 0 to size - 1 because if it is the end of the list, we want to close the brackets instead of adding another comma. This creates the list in the correct format that we would like. It also adds single quotes around each element in the list. And there we have it!

Conclusion
==========
Below I will add a main class that you can use to test all of your code. I hope you have enjoyed these articles and it has helped you to create a nice functioning list!

Some methods you may want to add to this include stuff like sort(), which sorts the list accordingly, or remove(e) which removes a specific element, and anything else you can think of! I will include the code below, if you have any questions or comments please post them below!

Code
====
Main.java
{% highlight java %}
public class Main {
  public static void main(String[] args) {
    List<Integer> numbers = new List<Integer>();

    for(int i=0; i < 100; i++) {
      numbers.add(i);
    }
    numbers.reverse();

    numbers.get(101);
    numbers.print();
  }
}
{% endhighlight %}

List.java
{% highlight java %}
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

  public void reverse() {
    for(int i=0; i < size / 2; i++) {
      Type tmp = array[i];
      array[i] = array[size - i - 1];
      array[size - i - 1] = tmp;
    }
  }

  public void extend(List<Type> otherList) {
    for(int i=0; i < otherList.size(); i++) {
      this.add(otherList.get(i));
    }
  }

  public int indexOf(Type element) throws EmptyListException {
    if(isEmpty())
      throw new EmptyListException("Empty list.");

    for(int i=0; i < size; i++) {
      if(array[i].equals(element))
        return i;
    }

    return -1;
  }

  public Type pop() throws EmptyListException {
    if(isEmpty()) {
      throw new EmptyListException("You cannot return element 0 of an empty list.");
    } else {
      size--;
      if( size < (0.25 * totalSize) ) {
        array = decreaseSize();
      }
      return array[size];
    }
  }

  public Type pop(int index) throws EmptyListException, OutOfBoundsException {
    if(isEmpty()) {
      throw new EmptyListException("You cannot return element " + index + " of an empty list.");
    } else if(index > size) {
        throw new OutOfBoundsException("Index " + index + " exceeds the size " + size + " of the list.");
    } else {
      Type tmp = array[index];
      for(int i= index; i < size; i++) {
        array[i] = array[i + 1];
      }
      size--;
      if(size < (1 / 4) * totalSize) {
        array = decreaseSize();
      }
      return tmp;
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

  public void print() {
    String print = "";

    if(isEmpty()) {
      print = "[]";
    } else {
      print = "[";
      for(int i=0; i < size - 1; i++) {
        print += "'" + array[i] + "'" + ", ";
      }
      print += "'" + array[size - 1] + "'" + "]";
    }
    System.out.println(print);
  }

  public boolean isEmpty() {return size == 0;}
  public int size() { return size; }
}
{% endhighlight %}
OutOfBoundsException.java
{% highlight java %}
public class OutOfBoundsException extends RuntimeException {
  String str1;

  public OutOfBoundsException(String str2) {
    str1=str2;
  }

  public String toString(){
    return ("MyException Occurred: "+str1) ;
  }
}
{% endhighlight %}
EmptyListException.java
{% highlight java %}
public class EmptyListException extends RuntimeException {
  String str1;

  public EmptyListException(String str2) {
    str1=str2;
  }

  public String toString(){
    return ("MyException Occurred: "+str1) ;
  }
}
{% endhighlight %}
