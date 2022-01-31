+++
title = "Proposition de pipe operator pour JavaScript"
date = "2022-01-29"
author = "Mikkel"
cover = "hello.jpg"
description = "L’idée d’ajouter un pipe operator à JavaScript provient de deux propositions issues des languages F# par Microsoft et Hack par Facebook avec chacun leurs avantages et désavantages. Parlons en!"
categories = ['Techonologie']
tags = ['JavaScript','JS-Proposition','Discussion']
+++

# Proposition de pipe operator pour JavaScript

L’idée d’ajouter un pipe operator à JavaScript provient de deux propositions issues des languages F# par Microsoft et Hack par Facebook avec chacun leurs avantages et désavantages.

Les défenseurs de la proposition affirment que des fonctions imbriquées peuvent rapidement devenir complexes et illisibles. Pour combler ceci, ils veulent introduire l’opérateur “|>” à JavaScript.

Pour comprendre ce que cet opérateur peut apporter au language, regardons les deux propositions.

## Fonctionnement du Hack pipe

Voici un exemple de fonctions imbriquées avec et sans l’opérateur pipe:

```jsx
// Sans opérateur pipe
const y = h(g(f(x)));

// Avec opérateur pipe
const y = x |> f(%) |> g(%) |> h(%);
```

### Avantages

L’opérateur n’a pas pour but de racocurcir des syntaxes mais plutôt les rendre plus accessibles / lisibles. Le hack pipe utilise le symbole % pour indiquer au pipe ou le résultat doit aller.

L’opérateur tel qu’il est peut gérer les mots-clés **await** et **yield** sans syntaxe spéciale.

### Désavantages

Cette proposition peut très rapidement devenir lourd dû à l’usage du caractère % pour dénoter le résultat.

Certains affirment que le Hack pipe n’est qu’une implémentation partielle du F# pipe plus complet et suggère un implémentation optionnelle sans le caractère % tel que le F# pipe.

## Fonctionnement du F# pipe

En reprenant le code de dessus:

```jsx
// Sans opérateur pipe
const y = h(g(f(x)));

// Avec opérateur pipe
const y = x |> f |> g |> h;
```

### Avantages

La proposition du F# pipe n’utilise pas le caractère %, il applique automatiquement la fonction à sa droite à la variable à sa gauche. De ce fait il est moins verveux que le Hack pipe. Ceci le rend par exemple beaucoup plus performant lors de la déstructuration d’objets :

```jsx
const obj = {first: "a", last: "b"};

// F# pipe
const str = obj |> ({first,last}) => first + ' ' + last;

// Hack pipe sans déstructuration
const str = obj |> %.first + ' ' + %.last;
// Hack pipe avec déstructuration
const str = obj |> (({first,last}) => first + ' ' + last)(%);
```

Le F# pipe se prête aussi mieux à des currifications de fonctions, à la cause une arité plus grand que 1 (fonctions prenant plus d’un arguments).

### Désavantages

Malheureusement la currification n’est pas toujours le meilleur choix en JavaScript, l’absence d’un opérateur “d’application partielle” en serait la cause. De ce fait, le F# pipe devient plus verbeux que sa concurrence.

L’utilisation de mémoire n’est pas optimale avec cet opérateur à cause de la création et l’invocation de fonctions à chaque utilisation. De plus, les mots-clés **await** et **yield** ne fonctionnent pas avec.

Finalement l’introduction du F# pipe pourrait amener à un ecosystème JavaScript dans lequel une moitié emploie fortement la currification et l’autre non.

## Utilisations

Un pipe opérateur dans JavaScript aurait 3 buts:

- Améliorer la syntaxe de fonctions imbriquées
- Post-processing de valeurs
- Permets l’enchainement d’opérations

Comme indiqué et montré ci-dessus, l’opérateur permet d’écrire des fonctions imbriquées complexes d’une manière beaucoup plus lisible et explicite. Ceci est grâce à l’enchainement d’opérations qui rend notre code plus logique et également moins verbeux.

L’opérateur permet également du post processing de valeurs ou de fonctions sur une ligne au lieu de plusieurs au lieu de devoirs passer par des variables temporaires.

## Conclusion

L’introduction d’un opérateur pipe dans JavaScript pourrait apporter avec lui un nombre d’avantages et permettre d’écrire du code plus compréhensible. Cependant toutes les solutions proposées ont leurs désavantages.

Des propositions alternatives pour répondre au même problème existent tel que “Function.pipe()” qui devrait nous permettre de déclarer une logique similaire avec des arrows fonctions. Mais tout comme le F# pipe, celui-ci ne permet pas l’utilisation de **await** et **yield.**

Pour l’instant il est possible de contourner l’absence de ce opérateur en utilisant des variables temporaires pour nos calculs, le désavantage étant sa verbosité. On peut améliorer ceci en utilisant des variables avec des noms courts ce qui péjore la lisiblité du code.

## À retenir

L’opérateur Hack pipe a été retenu comme proposition par TC39 à l’instar du F# pipe. Pour cause les faiblesses du F# pipe.

Une future implémentation de cette opérateur pourrait drastiquement changer la logique et l’approche que nous prenons vis-à-vis de certaines opérations en améliorant la lisibilité du code dans certains cas de figure ou en permettant des enchainements.

Cette introduction ne serait pas non plus un miracle pour le language car toutes les opérations mentionnées sont actuellements possibles bien qu’ils demandent une implémentation un peu plus complexe. Le pipe serait donc plus une amélioration du “quality of life” des développeurs.

### Sources :

- [](https://2ality.com/2022/01/pipe-operator.html?ref=webdesignernews.com)https://2ality.com/2022/01/pipe-operator.html?ref=webdesignernews.com
- [GitHub - tc39/proposal-pipeline-operator: A proposal for adding a useful pipe operator to JavaScript.](https://github.com/tc39/proposal-pipeline-operator)
- [Hack is dead. Long live F#. · Issue #205 · tc39/proposal-pipeline-operator · GitHub](https://github.com/tc39/proposal-pipeline-operator/issues/205)