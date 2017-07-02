+++
date = "2017-07-02T17:42:27+02:00"
tags = ["Javascript", "Framework", "Elm", "Riot", "Marko", "Development"]
title = "The best JS Framework"
+++

{{< img src="/post/img/The-Best-JS-Framework/a-new-javascript-framework-is-coming.png" >}}

Every now and then, a new JS framework arises. Every two days it seems.
Some of them gain much more traction than others, like React, Angular or Vue.
Most of them have a much more limited popularity.

The authors of each of them claim it's greater than others.
They create benchmarks to prove their framework is the fastest.
They compare some of them from multiple points of views:
technical, feature list, syntax, performance, weight... to support this assertion.

The result is all of them is the best JS framework!
For us, as developers, it's fantastic: we have a large choice of JS frameworks,
all of them being greater than the others.

So when it comes to make a choice, which one should you choose?
The most popular one? The fastest? The lightest? The most feature complete?
My answer is: *choose the one that matches your philosophy*.

## My past choices

After a few years developping Flex apps –please don't throw me rotten tomatoes,
I still think it was a good solution at this time, when Web browsers behaved so differently
and the Flash runtime security issues were not such a big concern–,
I came back to JS with JQuery, then rapidly switched to AngularJS.
I sticked to AngularJS a few years, during which some concerns grew up.
First of all, I always found repulsive the syntax of directives. And the performance issues,
especially on mobile, led some people to give recommendations like: don't use double binding,
avoid filters, avoid X and Y and Z... This lead me to wonder
where was the interest of continuing to use this framework, when the best practices were not to use what I loved in it at first?
The learning curve of AngularJS was a problem too: after a 4 or 5 days training,
I did not felt at ease with the framework, and it was the same for my workmates.
Moreover, I felt trapped in the AngularJS ecosystem. It was not easy to use components
which were not specifically designed for AngularJS. You had to use a wrapper,
when there was one, and hope the wrapper will follow the component's evolutions.
Like always with big frameworks, you end up struggling with the issues created by the framework itself,
in addition to the issues of your own code.

With all of this in mind, I started a couple of years ago to look for an alternative to AngularJS.
I read a lot of articles, tested the frameworks which looked the most promising,
but was often disappointed. Until...

## RiotJS

At the begininng of 2015 I discovered RiotJS 2. Most of what was important to my eyes was there:

* an elegant and intuitive syntax, close to HTML standard
* little or no boilerplate code
* a short learning curve
* a small size, synonym of short loading time
* compatibility with most JS libraries
* understandable internal code
* an active community
* backed by a company (Muut) using it in production

Except the last two, these points reflect what I don't like in Angular, React, Vue, Aurelia, Ember and other major frameworks.

Of course, Riot was not perfect. Like all frameworks, it had issues.
Many of them were addressed by Riot 3 at the end of 2016, although this release came with its own problems.

I developped some applications with it, both side projects and professional ones,
and was pleased until recently.

The problem with Riot is the team behind it seems to have shrinked to one person,
who leads the project with quite a dictatorial attitude,
as you can see by reading the discussions about [Riot V4 roadmap](https://github.com/riot/riot/issues/2283):
*"I don't accept critics on my behavior and/or on the way I am managing this project."*

I progressed a lot by listening to this kind of criticisms, so I can't agree less.

Moreover, this kind of behavior lead several open source projects to death or nearly-death –everybody is not Linus Torvald.
So I'm quite concerned by the future of Riot.
The last discussions were more constructive, and it looks like contributions won't be limited to one person, so I'm a bit more optimistic than a few weeks ago, but I'm still wary.

## Using a "minor" framework

RiotJS is not as popular as Angular, Vue or React, far from it.
When I say I use this kind of framework, I'm often told this is a problem because it's easier to find an Angular or React developer to complete a team.
That would be an issue if learning to use this kind of micro-framework was not so easy.
If the documentation is correct, any JS developer can start to work with it in less than one day.
So the difficulty of finding trained developers is a false problem.

Another common negative observation I hear is the lack of UI components. Having specific UI components is mandatory when the framework manipulates the DOM in a non-conventional way, making it incompatible with any other library. Once again, it's a solution to a problem created by the framework itself. For some of them, like Riot, this is a non-issue.

There are a few UI components library based on Riot, but the last time I checked, none of them was actively maintained. This could be an issue if Riot was not easy to mix with most of CSS frameworks. I used it in combination with Semantic-UI on two projects by example.

A notable exception is Bootstrap, which sometimes expects a specific hierarchy of Dom nodes and so doesn't fit well with custom tags. I assume this is an issue with all of UI frameworks.

On a project I'm currently working on, we have to display a huge table (30 columns, tens of thousand of lines), with sorting and filtering options, and specific formatting for the content of some columns. For reasons outside of this article's scope, pagination of data by the server was not the best solution in this case. There are many advanced table JS components, but only [SlickGrid](https://github.com/6pac/SlickGrid/wiki) matched our needs. Integrating it with Riot was a no-brainer.
So I prefer a micro-framework, with which I'm free to use any additional component, to being limited to some specific components.

## React License

A specific issue with React is the [additional grant of patent rights](https://github.com/facebook/react/blob/master/PATENTS) Facebook gave it. I work for a big company, and our lawyers would not accept for us to use a piece of software under such terms.

## Elm

In parallel with this, I looked at Elm during the last months. Elm is a Purely Functional Programming language
which compiles to Javascript. It is used in production on big applications, solves several issues of Javascript, is fun to develop with, and produces fast and robust code.

I began to learn this language two months ago, and I just started to rewrite in Elm a small JS app I want to extend.

Elm is full of qualities, but the learning curve is far from short, especially if you're new to Purely Functional Languages, like I am.
Although I think it could be a great solution for my professional projects, I'm afraid this learning curve and the change of paradigm will be a no-go for some of my colleagues. I do not feel ready yet to ask them to give Elm a try. Maybe in a few months, when I feel more confortable.

## Marko

It's in this context that I discovered [Marko](http://markojs.com/) a few days ago.
I found in Marko all the good points I've seen in RiotJS, and more.

The Ebay home page and mobile app use it, and that's a hell of a reference.
It's fast, light, supported by a big company. Isomorphism is supported out of the box, in a simple way.

The syntax is much more elegant and concise than the one of any other major framework, and that's very important to me.
After nearly 35 years of programming, readability and simplicity are crucial to my eyes.

I've not tested it yet, but it's the first framework which really caught my attention since Riot.
Considering it is supported by Ebay, which has much more visibility than the company behind Riot,
I think it has the potential to become one of the major JS frameworks in the next years.

So is Marko the best JS framework? It could be... According to my criteria at least.

