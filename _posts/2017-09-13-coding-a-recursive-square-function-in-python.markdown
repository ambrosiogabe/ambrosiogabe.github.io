---
layout: post
title:  "How to Build a Recursive Factorial Function in Python/Java"
date:   2017-09-13 17:20:32 -0500
categories: [Programming]
tags: [programming, python, java, recursion]
cover: /assets/images/python.png
comments: true
subtitle: Have you ever wondered how to implement a recursive function that does th factorial of a number for you? Well maybe you haven't...
---
Have you ever wondered how to implement a recursive function that does th factorial of a number for you? Well maybe you haven't, and maybe you just need to crank something out for class. Anyway you came here looking for some information on how to do it, so I'll show you how it's done and try and give you some insight on what's happening behind the scenes.

<u>Python 3</u>
---------------
So in Python 3 you would first want to declare two functions, your main function, and your factorial function like this.

{% highlight python %}
def factorial(n):
  return None

def main():
  return None

main()
{% endhighlight %}

This sets up a space for you to do the real coding, when your code runs it will reach the end of the line and hit the `main()` statement which executes the `main()` function and starts your code running. Currently we have both return None so that your code does not provide any errors.

{% highlight python %}
def factorial(n):
  if n == 0:
    return 1
  else:
   return #To be assigned a value

def main():
  print(factorial(4))
  return None

main()
{% endhighlight %}

Ok, so now there are a couple of more lines added. The line in our main function merely checks to see if our factorial function is working, and if it is, it should print out 24. We shall see if it works! Next, inside our factorial function, we have added an if statement. Every recursive function must have an if statement if you ever want to escape the recursion! Otherwise you will be stuck in an infinite loop and crash your program. The base statement for a factorial is when the factorial is 0!, which always equals 1. So we tell our program if the factorial is 0, then return 1. Now for the last part...

{% highlight python %}
def factorial(n):
  if n == 0:
    return 1
  else:
   return n * factorial(n - 1)

def main():
  print(factorial(4))
  return None

main()
{% endhighlight %}

This last bit of code simply starts the recursion. Our program now checks if the input is greater than 0, if it is, then it tells itself to return the result of n times the factorial of the next smaller number. This will work it's way down to 0, then reverse and return 1, then 1 * 2, then 1 * 2 * 3... until it reaches n. So there you have it! I will show the same function in Java right below this.

<u>Java</u>
-----------

{% highlight java %}
public class Factorial {
  public static void main(String[] args) {
    System.out.println(factorial(3));
  }

  public int factorial(int n) {
    return n = (n == 0) ? return 1 : return n * facorial(n - 1);
  }
}

{% endhighlight %}
