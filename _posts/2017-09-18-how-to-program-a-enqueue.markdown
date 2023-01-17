---
layout: post
title:  "How to Program a Queue in Java"
date:   2017-09-18 16:37:32 -0500
categories: [Data-Structures]
tags: [programming, java, queue, array]
cover: /public/assets/images/queue.png
---
Queues are very similar to deques. The only difference is they follow the First in First Out (FIFO) principle. This means that they add the newest elements to the back of the list, and the oldest elements get returned first. It's kind of like waiting in line at a fast food restaurant. If you pull in and their is a long line, you are the newest person there, but you are added to the end of the line (or list if we're thinking queues). The person at the front of the line has been waiting the longest, and they get their food first. Once they get their food they are "dequeued" and the next person gets to be at the front. If someone comes in behind you, they are "enqueued" and are added to the back of the list.

So here is the implementation of the queue in Java.

```java
public class Queue<Type> {
  int size;
  int front;
  Type[] array = (Type[])new Object[10000];

  public Queue() {
    size = 0;
    front = 0;
  }

  public void enqueue(Type e) {
    array[size] = e;
    size++;
  }

  public Type dequeue() {
    if(!isEmpty()) {
      Type tmp = array[front];
      array[front] = null;
      size--;
      front++;
      return tmp;
    } else {
      return null;
    }
  }

  public void print() {
    String print = "[";

    if(!isEmpty()) {
      for(int i=front; i < size + front; i++) {
        if(i != size + front - 1)
          print += array[i] + ", ";
        else
          print += array[i] + "]";
      }
    } else {
      print += "]";
    }

    System.out.println(print);
  }

  public boolean isEmpty() {return size == 0;}
  public int size() {return size;}
  public Type first() {return array[front];}
}

```

The coding technique is very similar to that of my tutorial on deques. The only difference is for queues, we have to keep track of three variables, the front, the size, and the array. The front keeps track of where the front of the queue is, the size keeps track of the size, and the array acts as our underlying data structure. The queue just has two main functions, enqueue and dequeue. The other functions are arbitrary when it comes to functionality and implementation.

I will discuss the two main functions in some detail to explain how they work. So, the way the enqueue function works is it appends the element that the program wishes to enqueue to the end of the queue. It does this by saying that the array at index size is equal to the element. Now, you may wonder why we don't use size - 1, which would be the last index in the array. The reason is because we're adding an element. So, by adding it to size, we are actually adding it to the slot right after the end. Then we increment the size. Pretty easy.

To dequeue an element is slightly more difficult, but it is still fairly simple. To dequeue, first we check if the queue is empty, if it is we return null. If it is not, then we proceed with the dequeue process. First we set a temporary variable equal to what is at the front of the array. Then we set the front of the array to null. Next we decrement the size, increment the front, then return our temporary variable. The reason for incrementing the front is because technically we aren't decreasing the size of the <i>array</i>, we're actually decreasing the size of the queue. So, by incrementing the front, it allows us to keep track of where the front of our queue resides inside the array.

Well that just about covers it. The astute programmer will note that my implementation for queues and deques has some potential problems. Such as, what if the queue has empty space at the front, but no space at the back? Our program will return an error even though we could technically add more elements to the front by wrapping around. This is true, and I can address that issue in another article if so needed. Otherwise, I hope you enjoyed the article. Please comment below if there are any other questions or errors with my code. Thanks!
