+++
title = "Observable, l’environnement dataviz accessible ?"
date = "2021-12-27"
author = "Mikkel"
cover = "hello.jpg"
description = "Observable, un “environement de découverte intégré” qui promet de rendre la dataviz plus portable et rapide grâce à un code plus accessible. Que promet-il ?"
categories = ['Frontend']
tags = ['JavaScript','Observable','D3','Dataviz','Accessibility']
+++

# Observable, l’environnement dataviz accessible ?

Mike Bostock, le créateur de la libraire D3 (librairie pour la visualisation des données), annonce la sortie d'Observable. Un environnement de Dev en ligne.

L’auteur explique qu’en produisant des visualisations plus complexes, le code le devenait également. Les languages de code utilisés, la syntaxe et la manière d’utilisation sont des sources de complexités qui contribue à ce problème. Le manque de preview pour certains utilisateurs est également une barrière à l’accessibilité du code et du domaine.

Place à Observable. Un “environement de découverte intégré” qui promet de rendre la dataviz plus portable et rapide grâce à un **code** plus accessible. Un principe introduit sont des cellules de code que l’utilisateur peut ajouter à sa page de code (dans son navigateur). Dedans, il peut coder du JavaScript tout comme dans un vrai environnement. Bostock explique la raison d’être de Observable par 4 mots :

- Reactivité
- Visibilité
- Réusabilité
- Portabilité

Un exemple des cellules de code et de la **réactivité** de Observable

![1 fkkqWrbWXua8sMGlYqnqAgif](file://C:\Users\Mikke\Documents\HEIG-VD\Semestre%205\LabVeilTech\Articles\1%20fkkqWrbWXua8sMGlYqn-qA.gif)

source : [](https://medium.com/@mbostock/a-better-way-to-code-2b1d2876a3a0)https://medium.com/@mbostock/a-better-way-to-code-2b1d2876a3a0

## Reactivité

Pour ce faire il introduit la réactivité, comme beaucoup d’autres framework, les variables sont réactives au changement et ceci de manière globale (sur une page).

Le code asynchrone est également pris en charge. Les cellules ne sont pas affichées tant que les requêtes soient résolues. Ce comportement est très important pour l’importation de libraire et bien sûr de données.

De manière plus importante pour la visualisation de données, la réactivité de l’environnement nous permet d’utiliser D3 pour créer directement des graphes en ajoutant le SVG généré au DOM.

Ceci nous permet d’ajouter plus interaction à nos graphes tels que des animations en une fois de plus prenant avantage de la libraire D3. Bostock donne également des exemples de graphes qui s’anime en fonction d’une valeur ou encore des proto-dashboard avec lequel l’utilisateur peut explorer les données avec un feedback visuel.

## Visibilité

Comme mentionné pour la réactivité, le fait de pouvoir afficher directement nos graphes à côté de notre code nous permet de les corriger plus facilement. Ceci rend également l’environnement hautement accessible en permettant à des débutants dans la visualisation de **voir** directement les changements dans le code ou pour les débutants d’avoir un feedback plus rapide et visuel de leur code.

On souligne également la possibilité d’afficher des executions de code pour des algorithmes qui ont une certaine durée. Ceci peut paraître comme étant un simple exercice mais au final ceci est utile pour visualiser comment notre code s’exécute. Ceci est très intéressant dans des contextes mathématiques pour par exemple, la résolution de problèmes géométriques.

## Réusabilité

Un grand avantage du code et surtout des librairies, est leur réusabilité, leur qualité à être réutilisé plusieurs fois dans différents projets et de différentes manières. Pour conserver ceci, les pages Observable sont considérés comme des fichiers JS par d’autres pages (avec un require() ).

Grâce à ce principe, les notebooks peuvent construit un peu comme tel des vrais projets dans lesquelles nous pouvons réutiliser des cellules ou page de code déclaré en amont.

Le plus grand avantage est bien sûr le partage avec les autres utilisateurs, les pages de code sont considérés comme des fichiers ou libraire de code par **de facto**. De ce fait, il est possible d’importer des pages d’autres utilisateurs tels que des packages NPM.

## Portabilité

Reposant sur votre navigateur et une connexion à Observable, l’environnement est très portable. Pour être rendu dans le navigateur, il utilise du JavaScript vanilla tout en donnant la possibilité d’importer des libraires ou du code d’ailleurs.

Il va de même pour le partage. Bostock se plaint que la majorité du code partagé partagé sur GitHub n’est pas accessible car nous devons repeter un nombre d’opérations pour le visualiser dans notre navigateur. Observable répond à ce problème en mettant à disposition un environnement partageable qui affiche directement le code qui le compose.

## Verdicte

La raison d’être de l’environnement est succintement expliquée par son créateur dans ces 4 points. Ayant utilisé l’environnement à divers buts, il est vrai qu’il répond à ces promesses mais derière cette élégante proposition, quelques problèmes se cachent.

L’environnement est fortement accessible et en reposant sur JavaScript, des développeurs web ou encore des data scientists peuvent facilement faire le changement vers Observable ou le tester très rapidement. Certains développeurs l’utilise en effet à des buts de documentation interactive pour leurs projets GitHub.

Dans mon usage, la puissance d'Obersvable est dans son accessibilité. De pouvoir partager et travailler de manière interactive sur du code (comme un GitHub live) et de pouvoir montrer en live les conséquences de notre code.

Ceci est fortement utile dans des contextes où l'on travaille avec des personnes moins familiers avec le code ou qui on besoin d'un feedback plus visuel.

### Sources :

- [](https://medium.com/@mbostock/a-better-way-to-code-2b1d2876a3a0)https://medium.com/@mbostock/a-better-way-to-code-2b1d2876a3a0