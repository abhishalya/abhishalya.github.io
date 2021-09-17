@def title = "New blog design"
@def published = "17 October 2020"
@def tags = ["design"]

# New blog design

New blog is up and running now! I always wanted to improve my blog and
portfolio site and it is finally ready now. The best part is this site is
generated using [Franklin.jl](https://github.com/tlienart/Franklin.jl) package.
I really like how Franklin has developed over the time and it really feels like
a solid static site generator now.

I'm always up for anything related to Julia and web-development.
Here are some functionalities which I wanted to test.

# Code blocks

Here are some of the code blocks

```julia
using Franklin

# some comment here
newsite("My new site name"; template="hyde")
```

```py
from django import models

class TestModel(models.Model):
    """
    This is some random docstring
    """
    pass
```

This is some inline $\LaTeX$.

And this is some fancy latex block.

$$
k_n = k_{n-1} + k_{n-2}
$$

This is a [test link](https://bhushankhanale.com) for testing GA.
