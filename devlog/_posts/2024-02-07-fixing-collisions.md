---
layout: post
title: "Fixing 2D collisions"
---

# Fixing 2D collisions
Collisions are one of the most basic things to implement in a game. I'm
currently working on the implementation of a 2D collision system using
ray casts, but it's not working properly: sometimes the object goes through the
object it should be colliding with.

![Collision going through](/devlog/images/collision_going_through.gif)

This problem seems to happen when the corner of an object collides with the
corner of another object. My understanding of what is happening is shown in the
image below:

![Collision failing diagram](/devlog/images/collision_failing_diagram.png)

First, the horizontal collisions are checked and none is detected. Then the
vertical collisions are checked and none is detected. The objects move and, on the
next iteration, they overlap and that messes the collision detection.

## Proposed solution
One way to avoid this is to move the object in two steps, first check the
horizontal collisions and move horizontally and then check the vertical
collisions and move vertically. By doing things like this we can avoid the
previous situation where collisions are not properly detected.

![Collision split in two steps diagram](/devlog/images/collision_split_diagram.png)

Looking at the code, what I have is something like this:
{% highlight c++ %}
if(velocity.x != 0.0f) {
	checkCollisionsHorizontal();
}

if(velocity.y != 0.0f) {
	checkCollisionsVertical();
}

updatePosition();
{% endhighlight %}

Then, applying the proposed fix it would be rewritten as:
{% highlight c++ %}
if(velocity.x != 0.0f) {
	checkCollisionsHorizontal();
	updatePositionX();
}

if(velocity.y != 0.0f) {
	checkCollisionsVertical();
	updatePositionY();
}
{% endhighlight %}
