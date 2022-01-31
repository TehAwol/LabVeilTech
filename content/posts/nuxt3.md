+++
title = "Nuxt3, la prochaine étape"
date = "2022-01-19"
author = "Mikkel"
cover = "hello.jpg"
description = "Avec la sortie récente de Vue3, une mise à jour de Nuxt2 (basé sur Vue 2) était donc attendue. En octobre 2021, la version beta de Nuxt3 est sortie."
categories = ['Backend']
tags = ['JavaScript', 'TypeScript', 'Vue', 'Nuxt', 'Discussion']
+++

# Nuxt3, la prochaine étape.

### Nuxt en quelques mots

Nuxt est une librairie JavaScript open source qui fournit un framework basé sur Vue fortement inspiré par Next.js, son équivalent React.

Avec la sortie récente de Vue3, une mise à jour de Nuxt2 (basé sur Vue 2) était donc attendue. En octobre 2021, la version beta de Nuxt3 est sortie.

![Nuxt3-image](/static/images/nuxt3.jpg)

## Que promet Nuxt3 ?

### Script Setup avec Composition API pour les SFC

Nuxt 3 apporte beaucoup de changements fondamentaux à son framework notamment le changement à TypeScript (discuté plus loin) comme language de framework, en gardant une compatibilité avec JavaScript tout comme d’autres frameworks. La possibilité d’utiliser le script setup avec la composition API est toujours présente ce qui a pour but de rendre les composants plus lisibles et le code plus accessible.

```jsx
<template>
  <button>Count</button>
  <Component>{{ counter }}</Component>
</template>

// Avec script setup
<script setup>
import Component from './components/Component';
import { ref } from 'vue';

const counter = ref(0)
<script>

// Sans script setup
<script>
import Component from './components/Component'
import { ref } from 'vue';

// in our component
setup (props) {
  const counter = ref(0)
  }

  return {
    counter
  }
}
<script>
```

Comme montré dans l’exemple, le script setup nous permet de nous affranchir de la fonction setup() et les variables qu’on en retourne. De plus, l’usage du Composition API introduit une structure plus cohérente à notre code qui permet de grouper notre code par logique au lieu de dépendre d’une structure orienté Options API.

Dans l’ensemble, ces ajouts permettent du code plus concis (élimination de beaucoup de logique de Vue) et une meilleure expérience de développement.

### Nuxt Nitro

Le nouveau serveur engine de Nuxt est Nitro qui amène avec lui un nombre de changements et de possibilités aux applications Nuxt. Tout comme Next, il est désormais possible de créer des routes API en plaçant nos fichiers dans les directoires concernés au lieu de les coder tel que dans Express.

Des déploiements facilités sur diverses plateformes telles que Vercel, Netlify, AWS, Azure ou encore des Node ou Deno.

Une génération statique de site statique va être implémentée de manière incrémentale.

Selon certain Nitro serait capable de produire des bundles qui seront jusqu’à 75x plus léger (client et serveur), une affirmation qui vaudrait l’objet d’une future expérimentation.

### Nuxt Bridge

Pour les applications qui tournent actuellement sur Nuxt2 ou Vue 2, l’introduction de Nuxt Bridge permet de faire le pas intermédiaire vers Nuxt3 en conservant d’anciennes fonctionnalités tout en introduisant progressivement des nouveaux.

Nuxt Bridge permet de prendre avantage du TypeScript avec la Composition API et Script Setup de manière partielle. Des composants ou plus généralement du code TS peuuvent donc être introduits graduellement dans nos applications. Cet intermédiaire introduit également Nuxi, le nouveau CLI de Nuxt 3.

### TypeScript

Nuxt3 introduit l’usage complet de TypeScript à la place de JavaScript. Une direction qu’une grande partie des frameworks semblent prendre. Nuxt Bridge devrait permettre cette transition pour des applications existantes.

TypeScript donne la possibilité de construire des applications qui évoluent tout en offrant du code plus strict et une meilleure expérience dev.

Ce changement semble venir très tard pour un framework tel que Nuxt mais étant jusqu’à récemment dépendant de Vue 2, cette évolution n’était pas possible avant.

## A retenir

Nuxt3 semble sans aucun doute être l’équivalent Vue de Next.js. Encore en phase de développement avec beaucoup d’implémentations à venir, le framework est très prometteur.

Le passage à TypeScript devrait encourager l’adoption plus générale du language dans les écosystèmes Vue et aussi permettre de construire des applications plus grandes et plus efficaces.

Ceci combiné au Composition API et le Script Setup devrait introduire un standard (best practice) de code pour les applications Vue 3 qui d’expérience sont très permissibles et peuvent parfois amener à des confusions entre collaborateurs.

Dans l’ensemble, le framework prend avantage des améliorations amenées par Vue3 et construit dessus pour fournir une expérience de développement amélioré.

## Sources

- [](https://www.vuemastery.com/blog/nuxt-3-is-here/)[Nuxt 3 is here! What does that mean for you? | Vue Mastery](https://www.vuemastery.com/blog/nuxt-3-is-here/)
- [](https://v3.nuxtjs.org/concepts/introduction)[Nuxt 3 - What is Nuxt?](https://v3.nuxtjs.org/concepts/introduction)
- [](https://www.sitepoint.com/nuxt-3-whats-new-get-started/)[SitePoint](https://www.sitepoint.com/nuxt-3-whats-new-get-started/)