+++
date = "2017-06-11T22:42:27+02:00"
tags = ["Elm", "Conference", "Development"]
series = ["Elm Europe 2017"]
title = "Elm Europe 2017 - Day 2"

+++

Ce billet est le second d'une série de deux consacrée à la conférence Elm Europe 2017.

This blog post is the second of a series of two about the Elm Europe 2017 conference.
<!--more-->

{{< img src="/post/img/elm-europe/logo.png" >}}

[Go to the English version](#english)

## Keynote jour 2 : Richard Feldman

Quand le code d'une application grossit, on peut se heurter à des problèmes :

* difficulté à localiser le code qui réalise une fonction donnée
* le code devient trop gros pour "tenir dans la tête"
* duplication de code


Pour répondre à la difficulté de localisation, une réponse est d'organiser son code. Richard a publié sur github un [exemple d'organisation d'une Single Page Application](https://github.com/rtfeldman/elm-spa-example).

Concernant le problème de représentation mentale, Richard conseille de façon très classique de découper le code, et de faire en sorte qu'il réponde à une seule problématique à la fois, mais aussi de spécialiser les types.

Par exemple si un message comportent trop d'alternatives :

```
type Msg
    = A
    | B
    | C
    | D
    | E
    | F
    | G
    |... 18 autres messages
```

et si les types de A à F sont liés à la notion de Flux, vous pouvez réorganiser votre code de la façon suivante :

```
type FeedMsg
    = A
    | B
    | C
    | D
    | E
    | F

type Msg
    = FeedMsg
    | G
    | ... 18 autres messages
```

Cela a de plus l'avantage de permettre de spécifier avec plus de précision le type de paramètre attendu par certaines fonctions.

Une troisième astuce qu'il recommande est d'écrire des fonctions qui prennent des [extensible records](http://elm-lang.org/docs/records) en paramètre.

Richard recommande la lecture de ["Data structure in elm"](http://tech.noredink.com/post/140646140878/data-structures-in-elm) par Tessa Kelly.

## A tool for telling interactive stories - Jeff Schomay

Jeff travaille chez Pivotal tracker, qui utilise du code Elm en production et en est très satisfait. On trouve d'ailleurs cette citation sur la page d'accueil du site [elm-lang.org](http://elm-lang.org) :

> "We’ve had zero run-time failures, the filesize is ridiculously small, it
> runs faster than anything else in our code base. We’ve also had fewer bugs...
>
> To sum it up, our manager has mandated that all new code be written in Elm."

Jeff a développé un [moteur d'histoires interactives](http://elmnarrativeengine.com/) en mode texte.

Il a utilisé le pattern [Entity Component System](https://en.wikipedia.org/wiki/Entity_component_system) pour rendre l'application configurable. Grâce à ce pattern, il a séparé complètement la logique, la présentation et le contenu de son application.

Pour implémenter ce pattern, il s'est basé sur des dictionnaires dont les clés sont des chaînes de caractères. Il y a gagné en flexibilité, mais perdu en sécurité le compilateur ne peut pas faire de vérification sur les chaînes de caractères, et détecter par exemple une faute de frappe.

Le fait d'avoir mis en place le pattern ECS lui a permis d'implémenter [trois IHM très différentes](http://blog.elmnarrativeengine.com/sample-stories/) pour la même histoire interactive. Il a notamment créé une version de son jeu en utilisant son moteur écrit en Elm avec le framework JS Phaser. Le couplage des deux technologies n'a pas posé de problème particulier.

## Manipulating time in Elm - Vincent Billey

Vincent présentait le package [rluiten/elm-date-extra](https://github.com/rluiten/elm-date-extra), qui compense certaines lacunes du package date standard, comme l'absence de support pour l'internationalisation.

Le contenu de cette présentation est détaillé sur le [blog de Synbioz](https://www.synbioz.com/blog/time-manipulation-in-elm)

## Persistent collections: How they work, and when to use them - Robin Heggelund Hansen

Robin a présenté les princpales structures de données en ELM, avec les cas d'usages auxquels elles sont le mieux adaptées, en fonction de leur implémentation. Les collections immutables ont des challenges en termes de performance qui diffèrent de ceux de leur équivalent mutable.

Chaque élément d'une liste pointe sur le suivant. Les listes sont idéales :

- pour travailler sur le premier élément de la liste
- pour un parcours des éléments de gauche à droite
- quand la chose la plus importante à faire est d'ajouter des éléments à la liste

Pour les autres usages, les listes ne sont pas optimisées. Par exemple pour certaines opérations, comme map, il va falloir parcourir la liste dans un sens, puis dans le sens inverse pour remettre les valeurs dans l'ordre initial.

Dans l'implémentation des dictionnaires, chaque élément pointe sur deux voisins : un avec une clé de valeur inférieure, l'autre avec une clé de valeur supérieure. C'est ce qui permet de naviguer dans le dictionnaire.
Les dictionnaires ont les caractéristiques suivantes :

- l'accès à une donnée via sa clé est rapide (recherche binaire)
- chaque clé est unique
- les valeurs sont triées en fonction de leur clé
- ils requièrent que les clés soient comparables

En Elm, on ne peut pas étendre la définition de "comparable", ce qui peut limiter l'emploi des dictionnaires.

Un ensemble (Set) est similaire à un dictionnaire dont les valeurs sont comparables. L'implémentation est donc assez similaire à celle du dictionnaire.

Les tableaux (Array) sont implémentés comme des arbres dont les feuilles sont des sous-tableaux de 32 élements. Cette implémentation possède les avantages suivants :

- l'accès en lecture ou en écriture à n'importe quelle valeur du tableau est rapide, grace à l'index
- les tableaux sont rapides [comparés aux autres structures de données du langage] lorsqu'il s'agit d'ajouter un élément à la fin
- les fonctions map, filter, foldr et foldl sont également rapides

Robin recommande d'éviter les conversions juste pour bénéficier d'une opération plus rapide (par exemple convertir une liste en tableau pour exécuter map, puis revenir à une liste), et de faire à la place des compromis sur l'utilisation de la structure existante.

Elm nous guide dans le choix des structures de données en n'exposant pas d'opérations inefficaces, comme get/set sur des listes.

Robin a écrit une implémentation des tableaux plus efficace pour la recherche, et qui corrige certains bugs, implémentation qui sera intégrée dans la prochaine version de Elm.


## Elm from a Business Perspective - Amitai Burstein

Amitai Burstein était le second CTO à venir sur scène pour justifier le choix de Elm pour les développements dans son entreprise. Il nous a expliqué que leur application écrite en Elm comporte beaucoup moins de bugs que leur application Angular.

D'après lui, avec Elm vous aurez de meilleurs résultats financiers, moins de bugs et des développeurs plus heureux :)

## Elm Native UI in Production - Josh Steiner

[Purple Train](https://purpletrainapp.com/) est une application mobile simple, écrite initialement avec React Native.
Les développeurs de [thoughtbot](http://thoughtbot.com) ont beaucoup apprécié cette techno qui leur a permis de développer trois fois plus vite qu'avec des technos natives iOS et Android, notamment grâce au hot-reloading.
Mais React-Native possède un inconvénient majeur d'après Josh : il est basé sur Javascript. Avec React UI les erreurs à l'exécution sont très fréquentes (Red Screen of Death).


Elm Native UI s'appuie sur React Native, mais avec Elm Native UI, il n'y a plus d'erreur à l'exécution.

Naturellement, Elm native Ui n'est pas sans défaut. Ce n'est pas une solution prête pour la production. Elle est encore incomplète et ajoute une couche d'abstraction supplémentaire.

[Plus d'infos](https://robots.thoughtbot.com/elm-native-ui-in-production)

## Conclusion

La conférence était très agréable. Il s'en dégageait beaucoup d'énergie du fait de l'enthousiasme général pour le langage. Il y avait bien quelques petits problèmes d'organisation liés sans aucun doute au fait qu'il s'agissait d'une première (pas de thé pendant les pauses notamment !), mais nul doute que cela va aller en s'améliorant. Thibaut Assus, le principal organisateur, a déjà annoncé que l'édition 2018 se déroulerait également à Paris.

Quant à moi, je vais attaquer mon premier projet personnel en ELM très rapidement !


<a name="english">*English version below*</a>

# Elm Europe 2017 - Day 2

## Keynote day 2 : Richard Feldman

When an application grows, we may encounter some issues:

* it may be difficult to identify the location of the code for a given feature
* the code becomes "too big to fit in the head"
* duplicated code


To address to the first issue, an answer is to organise the code. Richard put online [an example of code organisation for a Single Page Application](https://github.com/rtfeldman/elm-spa-example).

About the mental representation of the code, Richard made a very classical recommendation: split your code, and focus on one problem only. But he also add to specialize types.


By example if a message type has too many alternatives:
```
type Msg
    = A
    | B
    | C
    | D
    | E
    | F
    | G
    |... 18 other messages types
```

and if A to F types are related to the notion of Feed, you can refactor your code like this:

```
type FeedMsg
    = A
    | B
    | C
    | D
    | E
    | F

type Msg
    = FeedMsg
    | G
    | ... 18 other messages types

```
It also allows to be more accurate about the types expected by some functions as parameter.

A third tip he shared with us is to write functions using [extensible records](http://elm-lang.org/docs/records) as parameter.

Finally, Richard recommended the reading of ["Data structure in elm"](http://tech.noredink.com/post/140646140878/data-structures-in-elm) by Tessa Kelly.

## A tool for telling interactive stories - Jeff Schomay

Jeff works at Pivotal tracker, who uses Elm en production with a great satisfaction. *You can find this quote on the [Elm language Home Page](http://elm-lang.org)*:

> "We’ve had zero run-time failures, the filesize is ridiculously small, it
> runs faster than anything else in our code base. We’ve also had fewer bugs...
>
> To sum it up, our manager has mandated that all new code be written in Elm."

Jeff created an [interactive story engine](http://elmnarrativeengine.com/) in text mode.

He used the [Entity Component System](https://en.wikipedia.org/wiki/Entity_component_system) pattern, for the application to be configurable. Using this pattern, logic, presentation and content are fully separated.

To implent the ECS pattern, he used dictionnaries whose keys are strings. He won flexibility, but lost some type safety, as the compiler cannot detect typos in these strings by example.

This pattern allowed him to implement [three very different GUI](http://blog.elmnarrativeengine.com/sample-stories/) for the same interactive story.

He especially created one of them using the [Phaser JS game framework](http://phaser.io/). Combining both technologies was no big deal.

## Manipulating time in Elm - Vincent Billey

Vincent talked about the package [rluiten/elm-date-extra](https://github.com/rluiten/elm-date-extra), which fixes some issues in the standard date package. like the missing i18n suppport.

The content of this presentation is detailed on [Synbioz's blog](https://www.synbioz.com/blog/time-manipulation-in-elm)

## Persistent collections: How they work, and when to use them - Robin Heggelund Hansen

Robin presented the main data structure in ELM, with the use cases they fit best, according to their implementation.

Immutable collections have some performance challenges you won't find in their mutable equivalent.

Each item of a **list** points to the next one. Lists are ideal:

* to work on the first item they contain
* to iterate on items from left to right
* when the most important thing to do is to add items to the list

For all other usages, lists are not optimal. By example, for some operations like map, they require two iterations: one to apply the mapped function, the other to restore the original order of the items.

In **Dict** implementation, each item has pointers to two other elements: one with a key with an inferior value, one with a key with a greater value. This will allow to navigate quickly amongst the items.

Dicts have the following distinctive characteristics:

- the access to an item through its key is fast (binary search)
- each key is unique
- items are ordered according the value of their keys
- keys must be comparable

In Elm, you cannot extend the definition of "comparable", and it can be a limitation to the usage of Dictionnaries.

A **Set** is similar to a Dict whose values are comparable. So the implementation is quite similar to the one of Dict.

**Arrays** are implemented as tree whose leaves are 32 item sub-arrays. This implementation has the following benefits:

- reading and writing access to any value is fast, using the index;
- arrays are fast too, compared to other data structures, when you have to add an item at the end
- the map, filter, foldr and foldl functions are fast when applied to arrays

Robin recommends to avoid data structure temporary conversion, when the goal is just to benefit from a faster function (by example converting a list to an array, just to apply a map, and then convert it again to a list. It usually will be slower than applying map on the list).

Elm guides us to select the best data structure for our use cases, as it does not expose inefficient operation, like get/set on a list.

Robin wrote an implementation of arrays which is faster for search than the current standard one, and it will be integrated in next Elm release. It fixes some runtime issues too.

## Elm from a Business Perspective - Amitai Burstein

After Sébastien Crème, Amitai Burstein was the second CTO to come on stage to explain us why Elm is a good choice for [his company](https://www.gizra.com/). In a quite funny talk, he explained that their Elm application has much less issues than their Angular one.

According to Amitai, with Elm you'll get better financial results, less issues and happier developers :)

## Elm Native UI in Production - Josh Steiner

[Purple Train](https://purpletrainapp.com/) is a simple mobile application, initially written using React Native.

[thoughtbot](http://thoughtbot.com) developers really liked this technology. They believe it allowed them to develop the application three times faster than with native iOS and Android technologies. Hot-reloading was one of the key points in this, in addition to the shared code.

However, React Native has a major drawback according to Josh: it relies on Javascript. With React Native, runtime errors are very common (Red Screen of Death).

Elm Native UI is based on React Native. But with Elm Native UI, you have no runtime error.

Naturally, Elm Native UI is not perfect.
It's not production ready, still misses somes libraries, adds a new layer of abstraction...

[More info](https://robots.thoughtbot.com/elm-native-ui-in-production)

## Conclusion

The conference was really nice. There was a lot of energy in the air, due to the general enthusiasm for the language. Of course there was a few organisation problems, certainly linked to the fact it was the first Elm Europe conference ever (I really would have appreciated some tea during the breaks!). But without any doubt, this will improve next year. Thibaut Assus, the main organizer, already announced the 2018 edition will take place in Paris.

As for me, I will start my first Elm project very soon!
