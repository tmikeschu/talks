# Keeping Your Views in the Dark: Presenter Pattern

## abstract

HTML and JSON have one responsibility: present data. You may have heard of
either described as a "presentation layer". Notice that it's not called a
"presentation-with-a-bit-of-logic layer". I'm looking at you, person who
assigned a variable in an .erb template!

Of course, sometimes you are in a pinch and you need to use some logic in your
view -- but there is a better way! In this talk, we explore the presenter
pattern to achieve the famous "single instance variable" goal in a Rails action
and corresponding view. We'll talk briefly about the theory behind it and
spend most of our time making presenter objects in both green- and brownfield
contexts. You will learn how to use the presenter pattern to make your views more
declarative and your workflow more efficient.

## details

The intended audience member is an intermediate Rails developer who develops
HTML or JSON in their application.

Outline:

Personal intro: 1 minute
Overview of presentation layers: 5 minutes
The presenter pattern: 5 minutes
Greenfield presenter: 10 minutes
Brownfield presenter: 10 minutes
Testing presenters: 4 minutes
Review: 5 minutes

Outcomes:

1. identify opportunities to remove logic from .erb templates
2. implement the presenter pattern to separate logic from views

## pitch (Explain why this talk should be considered and what makes you qualified to speak on the topic.)

Isolating responsibilities is super helpful for developing workflow momentum.
Getting lost trying to parse variable assignment when you're really looking for
a CSS class is unnecessary overhead. I think the presenter pattern is a
fantastic way to level up as an intermediate Rails developer.

As a speaker, I'm very energetic and compassionate. With a natural and genuine
love for learning, I often get feedback that my talks are very engaging. I have
experience teaching adults and I've given talks at various meetups around Denver
and Detroit, as well as RailsConf and self.conference. I also take a lot of
initiative at work to present lunch and learns.

## bio

Mike Schutte is always uncomfortable describing himself in third person. He's a
solutions engineer at Clinc and lives in Detroit. In 2016, he pivoted his career
aspirations from sociology to software development. He has a deep passion for
learning, teaching, and writing elegant code for software that improves everyday
life. The pursuit of single responsibility sparks great joy for him, and the
presenter pattern is no exception!
