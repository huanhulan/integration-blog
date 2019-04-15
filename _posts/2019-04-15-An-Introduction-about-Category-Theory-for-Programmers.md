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

We, as homo sapiens, have been solving problems by decomposing-recomposing things since the begin of our time. From chiseling stone into weapons to building shuttles sending human into space. We do things this way because of our minds are limited, we have to chop huge information down apart into smaller parts that a individual's mind can process biologically, and then recomposing the to get full picture.

As programmers and software engineers, we do things this way in our daily jobs conformably and repeatedly: decomposing one huge system into many sub systems or micro services and then using things like function calls or networking to glue them up while following specific architectural style to meet the requirements that the system needed.

And of course, we also have another powerful tool —— abstraction. When dealing when a complex entity, we always defining specific interfaces for it to hide the complexity for the sake of the others during our programming activities. In day-to-day life, we also do thing this way, that's why we have things like phrases and algebras.

But abstraction has levels. Taking music as an example, sound of a music are causing by vibrations and human have been able to record vibrations almost about 2 centuries. But there is another ancient way of recording music and that is by using music score: encoding music into notations that can be interpreted into musicians' music activities.
