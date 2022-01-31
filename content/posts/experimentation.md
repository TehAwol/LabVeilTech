+++
title = "Expérimentation - Gestion de routes asyncrones sur Express"
date = "2021-01-24"
author = "Mikkel"
cover = "hello.jpg"
description = "Dans cette expérimentation, je vais implémenter une requête asynconre dans une route Express de différentes façons en regardant les avantages et désavantages."
+++

# Expérimentation - Gestion de routes asyncrones sur Express

## Asyncrone et Express

Dans le framework Node.js, Express, je vais mettre en place un REST API qui retourne divers ressources grâce à des requêtes. Je vais employer du code asyncrone afin de communiquer avec notre base de données hébergé sur un serveur MongoDB externe.

Devant faire face à divers latence ou tout simplement des érreurs, il est important qu'un gestion d'erreur est en place pour le code asyncrone. Express propose déjà une gestion d’erreur avec son Error Handler qui est accéssible en utilisant lui passant l’erreur grâce à next().

## Le contexte et but de l’expérimentation

Dans cette expérimentation, je vais implémenter une requête asynconre dans une route Express. Je vais travailler sur l’application Local-Search ([GitHub - Grolims/ArchiOWeb: Deploy your REST API on a cloud application platform](https://github.com/Grolims/ArchiOWeb)), un REST API qui permet à des agriculteurs de partager leur points de ventes et les produits qui y sont disponibles. Les agriculteurs doivent crée un compte qui est associé à tout leurs points de ventes et leurs produits.

Afin de pouvoir créer un nouveau produit, l’utilisateur et le point de vente associé doit exister. L’expérimentation va être fait sur la route correspondant (création de item) afin de pouvoir comprendre les différentes implémentations et si possible, identifier la solution optimale.

## La route initiale

Voici la route pour créer un nouveau produit (item).

```jsx
router.post('/', (req, res, next) => {

  // Vérifie que l'utilisateur existe
  const existsUser = await User.countDocuments({ _id: req.body.userId });  // Vérifie que le point de vente existe
  const existsSalepoint = await Salepoint.countDocuments({ _id: req.body.salepointId });

  // Gestion d'erreur si l'utilisateur existe pas
  if (!existsUser) {
      return res.status(400).send('User ID missing or invalid')
  }
  // Gestion d'erreur si le point de vente existe pas
  if (!existsSalepoint) {
      return res.status(400).send('Salepoint ID missing or invalid')
  }

  // Sauvegarde la nouveau produit et renvoie l'entré à l'utilisateur    
  const newItem = new Item(req.body);
  await newItem.save();
  res.status(201).send(newItem);
});
```

Cette implémentation présente plusieurs problèmes, les requêtes sont des opérations asyncrone qui font un appel à la base de données. Pour les gérer correctement dans le code nous devont rendre notre route (plus spécifiquement son callback) asyncrone avec le mot clé **async**. Les requêtes doivent également être précédé par le mot clé **await** pour indiquer qu’elles sont asyncrones.

```jsx
router.post('/', async (req, res, next) => {

  const existsUser = await User.countDocuments({ _id: req.body.userId });
  const existsSalepoint = await Salepoint.countDocuments({ _id: req.body.salepointId });

  if (!existsUser) {
      return res.status(400).send('User ID missing or invalid')
  }
  if (!existsSalepoint) {
      return res.status(400).send('Salepoint ID missing or invalid')
  }

  await newItem.save();
  res.status(201).send(newItem);
});
```

Le code est à présent exécutable en locale (si la BDD tourne sur notre machine). Comme indiqué dessus, une des contraintes pour la création d'un produit est que l'utilisateur et le point de vente associé existe. Les deux requêtes présentes s'occupent de valider cette existence, en conséquence elles font donc appels à la base de données. Une gestion d'erreur est donc cruciale pour gérer les cas de figure ou ces requêtes ne peuvent avoir lieu.

### Rappel sur les promesses

Les requêtes asyncrone sont temporairement des promesses en attendant leur réponse. La réponse donc notre cas va être un booléan si l'utilisateur ou le point de vente existe, au contraire un erreur nous est retourné en cas d'échec.

## L'implémentation classique

Nous pouvons implémenter notre route en utilisant une logique avec .then().

```js
router.post('/', async (req, res, next) => {

    User.countDocuments({ _id: req.body.userId })
        .then(resp => if(resp){
            return res.status(400).send('User ID missing or invalid')
        })
        .catch(err => next(err));

    Salepoint.countDocuments({ _id: req.body.salepointId })
        .then(resp => if(resp){
            return res.status(400).send('Salepoint ID missing or invalid')
        })
        .catch(err => next(err));

    const newItem = new Item(req.body);
    await newItem.save().then(res.status(201).send(newItem)).catch(err => next(err));

});
```

### Verdicte

Cette implémentation est pas très accèssible (il arrive que des problèmes d'identation surviennent sur des projets commun avec cette solution) et très verbeuse. Il est recommander de plutôt utiliser une try catch afin d'améliorer la lisibilité du code et être à jour avec les best practices.

## Try & Catch

La première solution que j'ai implémenté était un Try & Catch. Dans le block Try si la requête ne se résoud pas correctement, un erreur est retourner au catch (d'ou le nom) et je le passe à la gestion d'erreur d'Express en utilisant la fonction next().

```jsx
router.post('/', async (req, res, next) => {

  try {
    const existsUser = await User.countDocuments({ _id: req.body.userId });
    const existsSalepoint = await Salepoint.countDocuments({ _id: req.body.salepointId });

    if (!existsUser) {
      return res.status(400).send('User ID missing or invalid')
    }
    if (!existsSalepoint) {
      return res.status(400).send('Salepoint ID missing or invalid')
    }

    const newItem = new Item(req.body);
    await newItem.save();
    res.status(201).send(newItem);
  } catch(err) {
    next(err)
  }
});
```

Toutes les requêtes asyncrone peuvent être encapsulé par le try catch car un erreur va directement se résoudre dans le catch.

La fonction next() passe l'erreur au gestionnaire d'erreur d'Express qui vient s'ajouter à la fin du stack du middleware. Ce gestionnaire est très basique, il pourrait donc être intéressant d'améliorer ceci.

### Verdicte

Cette implémentation est bien plus lisible que l'usage de then(). Par contre

## Fonction Encapsulante

Une autre façon décrire le try catch serait avec un fonction asyncrone et un .then()

```js
router.post('/', async (req, res, next) => {

    async function main() {
        const existsUser = await User.countDocuments({ _id: req.body.userId });
        const existsSalepoint = await Salepoint.countDocuments({ _id: req.body.salepointId });

        if (!existsUser) {
          return res.status(400).send('User ID missing or invalid')
        }
        if (!existsSalepoint) {
          return res.status(400).send('Salepoint ID missing or invalid')
        }

        const newItem = new Item(req.body);
        await newItem.save();
        res.status(201).send(newItem);
    }

    main().catch(next)
});
```

Cette implémentation est complète mais il va rapidement devenir ennuyant d'écrire tout ce code pour chaque route. En conséquence la fonction peut être utilisé comme un sorte de middleware qui encapsule les parties asyncrones.

```js
router.post('/',  asyncHandler(req, res, next) => {

     const existsUser = await User.countDocuments({ _id: req.body.userId });
     const existsSalepoint = await Salepoint.countDocuments({ _id: req.body.salepointId });

     if (!existsUser) {
        return res.status(400).send('User ID missing or invalid')
     }
     if (!existsSalepoint) {
        return res.status(400).send('Salepoint ID missing or invalid')
     }

     const newItem = new Item(req.body);
     await newItem.save();
     res.status(201).send(newItem);
});


function asyncHandler (callback) {
    return function (req, res, next) {
        callback(req, res, next).catch(next)
    }
}
```

Comme mentionné avant, si un érreur survient le **catch** va l'attraper et la passer au error handler de Express.

### Verdicte

Cette implémentation est très élégante est légère pour des applications de grande échelle. Il peut être employé comme une sorte d'utilitaire pour gérer la majorité de notre code asyncrone dans nos routes. De plus, nous pouvons y ajouter une gestion d'erreur plus complet que celui d'Express si on le souhaite.

## Express Async Handler

La fonction encapsulante est une implémentation assez répandu qui a donné lieu à plusieurs package qui accomplisent le même fonctionnement. Un exemple est express-async-handler.

```clike
npm install express-async-handler --save
```

```jsx
const asyncHandler = require('express-async-handler')

app.post('/signup', asyncHandler(async(req, res) => {
    await firstThing()
    await secondThing()
}))
```

Ce package accomplit exactement la même chose que l'implémentation précédante en nous évitant de devoir déclarer une fonction. De plus, le package permet d'assurer que next() est toujours le dernier arguments dans la pile de middlewares.

### Verdicte

Plus léger et complet que l'implémentation fait maison, cette solution est optimale pour gérer tout code asyncrone dans des routes Express. La déclaration non explicite du asyncHandler peut par contre péjorer la compréhension des collaborateurs.

## A retenir

Les routes présentants du code asyncrone sur Express peuvent être implémentés de différentes manières qui accomplisent tous la même tâche mais ayant tous leur propre complexité.

Travaillant surtout sur des gros projets ou du code asyncrone est inévitable dans presque toutes les routes, il devient très avantageux d'utiliser un package comme async-handler ou d'en déclarer un nous même. Ceci permet aussi aux collaborateurs de contribuer du code uniforme et une compréhension plus complet de l'application.

Mais au final chaque implémentations fontionnent, le choix revient donc au développeur et à ses préférences.

### Sources :

- [https://www.wisdomgeek.com/development/web-development/using-async-await-in-expressjs/](https://www.wisdomgeek.com/development/web-development/using-async-await-in-expressjs/)
- [https://www.npmjs.com/package/express-async-handler](https://www.npmjs.com/package/express-async-handler)