@def title = "1-based vs 0-based Indexing"
@def published = "07 December 2019"
@def tags = ["julia", "code-in"]

So, in the previous post I mentioned how we can use Julia to do arbitrary or
0-based indexing. In this post, we will talk about what are the differences
between the two indexing types and why is it such a big deal. (It's not as
you will eventually see).

First, what these indexing types mean?

The 0-based indexing considers the first element in a container to be at
index = 0, i.e. if we declare an array:

```julia
x = [1, 2, 3]
```

then the first element of `x` would be accessed by `x[0]`, second element by
x[1] and so on.

In 1-based indexing method, this same element would be accessed by `x[1]`,
second element by `x[2]` and so on.

And that's it! That is all the difference that exists between the two indexing
types. Now, people rant about the 1-based indexing being the worse of two
and I have no reason to support that. Mostly, I think these people are only
thinking of their personal experiences of using either of these two types.

To wrap it in two cases:

1. A person is writing a program which uses a large array as input;
The array is quite large and he mistakenly writes `0:n` instead of `0:n-1`
considering 0-based indexing and `n` being the number of elements in the array.
He waited for program to finish; Found out an error at last point:
`Array Index out of bound!`.
Finds himself 1-based indexing to be much better.

2. Another person writing some program has a habit of using 0-based indexing.
Makes mistake every time he tries to write programs using 1-based indexing.
Screams out loud ranting about 1-based indexing.

These are just random examples, and are completely made up. You might personally
face issues in one if you are more acquainted with the other.
You can hear about some people mentioning about the address space of processors
being linearly starting from 0, and it was there is microcontrollers as so on.

From what I've heard people prefer the 1-based indexing because it corresponds
natural language (i.e. first, second, etc but there is no zeroth) to start from
it. While people considering 0-based indexing consider the indices to be
offsets as to they way CPU calculates the absolute memory location. Offsets
start from 0 and it suddenly it becomes only logical to have 0-based indexing.

But personally, I think there's not much to talk about here. I think its just a
personal preference since here we are only working with high level code and
this issue would never make it to the lower levels based on what indexing type
you choose.

Also, if you want to use 0-based indexing in Julia, checkout my
[previous post](https://abhishalya.github.io/blog-post/2019/12/07/zero-based-indexing.html)
which would let you do that.

I hope this might have cleared up few things about the indexing types. Hope
it helps you in some way :)
