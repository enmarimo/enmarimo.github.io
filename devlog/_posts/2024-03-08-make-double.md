---
layout: post
title: "Make it double"
---

I got collisions working, that's nice. But there's something wrong with how
the objects move. There is some jitter, which shouldn't happen.
When rendering an object its position is interpolated precisely to
prevent that. The movement should be smooth, but it isn't.
{% highlight c++ %}
	Vector2 p = (c->next_state.position * frame_interpolation) +
	            (c->current_state.position * (1.0f - frame_interpolation));
{% endhighlight %}

I did the ususal in those cases: add some prints to debug the problem. The
result was surprising:

{% highlight c++ %}
783026436161700ns DEBUG: Frame 230: 1157.088013, 37.892395
783026452065600ns DEBUG:        Frame time: 0.000000
783026452089600ns DEBUG: Frame 231: 1157.088013, 37.892395
783026468039300ns DEBUG:        Frame time: 0.062500
783026468043800ns DEBUG:        Physics iteraton 0: accumulated time 0.062500
783026468052600ns DEBUG:        Physics iteraton 1: accumulated time 0.052500
783026468057700ns DEBUG:        Physics iteraton 2: accumulated time 0.042500
783026468062200ns DEBUG:        Physics iteraton 3: accumulated time 0.032500
783026468074800ns DEBUG:        Physics iteraton 4: accumulated time 0.022500
783026468079700ns DEBUG:        Physics iteraton 5: accumulated time 0.012500
783026468109400ns DEBUG: Frame 232: 1145.492065, 35.554283
783026484050900ns DEBUG:        Frame time: 0.000000
783026484075700ns DEBUG: Frame 233: 1145.955933, 35.159363
783026499785300ns DEBUG:        Frame time: 0.000000
783026499812400ns DEBUG: Frame 234: 1145.955933, 35.159363
783026516316400ns DEBUG:        Frame time: 0.000000
783026516338900ns DEBUG: Frame 235: 1145.955933, 35.159363
783026532297400ns DEBUG:        Frame time: 0.062500
783026532302200ns DEBUG:        Physics iteraton 0: accumulated time 0.062500
783026532310400ns DEBUG:        Physics iteraton 1: accumulated time 0.052500
783026532315200ns DEBUG:        Physics iteraton 2: accumulated time 0.042500
783026532319900ns DEBUG:        Physics iteraton 3: accumulated time 0.032500
783026532324500ns DEBUG:        Physics iteraton 4: accumulated time 0.022500
783026532329000ns DEBUG:        Physics iteraton 5: accumulated time 0.012500
{% endhighlight %}

For every frame that executes properly there are 3-4 frames where nothing
happens. But why?

To obtain the frame time I get a timestamp which is cast to a float value:

{% highlight c++ %}
	_current_timestamp = static_cast<float>(GetTickCount64()) / 1000.0f;
{% endhighlight %}

And that's the problem. The float value doesn't have enough precision to
store the timestamp. Therefore any calculations done with that timestamps
are wrong.

The easiest solution is to use a double instead of a float.

{% highlight c++ %}
	_current_timestamp = static_cast<double>(GetTickCount64()) / 1000.0;
{% endhighlight %}

With this simple change now it is working smoothly as expected. We can see
a huge difference visually and also in the log.

{% highlight c++ %}
784353338324400ns DEBUG: Frame 230: 1134.647949, 212.289215
784353354139600ns DEBUG:        Frame time: 0.015000
784353354143100ns DEBUG:        Physics iteraton 0: accumulated time 0.015000
784353354165600ns DEBUG: Frame 231: 1136.317871, 210.867508
784353370332200ns DEBUG:        Frame time: 0.016000
784353370336300ns DEBUG:        Physics iteraton 0: accumulated time 0.016000
784353370363700ns DEBUG: Frame 232: 1138.358643, 209.129852
784353385930300ns DEBUG:        Frame time: 0.015000
784353385934400ns DEBUG:        Physics iteraton 0: accumulated time 0.015000
784353385958600ns DEBUG: Frame 233: 1140.028564, 207.708145
784353402397900ns DEBUG:        Frame time: 0.016000
784353402402200ns DEBUG:        Physics iteraton 0: accumulated time 0.016000
784353402427500ns DEBUG: Frame 234: 1142.069336, 205.970490
784353417959800ns DEBUG:        Frame time: 0.016000
784353417964600ns DEBUG:        Physics iteraton 0: accumulated time 0.016000
784353417999500ns DEBUG: Frame 235: 1143.924805, 204.390808
784353434582300ns DEBUG:        Frame time: 0.015000
784353434587700ns DEBUG:        Physics iteraton 0: accumulated time 0.015000
{% endhighlight %}

