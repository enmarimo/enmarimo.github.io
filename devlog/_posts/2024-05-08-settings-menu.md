---
layout: post
title: "Settings menu"
---

It's been a while since the last post. ~~In fact, I had some drafts in a local
branch but I reinstalled the OS and it seems I deleted them by mistake. It's
not much of a problem as if they had been important things I would have
published them, but it still annoys me~~ Nevermind, found them. Anyway, let's
continue with this post.

Today I will discuss about the settings menu. Keep in mind this is not a
tutorial, it's just a way to express myself and organize my ideas.

Most games have a set of settings for controls, graphics, audio and so on. I
want to create a settings/options menu for my game but, which are the expected
settings and how to present them in a nice and intuitive way?

This will probably be an iterative process, I will outline some ideas which
might be discarded later. But I have to start somewhere.

## Settings/Options categories
There are many things we have to take care of, it seems a good idea to separate
them into sub-menus/tabs (if most games do it, there must be a reason). So we
could divide the menu into the following categories:
- General: For general things related to the game. Set language, save location,
           and whatever doesn't fit the other categories.
- Graphics/Video: Anything related to graphics. Set the resolution,
                  enable/disable fancy effects and so on.
- Audio: To control the audio levels.
- Controls: To display and modify the controls of the game.

Those seem to be enough for a simple menu. But which specific settings go into
each category? What about accessibility? Is FPS limit a graphics or a general
thing? There are many things to define yet.

Since I want to get something working as soon as possible, I will start small.
Let's start with, in my opinion, the simplest settings: the Audio (simplest
in terms of making the settings menu, actually managing the audio is a different
thing).

### Audio Settings
The audio settings will manage different audio levels:
- Main/General output audio level (audio level of what goes into the speaker).
- Music audio level, for background music.
- SFX audio level, for in-game sounds.
- UI audio, for general ui audio.

Those seem to be enough. Of course, different games might have different
requirements.

#### What do we want to do with each audio level? Which are the actual settings?
My first idea is that each audio level has:
- Volume slider that goes from 0 to 100.
- Button to mute/unmute the audio.

Then it would be nice that the audio stores the value of the volume when it is
muted and restores it when it is unmuted.

Also what about the audio automatically unmuting if we increase the volume?
If we increase the volume it is because we want to hear things, so it makes sense
to unmute. This could prevent the player from increasing the volume too much.
On the other hand, if the audio is muted keep it muted when decreasing
the volume.

## What's next?
It would be nice to have some diagrams to see how things are organized. At the
end the layout and overall visual appearance of the menu are quite important as
a good menu should be clear and intuitive.

The current goal is to make the audio menu, once that's done we can focus on
making it look nice.
