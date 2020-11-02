@def title = "Halfway through the MLH Fellowship"
@def published = "2 November 2020"
@def tags = ["mlh"]

# Halfway through MLH Fellowship

The last week has been the most productive for me in terms of getting
contributions there on the upstream repositories. I've created a couple
of PRs [#1454](https://github.com/JuliaDocs/Documenter.jl/pull/1454) and
[#1456](https://github.com/JuliaDocs/Documenter.jl/pull/1456) which adds
some of the most requested features in Documenter.jl project.

## The Documenter.jl project

Documenter is a very interesting project. For some reason before actually
looking at the code, I expected that a static site generator like Documenter
should have a bunch of HTML templates which it would use to then generate
HTML files for the site. But, actually Documenter just uses Julia to write the
HTML part as well. This was actually quite interesting because a site would
have loads of HTML and writing every part of it would just be lengthy, but it
is not! You can easily write a very good site just with a few Julia lines.
You can check how Documenter does it, and I really like how cleanly you can
write a HTML generator or writer.

Apart from that, Documenter does use JavaScript in places where you need them.
For example, the switching between the themes is based on JavaScript but it
does provide a good user experience with just few lines of code and doesn't
affect the performance of the site since there are not any dependencies apart
from jQuery which is also usually just cached in the browser.

## Looking forward for more contributions

So, overall it has been a really productive week because since the start of
the MLH program I haven't been able to focus on the project much because there
is a lot of stuff you can do with them and before you actually start
contributing to any issue you need to play around a bit to know exactly where
the changes are to be made.

I'm still in the process of reviewing the changes I want to introduce in those
PRs. There are some things which requires changes to the internal
escaping of the string content in Documenter. I'm discussing these issues
with the maintainer and soon you should be able to see the copy to clipboard
button there on your documentation site. Also, with small changes to the theme
switching mechanism your documentation will respect the user's color preference
and will auto-switch to the dark or the light mode.

I'm thinking of writing blogs every Monday from now on. So I really hope this
helps me improve my writing skills and at the same time, share some of my
knowledge with the community.

Thanks for reading! :smiley:
