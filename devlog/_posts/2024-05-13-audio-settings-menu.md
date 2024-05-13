---
layout: post
title: "Audio settings menu"
---

Update on the audio settings menu: after a bit of work the audio settings menu
is starting to take shape.

![Audio settings slider]({% link devlog/images/audio_settings_slider.gif %})

This is the idea of how things could look like: having a top slider to control
the main audio volume, and a set of sliders to control the audio of
individual elements such as music, sounds, etc.

As said in a [previous post][1], the slider goes from 0 to 100 (I also mentioned
a mute button that hasn't been added yet). But as you
can't hear, the functionality is not fully implemented yet. The next step is to
play some audio and actually modify the volume when moving the slider.

I have been testing the [XAudio2 API][2] and maybe I will make a post about it.
A kind of tutorial or overview that helps me better understand how to use it.

Besides that, there is also some work needed in the font. In this case I use the
[Jersey][3] font, but there is some problem in in the way I render it
and it doesn't look like I would expect. My guess is that there is an error in the
way I scale and render the text.

[1]: {% post_url /devlog/2024-05-08-settings-menu %}
[2]: https://learn.microsoft.com/en-us/windows/win32/xaudio2/xaudio2-apis-portal
[3]: https://github.com/scfried/soft-type-jersey
