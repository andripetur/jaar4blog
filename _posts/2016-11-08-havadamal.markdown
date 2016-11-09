---
layout: post
title:  "Hávaðamál"
date:   2016-11-08 10:06:00 +0100
categories: havadamal jaar4
---

Hávaðamál is a performance of an algorithmic composition that uses the text of [Hávamál][havamal] as source material.
The text is analyzed to generate a digital score, and dictated/chanted by a performer. The input of the performer will affect the score, and the score will affect the performer.

This project started out as an idea to make a live coding performance within a bigger artistic whole, building the setting/mood with the aid of visualization and physical representation of the coding.  
While testing the waters for the project I realized that I was making a pretty big assumption that I would get my live coding skills up to par in a short time. Being a Supercollider newbie, and having ruled out using (ó)regla (since it would cost to much development time) I started moving towards ways of using less code as input. Searching for inspiration I came by a performance by [Yarn:Wire(starts@08:42)][ywconcert] and it gave me the impression that what made the piece work was that it was telling a story. The thought that music can tell a story, reversed and interpreted quite literally led me to translating text of a improvised story to music.   
During tryouts with a proof of concept text-to-music Supercollider patch, it dawned on me that in order for the text translation to be musical (without endless filtering and babysitting), it would be ideal if the text already had structure to it. Pop/rap lyrics & poetry had a much more musical output than the text of a story for example.
So I had really just mitigated the problem of doubting about being able to improvise code, to improvising well constructed text, that also tells a story. A solutions to this problem is to use text that already has structure to it and generate a digital score from its content. The score is then performed by software/instrumentalist, and the text is vocalized by a performer (me). The voice will then influence the score and vice versa for a nice feedback-y stew.

__About Hávamál__  
[Hávamál][havamal] is a part of the [Poetic Edda][poetic-edda]; a collection of Old norse poems that passed orally between people, until they were collected into [Konungsbók][konungsbok] in the 13th century.
There are some speculations based on notations in the margins of Konungsbók, that some of the poems text were written to be acted out, or to be performed with musical accompaniment.
Hávamál has a total of 160 stanzas, split into a five sections each with its own theme.  I'll focus in the first 76 stanzas which are called Gestaþáttr, where [Odin][odin] shares his wisdom on proper living and manners.

__Að kveða rímur__  
Rímur: A "rhyme", an poem written in a specific meter.  It has long tradition in Icelandic literature,  evolving from the poetry of the poetic edda.
Að kveða: The style used to perform Rímur, a blend between spoken and sung poetry. A simple melody is paired with the text and it's up to the performer to add accents and flourish.
During Kvöldvökur (working nights), in pre-modern Iceland the people on the farm would gather in the living quarters of the farmhouse, and the guest, or a member of the household would tell stories, sing a rímu for entertainment/education while the others worked. Some performers where even good enough that they managed to make it their sole profession to travel between places and sing rímur.

Ríma kveðin:  
<iframe width="560" height="315" src="https://www.youtube.com/embed/NsuizeBOcrk?rel=0" frameborder="0"></iframe>
<!-- <iframe width="560" height="315" src="https://www.youtube.com/embed/NCXKF2xwh3Y?rel=0" frameborder="0"></iframe> -->

Possible elements to mirror in composition elements:  
-- Chant  
-- solo  
-- simple repetitive melody  
-- variation through expression    
-- tempo rubato  

<br>
Icelandic folk music:  
<iframe width="560" height="315" src="https://www.youtube.com/embed/LBxLPiMk7rI?rel=0" frameborder="0"></iframe>

Possible elements to mirror in composition/synthesis elements:  
-- Unison  
-- Canon  
-- Modal (one scale)  
-- Fifths are so cool.  

__Score rule set draft1__  
`a-z` -> Length from previous character -> controlValue  
`space` -> Short rest  
`.`   -> Long rest, restart to root  
`,`   -> ((short + long) / 2) rest  
`-`   -> accent  
`:`   -> the letters of the following sentence are used as control values for voice channel.  
`;`   -> the preceding and following sentence are interpolated to be used as control values for voice channel.  
`!`   -> mods preceding sentence, more intense(amp,speed).  
`?`   -> mods preceding sentence, less intense(amp,speed).

Even more possibilities:   

* text parameters:  
  * Sentence length  
  * Line length  
  * Wordcount  (sentence, line, verse)  
  * Capital letters  
  * Broddstafir  

* Voice parameters:  
  * Pitch  
  * isSinging / isTalking  
  * Amplitude envelope.  
  * Spectrum  
  * Noisyness -> isVowel/isConsonant  

* Manual Cues?
  * words  
  * verse number  
  * line number  


___Hávaðamál block diagram___  

![Block diagram of hávaðamál]({{ site.baseurl }}/assets/hm-block.png)

[ywconcert]: https://vimeo.com/93466218
[havamal]: https://en.wikipedia.org/wiki/H%C3%A1vam%C3%A1l
[odin]: https://en.wikipedia.org/wiki/Odin
[poetic-edda]: https://en.wikipedia.org/wiki/Poetic_Edda
[konungsbok]: https://en.wikipedia.org/wiki/Codex_Regius
<!-- https://www.youtube.com/watch?v=pufvwWFUqPE   -->
