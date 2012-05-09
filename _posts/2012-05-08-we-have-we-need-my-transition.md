---
layout: post
title: "We Have We Need: My Transition From Rails to Django"
category: 
tags: []
---
{% include JB/setup %}

**We Have We Need** intends to be a Craigslist for medical supplies targetted at Humanitarians and NGOS.
Currently led by Jon Wong ([@jnwng](http://twitter.com/#!/jnwng), it's a project
that saw its origins about 30 weeks ago, is built by a team of students during their
offtime, and an overall awesome attempt at solving the problem of supply deficiencies due to
a lack of communication and supply management in regions of the world that 
suffer from frequent disease outbreaks.

I [@_lewisf](http://twitter.com/#!/_lewisf), along with some other students joined 
[We Have We Need](http://www.wehave-weneed.org) about 5 weeks ago out of interest in the 
project and admittedly Jon doing  a good job recruiting me to the project. I was 
partially heartbroken when I found the project to be in Django (as I had no prior 
Django experience) but the experience of learning Django has been pleasant, probably 
because of some of its similarities to Rails and the experience of those around me.

Here are just a couple things I noted on my switch from Rails to Django over the course
of the last 5 weeks.

- **Asset Pipeline.** Rails 3 has spoiled me with the built in asset pipeline, and the
advantages it presents developers with - most importantly stylesheet, script organization.
Combined with preprocessors like SASS and CoffeeScript, Rails' Asset Pipeline provided me
with a painfree experience in dealing with having my scripts/styles available on the pages
that need them. To get the something similar in Django, I plugged in 
[django-mediagenerator](http://www.allbuttonspressed.com/projects/django-mediagenerator),
which with little configuration allows me the freedom of developing with preprocessors without 
having to run sass or coffeescript watchers, or having duplicate style/script files. Unfortunately
the documentation of mediagenerator seems to be everywhere. Unfortunately, there
doesn't seem to be a lot of activity around mediagenerator anymore, so we may consider moving
to another asset pipeline.
- **Template variables.** Gotten used to throwing template variables into a dictionary that is returned
in the methods in views.py. In Rails, it was convenient to just use `@var=5` and render
it in the template as `<%= @var %>`. In Django, we have to return a dictionary `{ var: 5 }` 
in order to render it in the template.

- **Less Magic.** Rails has a lot of magic. Django, on the otherhand, much less so.
As an example, routing definitions in Django can involve multiple urls.py's where 
I have to write regex's to do url matching and mess with a bunch of options. Admittedly, 
they are simple regexes, but Rails' notion of defining things in single routes.rb through
pre-defined helper functions through `resources` is much easier.

- **ActiveRecord vs Django Query Language.** A lot of purists probably enjoy writing raw
queries, but when it comes to the simple queries, I like expressive query languages. Unfortunately,
Django's query language falls short to ActiveRecord in my opinion. I find myself writing a lot of
User.objects.all - when in Rails it's pretty much implied that I want to query through all User objects.
This is just a minor complaint, but this is just one among many others.

- **Python imports.** I have to import everything. This is annoying because it gets messy fast, and I
forget imports when writing new code way too often. Having the magic of Rails' automatically resolving
dependencies at runtime is great.

So I still love Rails, but Django has been very eye-opening in terms of how frameworks work (or don't work). 
In fact, learning Django has gotten me to better understand Rails in a weird way, and to better appreciate 
the decisions Rails has made for the sake of developer happiness. I'm probably still going to 
default to Ruby on Rails for my personal projects, but I'd be fine working in Django if the need
ever arose again in the future.
