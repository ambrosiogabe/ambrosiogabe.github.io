---
layout: post
title:  "How to Program a Stack in Java"
date:   2017-09-18 16:32:32 -0500
categories: [Data-Structures]
tags: [programming, java, stack, array]
cover: /public/assets/images/stack.jpg
---

Stacks are a fundamental structure that we use everyday. Have you ever pressed the undo button? How about this website your reading? These and more all use stacks to help build their structures. The stacks are used to help parse code, or just hold the most recent value. In this post I would like to go over what a stack is and how to code it in Java. I am going to implement it using an array based stack with a fixed size. I may add in a linked list based stack in the future.

So, in order to program our stack we will need three main variables. The size, total size, and an array. The size is how many elements our stack currently holds. Total size is the maximum amount of elements we want our stack to be able to hold. And the array will be our primary structure for a stack. Furthermore, when we initialize a stack we will simply create a stack with size 0. Here is the code for the beginning:

```java
public class Stack<Type> {
  int size;
  int totalSize = 100;
  Type[] array = (Type[])new Object[totalSize];

  public Stack() {
    size = 0;
  }
}

```

You may notice that initializing the array is a little weird. The reason for this is because we are creating a generic array, so Java does not like that. To get around Java yelling at you for using a generic array we typecast the array and then create a new array of type Object. This would not be good if you were going to return this array.

IMPORTANT: Do not use this method to create a generic array if you are using it for anything other than a data structure.

Our stack is missing just a few crucial functions that will make it work. We will add a print function, to see if it works, a push function, which pushes a new element to the top of the stack, a pop function, which pops the top element and returns it, and size, top, and isEmpty. Let's get going.

The easy functions are size top and isEmpty. Here is the code:

```java
public class Stack<Type> {
  ...

  public Type top() {return array[size - 1];}
  public boolean isEmpty() {return size == 0;}
  public int size() {return size;}
}
```

The top method returns the top element without removing it, the is empty method returns whether size is 0 or not. Lastly, the size method simply returns our size variable. With these out of the way we can work on the more complicated functions.

First let's work on pushing an element onto our stack. If we want to push an element onto our stack we want that element to appear on the "top" of the stack. So we will add the new element to the stacks size - 1 index. This would indicate the stop. Before we do that though, we want to check whether the size is greater than the total size, if it is we will print to the console that we cannot add any more elements.

```java
...
public void push(Type e) {
  if(size <= totalSize) {
    array[size] = e;
    size++;
  }
  else
    System.out.println("Stack is full!");
}
...

```

It's as simple as that. Next let's add the pop function. We want this function to retrieve the element on top, and "remove" the top element from our stack. To do this we simply decrement the size by one and return the element at index size. Note we don't use size - 1 because the size has just decreased by one, this will ensure we return the correct element. The top element of our stack never truly disappears, it is still in the array, but our size variable ensures that we cannot access that element directly, hence "removing" the element from our stack. We can implement it like so:

```java
...
public Type pop() {
  if(!isEmpty()) {
    size--;
    return array[size];
  } else {
    return null;
  }
}
...
```

We also check whether the stack is empty or not before returning anything. If it is empty we return null. Lastly, let's add a print function. The print function will simply loop through the array and add each element to a string until it reaches the full size of the stack. Then it prints out our formatted string. It looks like this:

```java
...
public void print() {
  String print = "[";

  if(!isEmpty()) {
    for(int i=0; i < size; i++) {
      if(i != size - 1)
        print += array[i] + ", ";
      else
        print += array[i] + "]";
    }
  } else {
    print = "[]";
  }

  System.out.println(print);
}
...

```

So there is a basic implementation of a stack using an array as the base structure. I hope that helps I have the whole code listed below. Have fun implementing your stacks!

Here is the final code for our stack:

```java
public class Stack<Type> {
  int size;
  int totalSize = 100;
  Type[] array = (Type[])new Object[totalSize];

  public Stack() {
    size = 0;
  }

  public void push(Type e) {
    array[size] = e;
    size++;
  }

  public Type pop() {
    if(!isEmpty()) {
      size--;
      return array[size];
    } else {
      return null;
    }
  }

  public void print() {
    String print = "[";

    if(!isEmpty()) {
      for(int i=0; i < size; i++) {
        if(i != size - 1)
          print += array[i] + ", ";
        else
          print += array[i] + "]";
      }
    } else {
      print = "[]";
    }

    System.out.println(print);
  }

  public Type top() {return array[size - 1];}
  public boolean isEmpty() {return size == 0;}
  public int size() {return size;}
}
```
