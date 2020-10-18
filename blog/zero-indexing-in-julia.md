@def title = "0-based Indexing in Julia"
@def published = "07 December 2019"
@def tags = ["julia", "code-in"]

# 0-based Indexing in Julia

This is one of the submission blog post for the ongoing [Google Code-in
competition](https://codein.withgoogle.com/). This year I was mainly
concentrating on Julia tasks. I think you get to learn much more when you
stick to one org. While looking through the tasks I found this one where we
have to explain that we can do 0-based indexing in Julia.

For those who are not familiar with Julia, it has 1-based indexing. Hence, all
the arrays, vectors start from 1 rather than from 0 (as in many other
programming languages). Initially, I didn't think if it was much of a deal,
since I never actually thought people would judge the programming language based
on whether it has a 0-based on 1-based indexing.

But, to my surprise I saw [the comments on this post on
reddit](https://redd.it/e71l4y) and that made up my mind to do this task.
So, there are a lot of things Julia docs says about indexing, and yeah sometimes
it might just be possible that the 0-based indexing is better then 1-based.

I'm just writing here about how you can implement 0-based indexing using
[OffsetArrays.jl](https://github.com/JuliaArrays/OffsetArrays.jl).
Along with the relative indexing it offers many other features which you may
want from Julia but are not available out of the box.

For this task, we are concerned about the 0-based indexing and in order to do
that you can simply declare an OffsetArray in the following manner:

```julia
julia> using OffsetArrays

julia> x = OffsetArray([1, 2, 3], 0:2)
3-element OffsetArray(::Array{Int64,1}, 0:2) with eltype Int64 with indices 0:2:
 1
 2
 3
```

Here I'm just creating a vector with the values `1, 2, 3` with a relative
indexing of 0 to 2. Hence, if I ran:

```julia
julia> x[0]
1
```

or if I ran:

```julia
julia> x[2]
3
```

or even this:

```julia
julia> x[end]
3
```

it would work just as you'd expect from a 0-based indexed vector.

I feel this blog post was more designed to make people aware that they **can**
do 0-based indexing if they want. There are many packages which can help you
with your task.

I hope this helped. Thank you :)
