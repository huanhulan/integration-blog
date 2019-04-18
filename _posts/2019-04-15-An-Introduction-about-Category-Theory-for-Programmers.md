---
 layout: post
 title:  "An Introduction about Category theory for Programmers"
 date:   2019-04-15 10:00:00 +0800
 categories: blog
 author: Lex Huang
---

> Mathematical objects are determined by--and understood by--the network of relationships they enjoy with all the other objects of their species.

>> -- <cite>[Barry Mazur][1]</cite>

---
## Introduction

We, as homo sapiens, have been solving problems by decomposing-recomposing things since the beginning of our time. From chiseling bones into arrows to building space shuttles from scratch sending human into space. We do things this way because of our minds are limited, we have to chop huge information down apart into smaller parts that an individual's mind can process biologically, and then recomposing them back to get full picture of the original information.

As programmers and software engineers, we do things this way in our daily jobs conformably and repeatedly: decomposing one huge system into many sub systems or sub-modules and then using things like function calls or networking to glue them up by following specific architectural style like micro-service to meet the requirements that the system needed.

And of course, we also have another powerful tool —— abstraction. When dealing when a complex entity, we always define specific interfaces for it to hide the complexity for the sake of the others during our programming activities. In day-to-day life, we also do thing this way, that's why we have things like phrases and algebras.

But abstraction has levels, and our human brain can only understand things that are in good level abstraction. Taking music as an example, sound of a music are causing by vibrations and human have been able to record vibrations almost about 2 centuries. But the sound wave, just like assembly language, is extremely hard (but not impossible) for human to understand its information, only the machine can read and [play](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/j_s_bach_-_cello_suite_1007_complete.mp3) it easily.

 ![Sound wave of **Suite No. 1, BWV 1007, In G:Prelude**](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/j_s_bach_-_cello_suite_1007_complete001_sound_wave.png)

But there is another ancient way of recording music and that is by using music score: encoding music into notations that can be interpreted into musicians' music activities.

 ![Sound wave of **Suite No. 1, BWV 1007, In G:Prelude**](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/j_s_bach_-_cello_suite_1007_complete001.png)

Then the following question should be: what is the good abstraction? What's tool to examine the good from bad? Generally speaking, the answer is mathematics. And in our case, which is examining the way of decomposing-recomposing things, the tool is Category-theory.

So what is the category-theory about? It’s big insight is that we can figure out a thing by its relationship with the other things other than thinking about its intrinsic characteristic, and composition is at the very root of the category theory. Just like we can figure out a person's characteristic by observing his/her interaction and relationships with the others.

For mathematicians, the category theory provides a way to find a good (not the good, because there is no such thing) level of abstraction for thinking about mathematical structures. For us, as programmers and software engineers, we can use it to figure out things like: whether two functions can always be composed together, whether one data structure is alike to the other, whether the abstraction level of the code for a question is good enough, or even how to confine and express side effects, etc.

In the following chapters, I will try to explain basic concepts of the category theory intuitively and also show you the applications by using `Typescript` (Because our team are mainly focusing on front-end, even thought I'd love to use Haskell notations because it's much terser). Don't fear the math, it's deadly simple. Now make yourself a coffee and let's begin our tour.

## Basic Concepts of Category theory

### Objects and Arrows

The simplest formal definition of a category is:

> A category ***C*** consists of
>
>> * a collection *C*<sub>0</sub> of objects;
>>
>> * for each pair *x*, *y* of objects, a collection *C*<sub>1</sub>(x, y) of morphisms from *x* to *y*;
>>
>> * for each triple *x*, *y*, *z* of objects, a function ∘:
>>>
>>>> *C*<sub>1</sub>(y, z) × *C*<sub>1</sub>(x, y) → *C*<sub>1</sub>(x, z)
>>>
>>> which assigns, to any appropriate pair of morphisms *f*, *g*, their composite morphism g∘f (called: g after f)
>>
>> * for each object *x*, an element id<sub>x</sub> or 1<sub>x</sub> of *C*<sub>1</sub>(x, x), the identity morphism on x;
>>
>> * such that the following properties are satisfied:
>>>
>>> * associativity: for each quadruple *w*, *x*, *y*, *z* of objects, if *f* ∈ *C*<sub>1</sub>(y, z), *g* ∈ *C*<sub>1</sub>(x, y), and *h* ∈ *C*<sub>1</sub>(w,x), then (*f*∘*g*)∘*h*=*f*∘(*g*∘*h*);
>>> * the left and right unit laws: for each pair *x*, *y*, of objects, if *f* ∈ *C*<sub>1</sub>(x,y), then 1<sub>y</sub>∘*f*=*f*=*f*∘1<sub>x</sub>.

People also often write *x* ∈ ***C*** instead of *x* ∈ *C*<sub>0</sub> as a short way to indicate that *x* is an object of ***C***.

TLTR, intuitively, a category can be seen as quiver filled with arrows and objects, each arrow has following properties:

1. each arrow has two objects on their head and tail, each objects can be seen as a ***set***,
2. and there are special arrows that the head and tail objects are the same,
3. if one arrow's head match with the others tail, then there must be another arrow in the quiver which is the two arrow combined together.

And we can also explaining a category visually by using commutative diagrams. It's also deadly simple, just drawing all the information into a paper: objects as dots, morphisms goes between the dots.

![A category consists of 3 objects and their morphisms](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/3_objs_category.png)

An agile reader might ask: Why do we need identity? The answer is that it's like 0 when studying addition, which does nothing but significant: because once you have it, you can do algebras. The identity morphisms in category theory usually does nothing, [but the concept of isomorphism relies for its definition on the concept of identity morphism][2]:

> Given objects *A* and *B* in a category ***C***, an isomorphism is a morphism ƒ: A → B that has an inverse, i.e. there exists a morphism g: B → A with ƒ∘g = 1<sub>B</sub> and g∘ƒ = 1<sub>A</sub>.

In a programming language, Typescript in our case, we can treat types as objects(not only the type that are predefined in Typescript, but also your own classes), and morphisms as function that mapping between types. But one thing to notice that, functions should be total, which means given one specific input value, there can be only one determined effect, which means they are pure. So the type notation for the identity in Typescript morphism can be written as:

```Typescript
function identity<T>(arg: T): T {
    return arg;
}
```

***
Note that this is not exactly a categorical identity morphism, because:

```Typescript
const nan = NaN;
identity(nan) === nan // false
```

***

So far we are able to prove the correctness of our program in some extent: by only using type notations of our functions to prove whether functions can be composed together while leaving the implementation alone. (But still a long way to do the [*formal verification*][3], the other tools would be Type theory, denotational/operational semantics, proof theory, etc).

As you might know, the functions between ***set*** can be a ***surjection*** or  an ***injection*** or can be both in the same time. In category theory, the morphisms has similar properties called epimorphism and monomorphism.

An epimorphism in a category is a morphism *f*: *X*→*Y* that for all morphisms *g*<sub>1</sub> and *g*<sub>2</sub>: *Y*→*Z*:
> *g*<sub>1</sub> ∘ *f* = *g*<sub>2</sub> ∘ *f* => *g*<sub>1</sub> = *g*<sub>2</sub>

An monomorphism in a category is a morphism *f*: *X*→*Y* that for all morphisms *μ*<sub>1</sub> and *μ*<sub>2</sub>: *W*→*X*:
> *f* ∘ *μ*<sub>1</sub> = *f* ∘ *μ*<sub>2</sub> => *μ*<sub>1</sub> = *μ*<sub>2</sub>

In a category whose objects are sets, if an morphism is both `epi` and `mono`, then we can tell that the morphism is `invertible` and the head and the tail objects of the arrow are `isomorphic`.

An simple example of isomorphic can be found in the representations of natural numbers. Given infinite amount of storage, let's say:
> `[]`, as an instance of array, can be treated as 0;
>
> `[[]]` can be treated as 1;
>
> `[[[]]]` can be treated as 2;
>
> ...

Then it's easy to prove that there is an isomorphism between all natural numbers and this square brackets kind of representation. Actually this kind of representation has a name-- `Curry-Howard isomorphism`.

### Universal Properties and Universal Construction

As we mentioned above, category theory give us ability to study a object in terms of its relationships with the others. One way of achieving this is by finding *universal properties*.

Here I will using a brilliant example from [Eugenia Chen][4]'s wonderful talk -- [Category Theory in Life][5].

The factors of 30 can form following diagram:

![factors of 30](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/factors_of_30.png)

Since the integers can be represented as ***sets*** in *set theory*, then the diagram above can be further interpreted into following diagrams by 30's prime factors (for simplicity, using {*x*, *y*} as cartesian product *x*×*y*):

![factors of 30 in set](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/factors_of_30_1.png)

The same methodology can be applied to any other 'prime factors', take 'clean', 'performance' and 'architect' as an example, clearly the following diagram holds:

![software style comparison](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/software_style_comparison.png)

So why? Math is tool to answer 'why did this happen?', and the category theory is the tool to answer 'why math happens?'. The diagrams above holds because of the existence of the prime factors. So by combining them, we can get other factors, all the way down, finally we got ourselves a *partial ordered set*. And the reason why the second one can use the same logic is because of the isomorphism between these dual concepts:
> clean → dirty ≌ performance → slow ≌ architect → no-architect

So the similarity between the diagrams kinda explain the word 'universal'. Now I will formally introduce the concept of *universal properties*.

First, ***initial object***:
> An initial object in a category ***C*** is a object *I* such that for all object *X* there is a unique morphism:
>> *I* → *X*

Second, ***terminal object***:
> An terminal object in a category ***C*** is a object *T* such that for all object *X* there is a unique morphism:
>> *X* → *T*

In the diagrams above, the initial objects are 30, {2,3,5} and 'clean performance architect' because they have connections between any other objects in the diagram that they belong to (recall the properties of morphisms' composition). And the terminal objects are 1, {∅} and 'dirty slow no-architect'.

But a category may have many initial/terminal objects if it has. But all of them, initial/terminal objects, are unique up to a unique isomorphism. So in some extent, we can use the word 'the' in the definition of the initial/terminal object.

Further more, in the category of sets (I will denote it as ***Set*** in follows chapters), the initial object is the empty set:∅, because ∅ is subset of every set. And the terminal object in ***Set*** are the the singleton set (again, singleton is up to isomorphism).

Since we said that types can be seen as sets, so morphisms points to the terminal object can be written in Typescript as:

```Typescript
function unit(arg:any){
    return [];// {} is also good enough
}
```

But when we can't written a strict morphisms points out from the initial object, because theoretically, empty set can't have members, so any morphism points out from empty set is the empty set itself. But *void*, *null* and *undefined* are all implemented in javascript, so they have members which means they are not empty at all. That's why I will using `Haskell` notation for the morphism called *absurd*, since the `Void` in Haskell` corresponds to the concept of empty set:

```Haskell
absurd :: Void -> a
```

Back to game, you see that the definition of the initial and terminal object are simply inverting the arrows. This shows a very powerful aspect of the category theory: ***duality***. Which means we only need to prove a theorem or construct a category for one direction and by flipping every arrows in the original diagram we are guarantee to get the other in opposite category for free which really increases the productivity. The constructions in the opposite category are often prefixed with “co”. As I will introduce products and co-products as example latter.

### Functor

Todo

### Kleisli Category and Monad

Todo

## Conclusion

Todo

---
## Bibliography
1. [Category theory for programmers](https://bartoszmilewski.com/2014/10/28/category-theory-for-programmers-the-preface/)
2. [TheCatster's video](https://www.youtube.com/user/TheCatsters/playlists)
3. [Seven Sketches in Compositionality](http://math.mit.edu/~dspivak/teaching/sp18/7Sketches.pdf)
4. [Reason Isomorphically!](http://www.cs.ox.ac.uk/people/daniel.james/iso/iso.pdf)
5. [What you needa know about Yoneda](https://www.cs.ox.ac.uk/jeremy.gibbons/publications/proyo.pdf)

[1]:https://en.wikipedia.org/wiki/Barry_Mazur
[2]:https://www.quora.com/What-is-the-purpose-of-identity-morphisms-in-category-theory
[3]:https://en.wikipedia.org/wiki/Formal_verification
[4]: https://en.wikipedia.org/wiki/Eugenia_Cheng#Early_life_and_education
[5]: https://www.youtube.com/watch?v=ho7oagHeqNc
