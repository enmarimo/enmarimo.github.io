---
layout: post
title: "Audio perception is not linear"
---
Finally I implemented a working slider, this this with sound. But it has been
a bit more complex than expected.

## Linear slider
At first I made the slider linear. Which means that with a volume of 0 there is
silence, with 100 there is no attenuation and with 50 the signal is attenuated
by half. At this is the result:

<video width="738" height="160" controls>
  <source src="/devlog/images/audio_slider_linear.mp4" type="video/mp4">
</video>

It works, but there is small problem. Human loudness perception is not
linear[^1]. For example, with the linear slider above the percepcion is that
between 100 and 50 there isn't much change in the audio volume (while in fact
it is reduced by half), but between 50 and 0 there is much more difference. We
could say the audio decreases much more faster, which can make it more
diffult to control.

To address that we can use an exponential function.

## Exponential slider
Human perception of sound is logarithmic. To offer a better user experience we
can use an exponential function on the slider so, as a result, the percieved
changes in volume will feel more linear.
I don't want to get into much details as this post was only meant to
showcase the slider, so I leave this article from Alexander Thomas[^2] which
offers more information.

At the end we have to implement a function like this to map the values from the
\[0, 100\] range to \[0, 1\] using an exponential function.

{% highlight c++ %}
// We want volume_exp(0) = 0; As exp(0) equals 1, we have to subtract 1
// in the numerator.
// Then volume_exp(100) = 1; Since exp(1) is e, denominator is (e-1)
float volume_exp = (std::exp(volume/100) - 1) / (std::numbers::e - 1);
{% endhighlight %}

And this is the result:

<video width="738" height="160" controls>
  <source src="/devlog/images/audio_slider_exp.mp4" type="video/mp4">
</video>

## Conclusions
This post presented different ways to implement the volume slider. I think the
difference between the two can be noticeable: when sliding the exponential
slider the volume seems to change more progressively. Overall, I don't feel
there is a huge difference.

After all, this is an initial test. Now we could try different configurations,
like applying scaling factors or using different exponent bases, to find out a
more pleasing solution.


## References
[^1]: [https://en.wikipedia.org/wiki/Loudness][1]
[^2]: Thomas, A. *Programming Volume Controls*. [https://www.dr-lex.be/info-stuff/volumecontrols.html][2]

[1]: https://en.wikipedia.org/wiki/Loudness
[2]: https://www.dr-lex.be/info-stuff/volumecontrols.html
[3]: https://electronics.stackexchange.com/questions/101191/why-should-i-use-a-logarithmic-pot-for-audio-applications
[4]: https://stackoverflow.com/questions/1165026/what-algorithms-could-i-use-for-audio-volume-level
