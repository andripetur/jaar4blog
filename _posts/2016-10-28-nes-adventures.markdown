---
layout: post
title:  "NES adventures"
date:   2016-10-28 18:06:00 +0200
categories: oglaf jaar4
---

This week I started laying the technical groundwork soundtrack to the ascii game `Oglaf the seasick warrior`. The idea is to draw inspiration from the consoles of ye olde times like the NES, chiptune, 8bit aesthetics and whatnot. But with an algorithmic compositional twist.  
[Flocking][flocking] will be used for the audio synthesis and the composition system of my 3. year project [(ó)regla][oregla] will be used for the algorithmic composition.

Since (ó)regla is originally written to be used as a live coding environment the first step was to see how much of a hazzle it would be to make extract the composition system from the GUI.  Fortunately I seem to have categorized the code quite neatly when I was writing it, so making a "headless" version was simply a question of editing the combine script I use to assemble (ó)reglas javascript to exclude the GUI elements. I did run into a few weird bugs, but they  easily fixable(after a few well picked swear words), a semicolon missing there, and useless extension of native javascript object here.  

With (ó)regla working within Oglafs environment, I wanted to start by making simple algorithmic sketches, to help me figure an approach to the composition. My first instinct was to take two melodies, and by comparing the two, only allowing a few chosen intervals to sound at the same time.
Since (ó)regla has built in scale lock, I figured this couldn't take to much time to throw together.  Buuut after two hours of coding and tweaking, everything still sounded random and dissonant. It didn't make any sense since the logic seemed solid.  Out of curiosity I looked up the scale lock function in (ó)regla and behold there was the problem, a missing modulo.

So after changing:
{% highlight javascript %}
Scale.prototype.lockTo = function(n){
  if(this.absolute.length === 0) return n;
  return this.absolute.indexOf(n) >= 0 ? n : n+1;
}
{% endhighlight %}

to:
{% highlight javascript %}
Scale.prototype.lockTo = function(n){
  if(this.absolute.length === 0) return n;
  return this.absolute.indexOf(n%12) >= 0 ? n : n+1;
}
{% endhighlight %}

Turns out my brilliantly thought out scaleLock function hadn't been working for I don't know how long, and how I managed to not notice it is beyond me.  But three keystrokes later all was well, and the simple musical sketch sounded like angels singing in comparison with the previous abomination it had spewed out.

<iframe width="100%" height="166" scrolling="no" frameborder="no" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/290409980%3Fsecret_token%3Ds-DIdQZ&amp;color=ff5500&amp;auto_play=false&amp;hide_related=false&amp;show_comments=true&amp;show_user=true&amp;show_reposts=false"></iframe>

<br>

Next up I looked into moving closer to the 8-bit sound aesthetics, luckily there is plenty of info to find on to make chiptuny sounds, to focuse the search I decided to use the NES as a reference.

* Some light reading:
* [How to make 8-bit game music][nes-howto]
* [The nes chip specs at a clance][famitracker]
* [More detailed nes specs][nes-spec]

<iframe width="640" height="360" src="https://www.youtube.com/embed/la3coK5pq5w?rel=0" frameborder="0" allowfullscreen></iframe>

The NES has 5 channels.

1. Pulse wave w. four duty settings (12.5%, 25%, 50% and 75%).
2. identical to channel 1.
3. Triangle wave.
4. Noise, with two modes, and 16 pitch presets. 32 total settings.
5. Sample channel, low-quality DMC samples.

[flocking][flocking] is gloriously modern and hi-fi, so the first step is to nudge that bit depth down a notch.  Unfortunately flocking doesn't come with a bitcrusher, or 8bit wavetables, and documentation on writing your own ugens to the best of my googling skills non-existent(understandably though since it's still very much in active development).
But by working with a reference from existing ugens in the flocking source code, I got together a working bitcrusher, and a custom 8 bit wavetable triangle oscillator. Flocking has the ugen `lfPulse` a pulse oscillator with variable duty cycle.
Instead of implementing an old timy sample player, I will pre-process audio samples so they sound old, and replay them with our fancy hifi techniques.

So channel 1 & 2 signal flow looks like this.  
`lfPulse` -> `envelope` -> `bitcrusher` -> `master bus`

Channel 3 signal flow.  
`triangle8` -> `envelope` -> `bitcrusher` -> `master bus`

When summing a bunch of 8-bit signals we could end up with a number that doesn't fit in a 8-bit representation, so we throw a bitcrusher on the master output for good measure.  
`master bus` -> `bitcrusher` -> `out`

Getting the noise channel right was a bit more of a challenge, after some fiddling around, trying to find an explanation of the algorithm to implement myself, I came across the [tetris noise algorithm][tetris-noise] implemented in javascript, I threw it in a ugen, and it sounded closer than the ordinary flocking `noise`, but still far from the NES. I ended up looking at the source code for an [emulator of Nes sound chip][nes-sound-emu] written in c++, and using it as an reference I got `nesNoise` working as a ugen in flocking!

<iframe width="100%" height="166" scrolling="no" frameborder="no" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/290412529%3Fsecret_token%3Ds-xYuDa&amp;color=ff5500&amp;auto_play=false&amp;hide_related=false&amp;show_comments=true&amp;show_user=true&amp;show_reposts=false"></iframe>

<br>

When recording the test for this post, I noticed that I got some major dc offset going on in the noise, so I'm going to have to take a closer look at the scaling of the noise before sending it to the output.  


[flocking]: http://flockingjs.org/
[oregla]: http://andripetur.github.io/oregla/
[nes-howto]: https://thesoundgrad.com/2013/10/09/how-to-make-old-school-8-bit-video-game-sound-effects/
[famitracker]: http://famitracker.com/wiki/index.php?title=2A03#Internal_2A03.2F2A07_channels
[nes-spec]: http://nesdev.com/2A03%20technical%20reference.txt
[tetris-noise]: https://diplograph.net/posts/the_nes_tetris_prng
[nes-sound-emu]: http://www.slack.net/~ant/libs/audio.html#Nes_Snd_Emu
