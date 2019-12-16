---
layout: post
title:  "Developing apps using Interact.jl"
date:   2019-12-16 00:00:00
categories: blog-post
---

I think many of you might think that it is quite impossible or hard to develop
a web-app in Julia. Well, you are wrong! Developing a web-app using Julia is
very much possible and is easy too. This post will give you a brief guide to
how you can develop you apps using
[Interact.jl](https://github.com/JuliaGizmos/Interact.jl) and
[WebIO](https://github.com/JuliaGizmos/WebIO.jl).
This blog post is also a submission to one of my Google Code-in tasks at Julia.

## Where are the docs?

Well, Interact is a great package but one of the things it lacks is the
proper documentation and examples which are really important which you try
to build your own app. The existing documentation is probably only good enough
for widgets but many of the functions are missing there. One of the reason is
Interact is build upon WebIO, CSSUtil and other packages where each one has its
own documentation. So if you don't find something in Interact chances are it
will be somewhere else. Just doing a Github search would get you to the source
:P

But hopefully, this post will give you all the basics you'll
need to know in order to successfully develop your app at one place.
This might not cover all there is but this should at least get you started.

## Getting Started

Before we move on to using these packages, we first need to make sure we have
everything we need.

Interact works with the following frontends:
- [Juno](http://junolab.org/) - A flexible IDE for the 21st century
- [IJulia](https://github.com/JuliaLang/IJulia.jl) - Jupyter notebooks
(and Jupyter Lab) for Julia
- [Blink](https://github.com/JunoLab/Blink.jl) - An Electron wrapper you can
use to make Desktop apps
- [Mux](https://github.com/JuliaWeb/Mux.jl) - A web server framework

You can use any one of these. I'll be working with IJulia and Mux here.

For IJulia, you need to make sure you have Jupyter notebook installed along
with nbextensions.

You can just do:

```
pip3 install jupyterlab --user
```

I avoid using `sudo pip` and you should too in my opinion.

Next, install the nbextensions

```
pip3 install jupyter_contrib_nbextensions
jupyter contrib nbextension install
```

And finally install the WebIO Jupyter notebook extension in REPL:

```
julia> ]
(v1.3) pkg> add WebIO
```

```jl
using WebIO
WebIO.install_jupyter_nbextension()
```

Now if everything is goes fine, you can move towards next step.

## Interact.jl - An example

Interact provides a
[set of widgets](https://juliagizmos.github.io/Interact.jl/latest/widgets/) you
can include in your app. Also, you can create you own
[custom widgets](https://juliagizmos.github.io/Interact.jl/latest/custom_widgets/)
if you want to. Here we will only focus on the available widgets.

So, here we will be trying to replicate the UI of the
[DiffEqOnline](http://app.juliadiffeq.org/sde) app. We can see that the UI
contains text input fields, numerical inputs and a dropdown menu. All of which
we can implement using the available widgets of Interact as follows:

```jl
# Textarea for the input equations (for multiline input)
input_eqn = Widgets.textarea(label = "Enter the system of differential equations here:",
                             value = "dx = a*x - b*x*y\ndy = -c*y + d*x*y")

# Textbox for the input parameters
input_param = Widgets.textbox(label = "Parameters:",
                              value = "a=1.5, b=1, c=3, d=1")

# Textarea for input noise (for multiline input)
input_noise = Widgets.textarea(label = "Input the noise function here:",
                               value = "dx = a*x\ndy = a*y")

# Textbox for the noise parameters
noise_param = Widgets.textbox(label = "Noise parameters:",
                              value = "a=0.25")

# Since we only accept numerical values for the time span we can
# use the spinbox. (we can also specify the range for spinboxes)
time_span_1 = Widgets.spinbox(label = "Time span:", value = 0)
time_span_2 = Widgets.spinbox(value = 10)

# Textbox for the initial conditions
initial_cond = Widgets.textbox(label = "Initial conditions:",
                               value = "1.0, 1.0")

# Textbox for the plotting variables
plotting_var = Widgets.textbox(label = "Plotting variables",
                               value = "[:x, :y]")

# To create a dropdown menu, we need a dict with the keys and associated values
# to select the options within it.
dict = Dict("SRIW1: Rossler's Strong Order 1.5 SRIW1 method" => 1,
            "SRA1: Rossler's Strong Order 2.0 SRA1 method (for additive noise)" => 2)

plotting_var = Widgets.dropdown(label = "Solver:", dict, value = 2)

# Textbox for the graph name
graph_title = Widgets.textbox(label = "Graph title:",
                              value = "Stochastic Lotka-Volterra Equation")

# Creates a button with name "Solve it"
solve_but = button("Solve it")
```

Now, since we've got all the elements we needed, we can just create a UI element
by stacking them over one another.

We'll use `vbox` to vertically stack all the elements. You can use `hbox` to
horizontally stack elements. Also, to make it look better we will append a
horizontal line between each element and a vertical margin of 20px using
`hline()` and `vskip(20px)` respectively.

So, the final result should be something like this:

```jl
ui = vbox(vskip(20px), input_eqn, vskip(20px), hline(),
          vskip(20px), input_param, vskip(20px), hline(),
          vskip(20px), input_noise, vskip(20px), hline(),
          vskip(20px), noise_param, vskip(20px), hline(),
          vskip(20px), time_hor, vskip(20px), hline(),
          vskip(20px), initial_cond, vskip(20px), hline(),
          vskip(20px), plotting_var, vskip(20px), hline(),
          vskip(20px), graph_title, vskip(20px), hline(),
          vskip(20px), solve_but)
```

Now, if you're running all this code you'd see that the elements are already
styled. This is because Interact uses 'Bulma' CSS for the styling. We can
modify this, but it is a topic for some other post.

So far we've got the user-interface we needed. Now, how to record the values
and work with them. To understand that, we'll need to understand what 
are `Observables`.

## Observables

Observables are like `Ref`s but you can listen to changes.

As an example:
```jl
using Observables
obv = Observable(0)

on(obv) do val
    println("Value changed to: ", val)
end
```

So if we do:
```
obv[] = 10
```
Then the output will be:
```
Value changed to: 10
```

So, for the above example we need to construct an observable for each of the
elements we just created. I'll define a new function `make_observable` to do
this. But before that let's define a scope object to enclose the observables.

```jl
scope = Scope()
```

A `Scope` acts as a medium for bidirectional communication between Julia
and JavaScript. The primary method of communication is `Observables` which are
essentially wrappers around values that may change over time. A `Scope` may
contain several observables whose values can be updated and read from either
JavaScript or Julia.

So the `make_oservable` function will rely on a unique key which we will provide
for each of the elements we just constructed. So, in order to do that, we will
set an Observable object to each of the elements' value. What this will do is,
it will record the values of each of these elements. And we will trigger the
function which we want to run (the work to be done on the given values) on
a click of the `solve_but`.

So, to do this we might do something like this:

```jl
function makeobservable(key, val = get(dict, key, nothing))
    scope[key] = Observable{Any}(val)
end

input_eqn = Widgets.textarea(label = "Enter the system of differential equations here:",
                             value = makeobservable("input_eqn"))

input_param  = Widgets.textbox(label = "Parameters:",
                               value = makeobservable("input_param"))

input_noise  = Widgets.textarea(label = "Input the noise function here:",
                                value = makeobservable("input_noise"))

# Do this for all elements in a similar way
```

Finally, for the button we need an observable for counting clicks. We can do
that like this:

```jl
clicks = scope["clicks"] = Observable{Any}(0)
```

Now, we need to provide some initial data for all of the elements. So, we will
construct a dict with the keys for each of the element and values set to the
initial values of their corresponding elements.

```jl
const init_dict = Dict(
    "input_eqn" =>"dx = a*x - b*x*y\ndy = -c*y + d*x*y",
    "input_param" =>"a=1.5, b=1, c=3, d=1",
    "input_noise" => "dx = a*x\ndy = a*y",
    "noise_param" => "a=0.25",
    "time_span_1" => 0,
    "time_span_2" => 10,
    "initial_cond" => "1.0, 1.0",
    "plotting_var" => "[:x, :y]",
    "solver" => 1,
    "graph_title" => "Stochastic Lotka-Volterra Equation",
)
```

Finally, we will construct a dict containing all of the form input elements like
this:

```jl
form_input = Observable{Dict}(dict)
form_input = makeobservable("form_input", init_dict)
```

Finally to update the `form_input` on the click, we can do something like this:

```jl
form_contents = Dict(key=> @js $(scope[key])[] for key in keys(init_dict))
onjs(clicks, @js () ->$form_input[] = $form_contents)
```

We will call the function we want to work with by sending the `form_input` as
an argument and appending the output to the `ui`.

To use Mux.jl to serve the web-page we can simple do:

```jl
]add Mux

using Mux
WebIO.webio_serve(page("/", req -> ui), 8488)
```
Here the number 8488 is the port number, you can use any port you want. After
this you can simply open the browser and redirect to `localhost:8488` or any
other port number you used and you should see the UI just created.

This completes the introductory blog post on how you can create a web-app
using Interact and WebIO. I hope it was helpful for you somewhat to make
your own apps. You can use all of the available documentation mentioned below
to get more details.

## Thanks

A huge thanks to [@sashi](https://github.com/shashi) and
[@logankilpatrick](https://github.com/logankilpatrick) for helping me out
throughout my tasks. :)

## References

1. https://juliagizmos.github.io/Interact.jl/latest/
2. https://juliagizmos.github.io/WebIO.jl/latest/
3. https://juliagizmos.github.io/Observables.jl/latest/
