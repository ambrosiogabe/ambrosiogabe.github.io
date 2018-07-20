---
layout: post
title:  "How to Code Insertion Sort in Java"
date:   2017-12-20 1:57:32 -0500
categories: [Data-Structures]
tags: [programming, java, sort]
cover: /assets/images/insertionSort.png
comments: true
subtitle: I haven't written in awhile. I decided that I'm going to continue my tutorials on data structures, so next up is...
---
I haven't written in awhile. I decided that I'm going to continue my tutorials on data structures, so next up is learning how to code an insertion-sort algorithm in Java.
I have a graphic pictured below that shows how this insertion-sort algorithm works.

![insertionSortPicture](/assets/images/insertionSort.png)

What you see pictured above is the execution of the insertion-sort algorithm. This algorithm basically loops through a list of items, it could be any comparable data type.
As it is looping through it pulls out the current item and compares it to the next item in the array. If it is more, or less depending on if it is a maximum sort or minimum sort,
it moves the element back one. It continues to do this comparison until it reaches the end, or finds an element that is less than the element held in current. Once it hits that point
it inserts the current element into that slot, then it moves onto the next element. So it basically sorts a list of data the same way that a human would sort it. The human may be slightly
more efficient by seeing patterns, but we will explore ways to do that in future tutorials.

So, in order to build this algorithm we will need two basic moves. Swap, and insert. In pseudocode it looks like this:

{% highlight python %}
Algorithm InsertionSort(A):
  Input: An array A, of n elements
  Output: The array A ordered in increasing order
  for i from 1 to n - 1
    insert A[i] in its proper place
  return A
{% endhighlight %}

Now that we have a basic layout, let's start to put the actual code together. First off We will build a class in Java called InsertionSort.java. This class will contain two functions,
sortMin(int[] array), and sortMax(int[] array). The sortMin returns the array in decreasing order, and sortMax returns the array in increasing order. We will add in two other functions
strictly for testing purposes. We will add in a main function to run the program, and a printArray(int[] array) function which prints the array in the fashion, [A1, A2, A3...An], where
it lists the elements from A1 to An where n is the number of elements.

{% highlight java %}
public class InsertionSort
{
  public static void main(String[] args)
  {

  }

  public static int[] sortMin(int[] array)
  {
    return array;
  }

  public static int[] sortMax(int[] array)
  {
    return array;
  }

  public static void printArray(int[] array)
  {
    return;
  }
}
{% endhighlight %}

Now that we have each of our functions in place, let's fill them in. Since printing the array will be the simplest part of this program I'll start with that. To print the array we will
code this:

{% highlight java %}
public static void printArray(int[] array)
{
  String printString = "";
  printString += "[";
  for(int i=0; i < array.length - 1; i++)
  {
    if(i != array.length - 1)
      printString += array[i] + ", ";
  }
  printString += array[array.length - 1] + "]";
  System.out.println(printString);
}
{% endhighlight %}

Now this code is pretty self-explanatory. It simply loops through the list and prints out each element in the array. It starts by building a string starting with a left bracket, [, then adds each element of the array until it reaches the end where it then places the right bracket, ]. Next we will begin coding the sort algorithm. We will start with sortMax. Here is the code:

{% highlight java %}
public static int[] sortMax(int[] array)
{
  for(int i=0; i < array.length; i++)
  {
    int currentInteger = array[i];
    int j = i;
    while(j > 0 && array[j - 1] > currentInteger)
    {
      array[j] = array[j - 1];
      j--;
    }
    array[j] = currentInteger;
  }
  return array;
}
{% endhighlight %}
This function first loops through the array. It then stores the current variable as currentInteger. Then it sets up the inner loop by setting a new integer j, equal to i. J is set to i because i is the current index of the integer we are on. Then we begin to loop through the array, starting from the number we are at, backwards. We check if j is greater than 0, it is not the end of the array, and we check if the previous integer is greater than the current integer. If it is, we move the previous integer to the slot we are at. We continue this until we reach the beginning of the data structure. After it moves all the appropriate elements, it inserts the current element in the empty slot then continues with the sorting until the end of the algorithm.

Now if you know some about data structures you know there is much more efficient ways to do this which we will get to in due time. As a challenge, I would like you to code sortMin by yourself. It is the exact same premise as the code above, and you actually only need to change one symbol for it to work. Here is the complete code, minus sortMin, so you can run it and test it.

{% highlight java %}
public class InsertionSort
{
  public static void main(String[] args)
  {
    int[] array = new int[50];
    for(int i=0; i < array.length; i++)
    {
      int rand = (int)(Math.random() * 100);
      array[i] = rand;
    }

    printArray(array);
    array = sortMax(array);
    printArray(array);
  }

  public static int[] sortMax(int[] array)
  {
    for(int i=0; i < array.length; i++)
    {
      int currentInteger = array[i];
      int j = i;
      while(j > 0 && array[j - 1] > currentInteger)
      {
        array[j] = array[j - 1];
        j--;
      }
      array[j] = currentInteger;
    }
    return array;
  }

  public static void printArray(int[] array)
  {
    String printString = "";
    printString += "[";
    for(int i=0; i < array.length - 1; i++)
    {
      if(i != array.length - 1)
        printString += array[i] + ", ";
    }
    printString += array[array.length - 1] + "]";
    System.out.println(printString);
  }
}
{% endhighlight %}

You will see that in the code I have filled in the main method. All I've done is set it up to create a random array of length 50 with radnom integers ranging from 0 to 99, then it prints the array, sorts it, then prints it again. This is for testing purposes so you can see that it works. I hope this helped some, thanks for reading! If you have questions or comments leave them below!
