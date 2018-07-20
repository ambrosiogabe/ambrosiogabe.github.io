---
layout: post
title:  "Install My First Box Collision Test"
date:   2017-10-04 17:57:32 -0500
categories: [Games]
tags: [programming, java, game]
cover: /assets/boxCollision/gameplay.png
comments: true
subtitle: Here's my first attempt at box collision in Java. I was inspired to create a simple 2D platformer with some very simple physics and this is...
---

Download
========
To download simply right click the link below and save it onto you computer. Make sure you have Java installed, and then click the downloaded file to play!

[Minigame.jar (4.8kB)][1]


How it Works
============
Here's my first attempt at box collision in Java. I was inspired to create a simple 2D platformer with some very simple physics and this is what came of it. I hope you enjoy it. While you're looking at this too, I may as well explain how this game came about.

Now this game is very simple, it's just a one level scene where you get the coin then touch the door and win. It could have potentially grown into something much more complex, but I never got to that point (although I am working on something much better, keep looking it should be here soon). The way I do my box collision detection is actually using some vector calculus. Now the concepts are somewhat complicated, and I mostly used algorithms already available, although the concept is quite simple. What the program does is it first draws a vector from the character to the nearest "collider" as shown below.

![box-collision-vectors](/assets/boxCollision/vectors1.png)

And the way I coded my game, it actually draws a vector between each and every object and the player, I did not draw it for simplicities sake. But if you can imagine a vector drawn between each and every object and the character. What the code then does is it calculates the vectors for the characters every move. Once one of the vectors is length negative, or less than zero, i.e the character is colliding, it "pushes" the character back so that it is no longer inside whatever object it has collided with. This all happens so fast that it gives the illusion of the character being "blocked" by the "solid" objects. All of this is of course, just manipulation of numbers.

The Code
========
Here's the code that actually runs it. It looks confusing and everything, but it's just some vector calculus that keeps our character from falling through solid objects.

```java
public class MyRectangle {
	public int width;
	public int height;
	public int x, y, dx, dy;

	public MyRectangle(int setWidth, int setHeight, int setx, int sety) {
		width = setWidth;
		height = setHeight;
		x = setx;
		y = sety;
	}

	private int centerx() {
		int centerx = this.x + (this.width / 2);
		return centerx;
	}

	private int centery() {
		int centery = this.y + (this.height / 2);
		return centery;
	}

	private int halfWidth() {
		int halfWidth = width / 2;
		return halfWidth;
	}

	private int halfHeight() {
		int halfHeight = height / 2;
		return halfHeight;
	}

	public int blockRectangle(MyRectangle r1, MyRectangle r2) {
		//For collision side 1 = 'top'   2 = 'bottom'   3 = 'left'   4 = 'right'
		int collisionSide, overlapX, overlapY;

		int vx = r1.centerx() - r2.centerx();
		int vy = r1.centery() - r2.centery();

		int combinedHalfWidths = r1.halfWidth() + r2.halfWidth();
		int combinedHalfHeights = r1.halfHeight() + r2.halfHeight();

		if(Math.abs(vx) < combinedHalfWidths) {
			if(Math.abs(vy) < combinedHalfHeights) {
				overlapX = combinedHalfWidths - Math.abs(vx);
				overlapY = combinedHalfHeights - Math.abs(vy);

				if(overlapX >= overlapY) {
					if(vy > 0) {
						collisionSide = 1;
						r1.y += overlapY;
					} else {
						collisionSide = 2;
						r1.y -= overlapY;
					}
				} else {
					if(vx > 0) {
						collisionSide = 3;
						r1.x += overlapX;
					} else {
						collisionSide = 4;
						r1.x -= overlapX;
					}
				}
			} else {
				collisionSide = 0;
			}
		} else {
			collisionSide = 0;
		}

		return collisionSide;
	}

}
```

You can use this code as some simple box collision detection, just use two objects of type MyRectangle, and the function blockRectangle, and it will ensure that the two objects do not collide. Hope you enjoy! Questions or comment? Leave them below and I will get to them as soon as possible!

[1]: {{site.url}}/downloads/boxCollision/Minigame.jar
