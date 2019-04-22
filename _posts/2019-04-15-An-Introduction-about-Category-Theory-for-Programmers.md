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

We, as homo sapiens, have been solving problems by decomposing-recomposing things since the beginning of our civilization. From chiseling bones into arrows to building space shuttles from scratch sending human into space. We do things this way because of our minds are limited, we have to chop huge information down apart into smaller parts that an individual's mind can process biologically, and then recomposing them back to get full picture of the original information.

As programmers and software engineers, we do things this way in our daily jobs conformably and repeatedly: decomposing one huge system into many sub systems or sub-modules and then using things like function calls or networking to glue them up by following specific architectural style like micro-service to meet the requirements that the system needed.

And of course, we also have another powerful tool —— abstraction. When dealing when a complex entity, we always define specific interfaces for it to hide the complexity for the sake of the others during our programming activities. In day-to-day life, we also do thing this way, that's why we have things like phrases and algebras.

But abstraction has levels, and our human brain can only understand things that are in good level abstraction. Taking music as an example, sound of a music are causing by vibrations and human have been able to record vibrations almost about 2 centuries. But the sound wave, just like assembly language, is extremely hard (but not impossible) for human to understand its information, only the machine can read and [play](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/j_s_bach_-_cello_suite_1007_complete.mp3) it easily.

 ![Sound wave of **Suite No. 1, BWV 1007, In G:Prelude**](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/j_s_bach_-_cello_suite_1007_complete001_sound_wave.png)

But there is another ancient way of recording music and that is by using music score: encoding music into notations that can be interpreted into musicians' music activities.

 ![Sound wave of **Suite No. 1, BWV 1007, In G:Prelude**](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/j_s_bach_-_cello_suite_1007_complete001.png)

Then the following question should be: what is the good abstraction? What's tool to examine the good from bad? Generally speaking, the answer is mathematics. And in our case, which is examining the way of decomposing-recomposing things, the tool is Category-theory.

So what is the category-theory about? It’s big insight is that we can figure out a thing by its relationship with the other things other than thinking about its intrinsic characteristic, and composition is at the very root of the category theory. Just like we can figure out a person's characteristic by observing his/her interaction and relationships with the others.

For mathematicians, the category theory provides a way to find a good (not the good, because there is no such thing) level of abstraction for thinking about mathematical structures. For us, as programmers and software engineers, we can use it to figure out things like: whether two functions can always be composed together, whether one data structure is alike to the other, whether the abstraction level of the code for a given question is good enough, or even how to confine and express side effects, etc.

In the following chapters, I will try to explain basic concepts of the category theory intuitively and also give applications by using `Typescript` (Because our team are mainly focusing on front-end, even thought I'd love to use Haskell notations because it's much terser). Don't fear the math, it's deadly simple. Now make yourself a coffee and let's begin our tour.

## Basic Concepts of Category theory

### Objects and Arrows

The simplest formal definition of a category is:

> A category ***C*** consists of
>
>> * a collection *C*<sub>0</sub>, or ob(***C***), of `objects`;
>>
>> * for each pair *x*, *y* of objects, a collection *C*<sub>1</sub>(x, y) of `morphisms` from *x* to *y*;
>>
>> * for each triple *x*, *y*, *z* of objects, a function ∘:
>>>
>>>> *C*<sub>1</sub>(y, z) × *C*<sub>1</sub>(x, y) → *C*<sub>1</sub>(x, z)
>>>
>>> which assigns, to any appropriate pair of morphisms *f*, *g*, their composite morphism g∘f (called: g after f)
>>
>> * for each object *x*, an element id<sub>x</sub> or 1<sub>x</sub> of *C*<sub>1</sub>(x, x), the `identity morphism` on x;
>>
>> * such that the following properties are satisfied:
>>>
>>> * `associativity`: for each quadruple *w*, *x*, *y*, *z* of objects, if *f* ∈ *C*<sub>1</sub>(y, z), *g* ∈ *C*<sub>1</sub>(x, y), and *h* ∈ *C*<sub>1</sub>(w,x), then (*f*∘*g*)∘*h*=*f*∘(*g*∘*h*);
>>> * `the left and right unit laws`: for each pair *x*, *y*, of objects, if *f* ∈ *C*<sub>1</sub>(x,y), then 1<sub>y</sub>∘*f*=*f*=*f*∘1<sub>x</sub>.

People also often write *x* ∈ ***C*** instead of *x* ∈ *C*<sub>0</sub> as a short way to indicate that *x* is an object of ***C***.

TLTR, intuitively, a category can be seen as quiver filled with arrows and objects, each arrow has following properties:

1. each arrow has two objects on their head and tail, each objects can be seen as a ***set***,
2. and there are special arrows that the head and tail objects are the same,
3. and there can be empty arrows means that there is no relationships between the arrow's head and its tail,
4. if one arrow's head match with the others tail, then there must be another arrow in the quiver which is the two arrow combined together.
5. Arrows can represent `any binary relationship`, from the ≤ relationship in natural numbers to description like “_ is _'s grand parents”.

And we can also explaining a category visually by using commutative diagrams. It's also deadly simple, just drawing all the information into a paper: objects as dots, morphisms goes between the dots.

![A category consists of 3 objects and their morphisms](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/3_objs_category.png)

A simplest category is an empty quiver, there are no arrow inside of it.

An agile reader might ask: Why do we need identity? The answer is that it's like 0 when studying addition, which does nothing but significant: because once you have it, you can do algebras. The identity morphisms in category theory usually does nothing (soon we can see an identity that does something), [but the concept of isomorphism relies for its definition on the concept of identity morphism][2]:

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

As you might know, the functions in the `set theory` can be a `surjection` (which simply means that for every elements in the co-domain there is a corresponding element in the domain), or  an `injection` (different elements in the domain would be mapping to different element in the co-domain) or can be both at the same time. In category theory, the morphisms has similar properties called `epimorphism` and `monomorphism`:

* An epimorphism in a category is a morphism *f*: *X*→*Y* that for all morphisms *g*<sub>1</sub> and *g*<sub>2</sub>: *Y*→*Z*:
    > *g*<sub>1</sub> ∘ *f* = *g*<sub>2</sub> ∘ *f* => *g*<sub>1</sub> = *g*<sub>2</sub>

* An monomorphism in a category is a morphism *f*: *X*→*Y* that for all morphisms *μ*<sub>1</sub> and *μ*<sub>2</sub>: *W*→*X*:
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

Then it's easy to prove that there is an isomorphism between all natural numbers and this square brackets kind of representation. Actually this kind of representation has a name -- `Curry-Howard isomorphism`.

### Monoid

Yet another deadly simple category. Despite of the definition of `monoid`s in `Group theory`, in category theory a monoid is just a category who has only one object along with a set of endo-morphisms that from and go back to itself.

This simple definition is actually powerful: the only object it has can be seen as a set that contains all the possible combinations of values of a given monoid, and the multiplication rule of a monoid is automatically satisfied!

The traditional way of defining a monoid in typescript is:

```Typescript
type Monoid<T> = {
  mempty: T,
  mappend: (first:T)=>(second:T)=>T
}
```

and a trivial example is the list concatenation, since concatenating with an empty list is interchangeable:

```Typescript
const listConcatenation: Monoid<any[]> = {
  mempty: [];
  mappend: Array.prototype.concat
}
```

And of course, for a fundamental idea that permeates category theory and the whole of mathematics, the story goes deeper than these few words. For those who are interested in it, there is a article, [Monoids on Steroids](https://bartoszmilewski.com/2017/02/09/monoids-on-steroids/), explains the very idea in great detail.

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

So the similarity between the diagrams kinda explain the word 'universal'. Now I will formally introduce two basic concept of *universal properties*.

First, `initial object`:
> An initial object in a category ***C*** is a object *I* such that for all object *X* there is a unique morphism:
>> *I* → *X*

Second, `terminal object`:
> An terminal object in a category ***C*** is a object *T* such that for all object *X* there is a unique morphism:
>> *X* → *T*

In the diagrams above, the initial objects are 30, {2,3,5} and 'clean performance architect' because they have connections between any other objects in the diagram that they belong to (recall the properties of morphisms' composition). And the terminal objects are 1, {∅} and 'dirty slow no-architect'.

But a category may have many initial/terminal objects if it has. But all of them, initial/terminal objects, are unique up to a unique isomorphism. So in some extent, we can use the word 'the' in the definition of the initial/terminal object.

Further more, in the category of sets (I will denote it as ***Set*** in follows chapters, be careful it starts with the upper case 'S'), the initial object is the empty set:∅, because ∅ is subset of every set. And the terminal object in ***Set*** are the the singleton set (again, singleton is up to isomorphism).

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

Back to game, We see that the definition of the initial and terminal object are simply inverting the arrows. This shows a very powerful aspect of the category theory: ***duality***. Which means we only need to prove a theorem or construct a category for one direction and by flipping every arrows in the original diagram we are guarantee to get the other in opposite category for free which really increases the productivity. The constructions in the opposite category are often prefixed with the world: “*co*”. I will use the products and co-products as an example.

Imagine there are two lines, colored in red and blue like the picture down below. How many ways can you describe them all at the same time?

![software style comparison](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/two_lines.png)

Of course the most intuitive way is just like what I said, simply just putting them together. In a mathematical point of view, if we just thinking the two line as two sets of coordinates, then the structure to describe the two line into one is just a `disjoint union` of two sets -- a set that simply just put the constituent sets together even if the constituent sets have their intersections.

![two lines together](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/two_lines_in_disjoint_union.png)

The another way of thinking comes trickier, it's critical insight is that the 1-dimensional lines can be seen as **projections from higher dimensions**. So the two lines above, can be seen as two projections from one single 2-dimensional object using 2-different color of flashlights illuminating at different angles. But this 2-dimensional object can have many shapes, for example, it can be a square whose width and height is equal to the lengths of 2 lines, or a square has the same size but has a hole in its center, or it can be an ellipse whose axis is equal to the lengths of 2 lines, etc.

So which one is the best one? The best one should be the one that doesn't involve extra information. So neither the ellipse nor the square that has a hole meets this requirement. And this is true meaning of the `universal`.

Now let's using a commutative diagram to describe the way we just used to find the best 2-dimensional object which is a square whose width and height is equal to the lengths of 2 lines. As we can see all 3 objects can have projections into the two lines, but 2 of them have 2 projections, which can be seen as cutting of extra information, into the best one we just found.

![relationships between lines and their higher dimension sources](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/product_example.svg)

And we just found ourselves a product! Here is the formal definition of the `categorical product`:

> Let ***C*** be a category with some objects *X<sub>1</sub>* and *X<sub>2</sub>*. A product of *X<sub>1</sub>* and *X<sub>2</sub>* is an object *X* (often denoted *X<sub>1</sub>* × *X<sub>2</sub>*) together with a pair of morphisms π<sub>1</sub> : *X* → *X<sub>1</sub>*, π<sub>2</sub> : *X* → *X<sub>2</sub>* that satisfy the following universal property:
>
> * for every object Y and pair of morphisms f<sub>1</sub> : Y → *X<sub>1</sub>*, f<sub>2</sub>: Y → *X<sub>2</sub>* there exists a unique morphism f : Y → *X<sub>1</sub>* × *X<sub>2</sub>* such that the following diagram commutes:
> >
> > ![product](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/product.svg)
>
> The unique morphism f is called the product of morphisms f<sub>1</sub> and f<sub>2</sub> and is denoted < f<sub>1</sub>, f<sub>2</sub> >. The morphisms π<sub>1</sub> and  π<sub>2</sub> are called the canonical projections or projection morphisms.

In our former example, which is the factors of 30, the 10, as the least common multiple of 2 and 5, is the product of 2 and 5. Because {2, 5, 10, 30} can form a divisibility poset and there is a unique morphism, which is *_ → _/3*, from 30 to 10 which factorizes both morphisms from 30 to 2 and to 5.

---

The phrase `x factorizes y` simply means that morphism y can be composed through x.

---

Now I am showing you the power of the `duality`. By simply flipping the arrow of the commutative diagram of the product, we can get the definition of the `coproduct`:
> Let ***C*** be a category and let *X<sub>1</sub>* and *X<sub>2</sub>* be objects in that category. An object *P* is called the coproduct of these two objects, if there exist morphisms
> > i<sub>1</sub>: *X<sub>1</sub>* → *P*
>
> and
> > i<sub>2</sub>: *X<sub>2</sub>* → *P*
>
> satisfying a universal property: for any object *Y* and morphisms
> > f<sub>1</sub>: *X<sub>1</sub>* → *Y*
>
> and
> > f<sub>2</sub>: *X<sub>2</sub>* → *Y*
>
> there exists a unique morphism:
> > f: *P* → *Y*
>
> such that f<sub>1</sub> = f ∘ i<sub>1</sub> and f<sub>2</sub> = f ∘ i<sub>2</sub>. That is, the following diagram commutes:
> > ![product](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/coproduct.svg)

And the example of the coproduct is the first way we combining the two lines, to be more general, in ***Set***, the coproduct of 2 sets is the disjoint union of them.

The other thing that is the word `universal` implies is that the product and coproduct if they exist, there can be may of them, but all of them are unique up to unique isomorphisms. So that we can using 'the product' or 'the coproduct'.

Now let's see a Typescript application of the categorical product. A pair of values, a two values list in Typescript, can be used to describe a product. Since the product can have 2 projections, then we can use Typescript to defining it:

```Typescript
function fst<T,U>([a, b]:[T, U]):T{
    return a;
}

function fst<T,U>([a, b]:[T, U]):T{
    return b;
}
```

In Typescript, a value of `number` type can be turning into `boolean` by simply using `!!` operator. so there are also a function that can turning `number`:

```Typescript
const m = (a:number) => [a, !!a];
```

But for `number` and `boolean` only, their product is any value of the type `[number, boolean]`, because the following diagram commutes:

![product of number and boolean](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/product_ts.svg)

so the `id` and `a=>!!a` can be composed by using `m` and `fst` or `snd` respectively:

```Typescript
id(x) === fst(m(x));

(a=>!!a)(x) === snd(m(x));
```

The product and coproduct seems simple, but their real power is that by combining them, like addition and multiplication, we can have ourselves the `Algebraic Data Types` (`ADT` for abbreviation) that can be used in everyday programming. For Typescript/Javascript programmers, a famous `ADT` lib is the
[fantasy-land](https://github.com/fantasyland/fantasy-land). For those who want to learn more about `ADT`, have a little understanding of the `Curry-Howard isomorphism` would be a great start.

### Functor

Now we have some understanding about objects and their mappings in a category. It's natural to ask can there be mappings between categories? The answer is yes and  and a map between categories is called a `functor`.

Here is the definition of functor:

> Let ***C*** and ***D*** be categories. A functor *F* : ***C*** → ***D*** consists of:
>
>> * a function: ob(***C***) → ob(***D***)
>>>
>>>> written as: ***C*** ↦ *F*(***C***)
>>>
>> * for each morphism f:C→C' in ***C***, a function:
>>
>>>>> ***C***(C,C') → ***D***(*F*(C),*F*(C'))
>>>>
>>>> written as: f ↦ *F*(f)
>>
>>satisfying the following axioms:
>>
>>> * *F*( f' ◦ f ) = *F*(f') ◦ *F*(f) wherever f: C → C', f': C' → C'' in ***C***
>>>
>>> * *F*(1<sub>C</sub>) = 1<sub>*F*(C)</sub> whenever C ∈ ***

Just like the picture below, a functor guarantees us that it doesn't matter whether we go from `C` to `C'` by using f in ***C*** firstly then go through *F* to *F*C'' or we go to *F*C firstly then to *FC''* through *F*f.

![product of number and boolean](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/functor.svg)

So a functor preserves the structure of the original category but it also enables us with extra properties the target category has. Here I will show you a trivial example of the functor. In typescript, an generic typed array can be seen as *lifting* a value from the original type's category into the category of lists. So given a function:

```Typescript
function numberToString(n:number):string {
    return `${n}`;
}
```

and another function that preserve the structure:

```Typescript
function fmap(f:(number) => string, list: number[]): string[]{
  const [head, ...res] = list;
  return [f(head), ...fmap(f, res)];
}
```

Then we can get a list of string from list of numbers by simply using a function that can goes from a number to a string. As you might noticed, this is exactly the rationale of the `Array.prototype.map`. And of course we can make our `fmap` more generic, I will leave that as an exercise to the readers so that we can move on.

### Natural Transformations

Since functor can be used to compare categories, natural transformations can be used to compare functors. Since it adds another layer of abstraction, it's better for me to draw a diagram for you in the first place.

In the diagram down below, there is a tiny category with only 2 objects and a morphism f between them called ***C***, and there is another category ***D*** that has the 2 images of ***C*** through 2 functors: *F* and *G*. But the images aren't the same. And a natural transformation is just a way of transforming *F*f into *G*f in a way that is natural, which means the morphisms: *F*C→*G*C and *F*C'→*G*C' are 2 morphisms that naturally exist in ***D***.

![natural transformation](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/natural_transformation.svg)

And what this diagram suggesting is that you can go through *F*f firstly the transform along *F*C'→*G*C' to *G*C', and it should be the same as you transform along *F*C→*G*C firstly and doing the *G*f afterwards. And that's what the natural transformation does, it does thing in this way for every instance of morphisms inside of the category ***C***.

So formally, a natural transformation is:
> Given functors *F*,*G*: ***C*** → ***D***, a natural transformation α: *F*=>*G* is given by:
>
> * for all ob(***C***), a morphism:
>
>>> F(ob(***C***)) → G(ob(***C***)) ∈ ***D***
>>
> * for all morphisms f ∈ ***C***, the following naturality square commutes:
>
>>> ![natural square](/integration-blog/assets/2019-04-15-An-Introduction-about-Category-Theory-for-Programmers/natural_square.svg)

___
The α<sub>C</sub> and α<sub>C'</sub> are the components of α on c and C and C' respectively.
___

There is a category of functors for each pair of categories, ***C*** and ***D***. Objects in this category are functors
from ***C*** to ***D***, and morphisms are natural transformations between those
functors. And there is a name for such category: 2-category, and category like I've given above they can be called 1-category since they have simple structure. The morphisms in a 2-category, which are natural transformations, are called 2-morphisms. Here is how thing go wild: every time you add another dimension to your categories, just like we add functor G and F, you get another level of thing between them, and that's why n-dimensional categories form a n+1 dimensional category and infinite-dimensional categories form infinite-dimensional category.

### Kleisli Category and Monad

Todo

## Conclusion

Todo

---

## Bibliography

1. [Category theory for programmers](https://bartoszmilewski.com/2014/10/28/category-theory-for-programmers-the-preface/)
2. [Basic Category Theory](https://arxiv.org/pdf/1612.09375.pdf)
3. [TheCatster's video](https://www.youtube.com/user/TheCatsters/playlists)
4. [Seven Sketches in Compositionality](http://math.mit.edu/~dspivak/teaching/sp18/7Sketches.pdf)
5. [Reason Isomorphically!](http://www.cs.ox.ac.uk/people/daniel.james/iso/iso.pdf)
6. [What you needa know about Yoneda](https://www.cs.ox.ac.uk/jeremy.gibbons/publications/proyo.pdf)

[1]:https://en.wikipedia.org/wiki/Barry_Mazur
[2]:https://www.quora.com/What-is-the-purpose-of-identity-morphisms-in-category-theory
[3]:https://en.wikipedia.org/wiki/Formal_verification
[4]: https://en.wikipedia.org/wiki/Eugenia_Cheng#Early_life_and_education
[5]: https://www.youtube.com/watch?v=ho7oagHeqNc
