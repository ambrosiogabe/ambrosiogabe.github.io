---
layout: post
title:  "How to Program a Deque in Java"
date:   2017-09-18 16:37:45 -0500
categories: [Data-Structures]
tags: [programming, java, deque, array]
cover: /assets/images/deque.png
comments: true
subtitle: Deques are another important data structure. They are like queues, except they are double ended which enhances them in certain ways... Read more
---

Deques are another important data structure. They are like queues, except that they are double ended. What this means is that you can add an element to the front of the deque, or to the end of the deque. It also means you can remove an element from the front of a deque or the end of a deque. Now, this is very efficient, it is O(1). The only hindrance is that it is limited on the amount of space it can take up. Since a deque is also constructed using an array as the base it has a fixed size. This means your deques cannot grow or shrink. Posted below is the code to a fully functional deque in Java. I will briefly explain key sections for further understanding.

{% highlight java %}
public class Deque<Type> {

  int size, front, end;
  int totalSize = 10000;
  Type[] array = (Type[])new Object[totalSize];

  public Deque() {
    size = 0;
    front = totalSize / 2;
    end = front + size - 1;
  }

  public void addFirst(Type e) {
    front--;
    size++;
    array[front % totalSize] = e;
  }

  public void addLast(Type e) {
    end++;
    array[end % totalSize] = e;
    size++;
  }

  public Type removeFirst() {
    if(!isEmpty()) {
      front++;
      size--;
      return array[front - 1];
    } else {
      return null;
    }
  }

  public Type removeLast() {
    if(!isEmpty()) {
      size--;
      end--;
      return array[end + 1];
    } else {
      return null;
    }
  }

  public void print() {
    String print = "[";

    if(!isEmpty()) {
      for(int i=front; i < end + 1; i++) {
        if(i != end)
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
{% endhighlight %}

The way I construct my deque is by implementing an array of a vast size so that the deque does not run out of space any time soon. Then I have the deque begin in the middle of this array. You need three variables to keep track of where everything is, the front, the end, and the size. Using these three variables it is very simple to add or remove elements from the beginning or the end of the deque.

The addFirst method simply decrements the front by one, increments the size by one, then adds the new element to the new front. Simple. Likewise, the addLast method simply increments the end by one, increments the size by one, then adds the new element to the new tail.

To remove elements it is just slightly more tricky. To remove first the code simply decrements size by one, increments the front by one, then returns the array's front - 1. The reason for subtracting one is because the new front is now the next element above where the original front was, hence "removing" the front. So we have to look one behind the new front to see what the old front was. Removing last is very similar. Just decrement the size by one, decrements the end by one, and then return the array's end + 1.

I hope this helps you on your journey to understanding deque's. Questions or comments, leave them below and I'll get back to them shortly!
