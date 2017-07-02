+++
date = "2017-06-11T09:42:27+02:00"
tags = ["Elm", "Conference", "Development"]
series = ["Elm Europe 2017"]
title = "Elm Europe 2017 day 1"

+++

Ce billet est le premier d'une série de deux consacrée à la conférence Elm Europe 2017.

This blog post is the first of a series of two about the Elm Europe 2017 conference.
<!--more-->

[Go to the English version](#english)


{{< img src="/post/img/elm-europe/logo.png" >}}


Les 8 et 9 juin 2017 se déroulait la première conférence [Elm-Europe](http://elmeurope.org/), à Villejuif, près de Paris.

## Qu'est-ce que ELM ?

Elm est un langage fonctionnel, qui peut se substituer à Javascript.
Il permet de créer des applications robustes et performantes,
dans lesquelles il n'y a pas de surprises en production (fini les "undefined is not a function"). Elm est la source d'inspiration derrière [Redux](http://redux.js.org/), qui est souvent couplé à React.

En savoir plus :

* [Elm lang, le prochain react/redux/angularjs ?](http://vincent.jousse.org/tech/elm-lang-prochain-react-redux-angular/)
* [Pourquoi utiliser Elm ?](https://medium.com/elmlightments/10-reasons-why-you-should-give-elm-a-try-62b56d305643)

Le compilateur Elm est réputé pour la clarté de ses messages d'erreur.
Ils fournissent des indices très pertinents qui aident à mettre au point son code, mais aussi à apprendre le langage pour les débutants.

## La conférence

Les 230 participants se sont retrouvés pendant deux jours dans les locaux de l'Efrei, à Villejuif, près de Paris. Sans surprise, ils étaient en grande majorité jeunes, masculins, venus de l'étranger, et pour beaucoup équipés d'un Macbook. La parité dans ce type d'événement n'est pas pour demain.

Sans surprise non plus, le public était constitué d'enthousiastes. J'y ai rencontré des gens venus de l'étranger, juste pour la conférence, à titre personnel !

Dans la suite de ce billet, je ne vais pas évoquer toutes les présentations, mais seulement celles qui m'ont marqué le plus. Bien entendu cette sélection est très subjective, et reflète sans doute mon statut de débutant en Elm aussi bien que mes centres d'intérêt.

## Keynote - Evan Czaplicki

*Evan a créé Elm dans le cadre de sa thèse de doctorat. Depuis 2016, il travaille chez No Red Ink, qui, après qu'il ait passé quelques années chez Prezi, l'a embauché pour continuer à développer le langage à temps complet.*

Le sujet de sa keynote était : comment faire grossir du code Elm ?

En javacript on recommande de faire des fichiers courts pour éviter des effets de bord involontaires, qui parfois sont difficiles à identifier. Par ailleurs, le refactoring dans ce langage peut être risqué. De plus, il faut choisir la bonne architecture dès le début, sous peine de rencontrer de sérieuses difficultés lorsque l'application va prendre de l'ampleur.

Avec Elm, la probabilité d'introduire des effets de bord involontaires est nulle.
Le refactoring est facile et fun.

Voici quelques recommandations d'Evan :

* construisez vos fichiers autour des structures de données ; choisissez la plus adaptée et ne vous préoccupez pas du nombre de lignes dans le fichier avant qu'il ne devienne vraiment un problème.

* Limitez l'API exposée publiquement. Testez là. Cela facilite le refactoring.

* Evitez les get/set. Les modules cachent les détails tandis que les accesseurs les dévoilent. Utilisez plutôt un [record](http://elm-lang.org/docs/syntax#records).

* Ne faites pas d'over-engineering : pas la peine de rendre générique un module si vous n'en avez pas l'usage.

Evan Czaplicki travaille sur un nouveau livre en ligne : [Functional Programming in Elm](https://www.gitbook.com/book/evancz/functional-programming-in-elm/details)


## Rethinking style - Matthew Griffith

Mettre au point le layout en CSS est souvent pénible.
Il est souvent réparti entre le HTML et les CSS, dans plusieurs fichiers.
Il faut tenir compte de l'arborescence des noeuds dans le DOM, des CSS, des propriétés implicites (telle propriété ne s'applique que si le noeud A est un enfant d'un noeud de type B).

La librairie [Style Elements](http://github.com/mdgriffith/style-elements), développée par Matthew Griffith, simplifie la mise au point de la mise en page en la définissant dans un fichier unique.

De plus elle apporte à l'écriture des styles toutes les garanties du langage ELM, comme la sûreté apportée par le typage fort.

## Elm from a CTO perspective - Sébastien Crème

Sébastien est CTO chez Nomalab. Nomalab est une entreprise parisienne de 10 personnes, qui travaille pour les chaînes de TV. Ils utilisent ELM en production depuis 18 mois.

L'application de Nomalab est constituée de 35 000 lignes de code Elm : une grosse appli de 20 000 lignes côté client, et 15 000 côté serveur. Il s'agit d'une appli complexe pour uploader et afficher des vidéos parfois très lourdes (jusqu'à 500 Go) et leur métadatas.
Cette appli est consitutué de plus de 220 modules.
84.5% de leur code est en Elm, 14.5% en Javascript, 1% en d'autres langages (Rust notamment).
Ils ont découvert ELM en 2016  et y ont vu un gain si important qu'ils ont basculé aussitôt sur ce langage.

Ils continuent à coder des prototypes en JS, pour tester très vite, avec du code de mauvaise qualité qui ne sera pas réutilisé en dehors du prototype.

Leur utilisation de ELM côté serveur est assez unique, mais ils estiment ne pas avoir les moyens de partager correctement leur expérience avec la communauté pour l'instant. Ils n'ont pas les ressources nécessaires pour assurer un support pour du code qu'ils mettraient en open source.

Les avantages de Elm selon Sébastien :

* ils n'ont pas peur de changer les choses
* la sureté apportée par le typage : quand on l'a testée on ne peut plus s'en passer
* Elm est adaptée à la production, y compris pour de grosses applications
* Elm est incroyablement fiable et clair
* une seule journée est nécessaire pour devenir opérationnel en ELM... à condition d'être à l'aise en programmation fonctionnelle !

*Ce dernier point ne semble pas partagé par tous, si l'on en croit les résultats du sondage State of Elm 2017, que j'évoque ci-dessous.*

## State of Elm 2017 - Brian Hicks

Brian Hicks présentait les résultats du sondage "State of Elm 2017". Le nombre de réponses a quasiment doublé en un an : 614 en 2016, 1170 en 2017.

Voici quelques enseignements que l'on peut tirer de ce sondage :

* Elm est évidemment très utilisé pour faire du développement Web, mais aussi un peu pour des jeux —moins toutefois en 2017 qu'en 2016 *(je soupçonne que c'est lié à l'absence de moteur de jeu écrit en ELM)*.
* Une large majorité des répondants a moins d'un an d'expérience avec ELM, ce qui confirme l'intérêt croissant pour ce langage
* plus d'un quart des répondants l'utilisent en production ou sur un projet qui va partir en production
* Les principaux points que les développeurs apprécient à propos d'ELM (par ordre décroissant) :
    * le système de type
    * les messages d'erreur du compilateur
    * le fait que ce soit un langage fonctionnel pur
    * l'architecture ELM
    * sa simplicité
    * "si ça compile, ça marche"
    * l'absence d'erreur à l'execution

* Les principales difficultés rencontrées (par ordre décroissant) :
    * l'apprentissage du langage
    * le décodage JSON
    * les interactions avec Javascript
    * des problèmes avec des packages de l'écosystème.

* Où trouver de l'aide sur ELM ?
    * elm slack
    * elm subreddit
    * elm-discuss mailing-list
    * Elm-town podcast
    * Elm Weekly newsletter

A plusieurs reprises au cours de cette journée nous avons entendu d'excellents commentaires au sujet de l'aide apportée aux débutants par la communautée.

<a name="english">*English version below*</a>

# Elm Europe 2017 - Day 1

The 8th and 9th of June 2017 the first [Elm-Europe](http://elmeurope.org/) conference took place at Villejuif, near Paris (France).

## What is ELM?

Elm is a functional language, which can replace Javascript.
It helps to create reliable and performant applications, in which there is no runtime error (no "undefined is not a function"). Elm inspired [Redux](http://redux.js.org/), which is often used with React.

More info about Elm:
[10 reasons why you should give Elm a try](https://medium.com/elmlightments/10-reasons-why-you-should-give-elm-a-try-62b56d305643)

The Elm compiler is famous for the clarity of its error messages, and the guidance it provides. It helps debugging your apps, but also learning the language when you're a beginner.

## The conference

230 attendees gathered during two days at Efrei, a Computer Science School at Villejuif, near Paris. It was no surprise to me, most of the attendees were young men, coming from foreign countries, and for many of them, equipped with a MacBook. Parity is not for tomorrow in this kind of event.

Without any surprise, the attendance was consituted of enthusiastic persons. I met there some people coming from UK, just for the  conference, on their free time!

In this blog post, I won't speak of all the talks, but only of a selection which probably reflects my current status of newbie in Elm.

## Keynote - Evan Czaplicki

Evan created Elm during his PhD. Since 2016, he works for No Red Ink, which recruited him to work full time on the language development.

The subject of his keynote was: how to make growing Elm code?

He started by reminding us that in Javascript, it is often recommended to write short files, to avoid unwanted and sometimes sneaky side effects.
Morevover, the refactoring in Javascript may be risky. And last but not least, you must choose the right architecture from the start, otherwise you may be damned.

Using Elm, you cannot introduce side effets. The language prevents it. And the refactoring is fun and easy.

Here are some recommendantion made by Evan:

* build your files around data structures ; select the best one to solve your problem, and don't worry about the number of lines in the file, until it becomes a real issue.
* limit the public exposed API. Test it. It makes refactoring easy.
* avoid get/set. Modules hide the details, when the accessors reveal them. Use a [record](http://elm-lang.org/docs/syntax#records) instead.
* don't make over-engineering: it's pointless to make a generic module, when you don't need it.

Evan Czaplicki is working on a new online book: [Functional Programming in Elm](https://www.gitbook.com/book/evancz/functional-programming-in-elm/details)


## Rethinking style - Matthew Griffith

Debugging CSS layout is usually a pain. The layout is often split between several HTML and CSS files. You have to take into account the hierarchy of nodes in the DOM, the CSS styles and the implicit properties (a property applies only if a given node is the child of a node of another given type).

[Style Elements](http://github.com/mdgriffith/style-elements), is a library created by Matthew Griffith. It simplifies the debugging of the layout by defining it in a single file. Moreover, it brings to CSS styles the guarantees offered by ELM (like type safety).

It looks very promising, I definitely will try it!

## Elm from a CTO perspective - Sébastien Crème

Sébastien is CTO at Nomalab. Nomalab is a French startup with 10 employees, whose customers are TV networks. They use Elm in production since 18 months.

Nomalabs' app consists in 35 000 lines of Elm code: 20 000 on client side, and 15 000 on server side. It's a complex application to upload big video files (up to 500Go), view their metadata and insert ads in the video stream.
This app is made of more than 220 Elm modules.

84.5% of their code is written in Elm, 14.5% in Javascript, and 1% in other languages (Rust in particular).
They discovered Elm in 2016 and have seen so important benefits they switch almost immediately from JS to Elm.

They continue to code some prototypes in JS, to validate quickly an idea, but this JS code is never reused in production.

Their usage of Elm on server side is quite unique, but they don't feel ready to share it with the community. It would require human resources they do no have, in order to provide support if they open sourced anything.

The pros of Elm according to Sébastien are:

* they don't fear to change things
* type safety: once you taste it, you can't do without it
* Elm is ready for production, included for large applications
* Elm is incredibly reliable and clear
* one day is enough for a developer familiar with functional programming to be operational with Elm.

*This last point does not seem to be shared with everybody, as we'll see with the next talk.*

## State of Elm 2017 - Brian Hicks

Brian Hicks presented the results of the "state of Elm 2017" poll. The number of responses nearly doubled in one year: 614 in 2016, 1170 in 2017.

Some lessons we can draw from this poll:

* Elm is of course mostly used to make Web development, but sometimes to make games too -less in 2017 than in 2016 *(I guess this is linked to the lack of game engine in Elm)*

* most of the respondents have less than one year of experience with Elm,
*which confirms the growing interest for the language*
* more than one quarter of respondents use it in production, or on project which will go to production.

* The main points people love in ELM (by decreasing order):
    * type system
    * the error messages of the compiler
    * it is a purely functional language
    * the Elm architecture
    * it simplicity
    * "if it commpiles, it works"
    * no runtime error

* The main difficulties with Elm (by decreasing order):
    * learning the language
    * decoding JSON
    * interactions with Javascript
    * issues with packages of the ecosystem.

* where to find help about Elm?
    * elm slack
    * elm subreddit
    * elm-discuss mailing-list
    * Elm-town podcast
    * Elm Weekly newsletter

*Several times during this conference, we heard great feedbacks about the help provided by the community to newbies.*