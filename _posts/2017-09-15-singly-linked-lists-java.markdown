---
layout: post
title:  "How to Program a Singly Linked List-Java"
date:   2017-09-15 8:22:32 -0500
categories: [Data-Structures]
tags: [programming, java, singly, lists]
cover: /public/images/articleHeaders/codeSinglyLinkedListJava.png
---

Perhaps you have seen lists in Python, or array lists in Java. A singly linked list is similar, but different in that you can use it to add elements indefinitely without worrying about size restriction, but it does not have a way to reach the element by index.

Before we can start coding it is important to understand what exactly a singly linked list is, look at the picture above. A singly linked list is a data structure that starts with a head, has an unknown amount of elements with pointers to the next element, and the last element points to null.

In order to start building this we are going to need two different data structures. First, the node, which is each individual element in the list. Secondly, the linked list, which is the structure that contains all the nodes. Let's start with the node structure. So, here is a class that builds a node in Java. It will need two things, an element, and a pointer to the next node.

```java
public class Node<Type> {
  Type element;
  Node<Type> pointer;

  public Node(Type e, Node<Type> reference) {
    element = e;
    pointer = reference;
  }

  public Type getElement() {return element;}
  public void setNext(Node<Type> next) {pointer = next;}
  public Node<Type> getNext() {return pointer;}
}
```

So in this code we start off defining our class as a general class that accepts any type of element. Then we specify that when a node is created, we want to pass it the element, and the pointer to the next node. We then set a few basic functions to retrieve different parts of the node. We have a getElement() function which returns the element, a setNext() function which sets the pointer, and a getNext() function which returns the next node.

This should all be saved in a file called <b>Node.java</b>

Next is the linked list structure.

```java
public class SinglyLinkedList<Type> {
  public Node<Type> head;
  Node<Type> tail;
  int size;

  public SinglyLinkedList() {
    size = 0;
  }

  public Node<Type> getHead() {return (isEmpty()) ? null : head;}
  public Boolean isEmpty() { return size == 0; }
  public int size() {return size;}
}

```

So we start off with a few basic functions. First we add in a instantiating function that instantiates a new linked list. We pass this nothing and set the list's size equal to 0 because we will add functions that allow you to add a new element to the front or the end of the list. Then we have a few basic functions, getHead returns the head node, isEmpty returns whether the size is equal to 0 or not, and size simply returns the size variable.

Next we will add a function that adds an element to the head of the list.

```java
  public void addFirst(Type element) {
    Node<Type> newFirst = new Node<Type>(element, head);

    if(isEmpty())
      tail = newFirst;

    head = newFirst
    size++;
}
```

This function takes an element as its input and returns nothing. It first creates a newNode and sets that equal to the element, and has it point to the current head. Then we assign the head node to this new node. In essence, making this new node the new head so that it adds an element to the list. If the list is empty though, we also assign the tail to this new node. The tail is assigned to newFirst because the head is the tail, which means that the list is of size one. Lastly we increment the size by one.

Right now would be a good time to add a function that prints out our list so that we can see what it looks like. So, we will create a print function now. Here is the code below.

```java
public void print() {
  String linkedList = "[";
  Node<Type> currentNode = head;

  if(!isEmpty()) {
    do {
      linkedList += currentNode.getElement().toString();

      if(currentNode.getNext() != null)
        linkedList += ", ";
      else
        linkedList += "]";

      currentNode = currentNode.getNext();
    } while(currentNode != null);
  } else {
    linkedList = "[]";
  }
  System.out.println(linkedList);
}

```

I personally like how Python prints out the output of a list. It looks like "[element1, element2 ... lastElement]". So this is the way I've decided to display our linked list in Java. In order to do this I declared a variable linkedList, and this is what will store our list in the format mentioned above. We set its value to "[", which is the first character in the string. Next we define a node called current node so that we can loop through our list. Now we can start looping.

First the code checks to see if the list is empty, if it is it closes the brackets and returns an empty list. Otherwise, it begins the loop. I chose a do while loop for this so that we can always execute the code and then end it if the node is null which represents the end of the list. Next we add the current element to our string value. Then we check to see if the next element is the null element. If it is, then we know we can close off the bracket because it is the end of the list, otherwise it adds a comma and a space to our string to continue adding elements to the list. Finally, after it finishes the loop we print out the completed string.

Now we can implement a Main class to see if our code is working. All the code above should be saved into SinglyLinkedList.java. And now we will create a new class called Main.java. This will simply execute our linked list to see if it works.

```java
public class Main {
  public static void main(String[] args) {
    SinglyLinkedList<Character> list = new SinglyLinkedList<Character>();
    String string = "I am a string";
    for(int i=0; i < string.length(); i++) {
      list.addFirst(string.charAt(i));
    }

    list.print();
  }
}
```

The main function simply creates a new list of characters, then adds the individual characters to our list, then prints it out. Next we can add an add last function, that adds an element to the end of our list. Inside our SinglyLinkedList.java file we will add the function below.

```java
public void addLast(Type element) {
  Node<Type> newest = new Node<Type>(element, null);

  if(isEmpty()) {
    head = newest;
  } else {
    tail.setNext(newest);
  }
  tail = newest;
  size++;
}

```

This function once again creates a new node, this time though, it takes an element as its input, then creates the node and has it pointing to null since it will be at the end of our list. After this, we simply check if the list is empty, and if it is we make the head the new node. We do this because if we have a list of one element, the head will simply point to null and that is the end of the story.

Next we say, if there are elements in there, we first have the tail point to the new node, then we assign the tail to the newest node and increment the size. Then we have an element added to the end of the list!

There are more functions we could add to this list structure, such as removing elements, adding elements in the middle of the list, reversing, copying, etc. I wish I could discuss all of them, but I will leave it at this, and if something else is of utmost importance to cover leave a comment below and I will try to get around to it as soon as possible!
