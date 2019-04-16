---
 layout: post
 title:  "An Introduction about Category theory for Programmers"
 date:   2019-04-15 10:00:00 +0800
 categories: blog
 author: Lex Huang
---

> Mathematical objects are determined by--and understood by--the network of relationships they enjoy with all the other objects of their species.

>> -- <cite>[Barry Mazur][1]</cite>

[1]:https://en.wikipedia.org/wiki/Barry_Mazur

## Introduction

We, as homo sapiens, have been solving problems by decomposing-recomposing things since the beginning of our time. From chiseling bones into arrows to building space shuttles from scratch sending human into space. We do things this way because of our minds are limited, we have to chop huge information down apart into smaller parts that an individual's mind can process biologically, and then recomposing the to get full picture.

As programmers and software engineers, we do things this way in our daily jobs conformably and repeatedly: decomposing one huge system into many sub systems or sub-modules and then using things like function calls or networking to glue them up by following specific architectural style like micro-service to meet the requirements that the system needed.

And of course, we also have another powerful tool —— abstraction. When dealing when a complex entity, we always define specific interfaces for it to hide the complexity for the sake of the others during our programming activities. In day-to-day life, we also do thing this way, that's why we have things like phrases and algebras.

But abstraction has levels, and our human brain can only understand things that are in good level abstraction. Taking music as an example, sound of a music are causing by vibrations and human have been able to record vibrations almost about 2 centuries. But the sound wave, just like assembly language, is extremely hard (but not impossible) for human to understand its information, only the machine can read and [play](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/j_s_bach_-_cello_suite_1007_complete001.mp3) it easily.

 ![Sound wave of **Suite No. 1, BWV 1007, In G:Prelude**](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/j_s_bach_-_cello_suite_1007_complete001_sound_wave.png)

But there is another ancient way of recording music and that is by using music score: encoding music into notations that can be interpreted into musicians' music activities.

 ![Sound wave of **Suite No. 1, BWV 1007, In G:Prelude**](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/j_s_bach_-_cello_suite_1007_complete001.png)

Then the following question should be: what is the good abstraction? What's tool to examine the good from bad? Generally speaking, the answer is mathematics. And in our case, which is examining the way of decomposing-recomposing things, the tool is Category-theory.

So what is the category-theory about? It’s big insight is that we can figure out a thing by its relationship with the other things other than thinking about its intrinsic characteristic, and composition is at the very root of the category theory. For mathematicians, it provides a way to find a good (not the good, because there is no such thing) level of abstraction for thinking about mathematical structures. For us, as programmers and software engineers, we can use it to figure out things like: whether two functions can always be composed together, whether one data structure is alike to the other, whether the abstraction level of the code for a question is good enough, or even how to confine and express side effects, etc.

In the following chapters, I will try to explain basic concepts of the category theory and also show you the applications by using Typescript (since our team are mainly focusing on front-end even thought I'd love to use Haskell notations).
