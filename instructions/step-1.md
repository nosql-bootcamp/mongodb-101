# Étape 1 - Opérations CRUD

## C - Create

La création d'un document dans une collection se fait via la méthode `insert()` (comme vu précédemment lors de la prise en main du shell).

Le shell étant un interpréteur Javascript, il est possible d'insérer plusieurs documents à l'aide d'une boucle `for` :

```javascript
for (var i = 1 ; i <= 100 ; i++) {
    db.personnes.insert({
      "prenom" : "Prenom_" + i,
      "nom" : "Nom_" + i,
      "age" : (Math.floor(Math.random() * 50) + 20)
    })
}
```

La méthode `insert()` peut également prendre en paramètre un tableau de documents à créer.

```javascript
db.personnes.insert([
  { "prenom" : "Jean", "nom" : "DUPONT", "age" : 56},
  { "prenom" : "Robert", "nom" : "MARTIN", "age" : 63},
  { "prenom" : "Kévin", "nom" : "DUNORD", "age" : 21},
])
```

## R - Read

La méthode `findOne()` permet de récupérer un document d'une collection (échantillon) :

```javascript
db.personnes.findOne()
```

La méthode `find()` permet de rechercher des documents dans une collection. Elle possède deux paramètres (optionnels) :

* les critères de recherche (un document JSON)
* la projection (les attributs à retourner)

Par exemple, pour rechercher les personnes se nommant "DUPONT" :

```javascript
db.personnes.find({ "nom" : "DUPONT" })
```

Il existe un certain nombre d'opérateurs utilisables pour filtrer lors de la recherche. Par exemple pour lister les personnes de plus de 60 ans :

```javascript
db.personnes.find({ "age" : { "$gte" : 60 }})
```

Si vous ne souhaitez retourner que le `nom` des personnes, utilisez une projection (deuxième paramètre de la méthode `find()`) :

```javascript
db.personnes.find({ "nom" : "DUPONT" }, {"nom" : 1})
```

Par défaut l'identifiant `_id` est toujours remonté. Il est tout de même possible de l'exclure en le disant explicitement :

```javascript
db.personnes.find({ "nom" : "DUPONT" }, {"_id" : 0, "nom" : 1})
```
**NB : les valeurs `1` et `0` peuvent être avantageusement remplacées par `true` et `false` néanmoins la totalité de la doc Mongo utilise la notation `0` et `1`.**

Si aucun des deux paramètres n'est renseigné, tous les documents de la collection seront retournés.

Plus précisément, la méthode `find()` retourne un **curseur** permettant d'itérer sur la liste des documents résultant de la recherche. Le shell itère automatiquement sur le curseur et retourne les 20 premiers documents. La commande `it` permet de continuer l'itération pour obtenir les 20 documents suivants, et ainsi de suite.

Plusieurs méthodes utiles sont disponibles sur un curseur, dont :

* `sort()` pour trier les documents
* `limit()` pour limiter le nombre de documents

Par exemple pour récupérer les 3 personnes les plus âgées :

```javascript
db.personnes.find().sort({"age": -1}).limit(3)
```

Un curseur possède également une méthode `count()`. Par exemple pour compter directement le nombre de personnes de plus de 60 ans :

```javascript
db.personnes.find({ "age" : { "$gte" : 60 }}).count()
```

## U - Update

La mise à jour de documents se fait via la méthode `update()`, qui possède plusieurs paramètres :

* le filtre permettant de sélectionner les documents à mettre à jour
* la requête de mise à jour
* des options (par exemple : `{"multi" : true}` pour mettre à jour tous les documents correspondant au filtre). Dans le cas où cette option n'est pas précisée, seul le premier enregistrement trouvé est mis à jour.

Par exemple, pour répartir les personnes dans deux catégories ("Master" pour les plus de 40 ans, "Junior" pour les autres) :

```javascript
db.personnes.update({"age" : { "$gte" : 40 }}, {"$set" : {"categorie" : "Master"}}, {"multi" : true})
db.personnes.update({"age" : { "$lt" : 40 }}, {"$set" : {"categorie" : "Junior"}}, {"multi" : true})
```

Remarque 1 : MongoDB crée automatiquement l'attribut "categorie" qui n'existait pas auparavant !

Remarque 2 : depuis MongoDB 3.2, la méthode `updateMany()` peut être utilisée pour éviter d'avoir à préciser `{"multi" : true}` en option de l'instruction `update`. Essayez ! :-)

```javascript
db.personnes.updateMany({"age" : { "$gte" : 40 }}, {"$set" : {"categorie" : "Master"}})
db.personnes.updateMany({"age" : { "$lt" : 40 }}, {"$set" : {"categorie" : "Junior"}})
```

## D - Delete

La méthode `remove()` permet de supprimer des documents étant donné un filtre :

```javascript
db.personnes.remove({ "nom" : "DUPONT" })
```

Une option (`{justOne : true}`) permet de préciser si la méthode `remove()` supprime un seul ou l'ensemble des documents correspondant au filtre.

Depuis MongoDB 3.2, il existe les méthodes `deleteOne()` et `deleteMany()`.

Pour supprimer complètement une collection :

```javascript
db.personnes.drop()
```

## Bulk

Il est possible d'effectuer plusieurs opérations (insert, mise à jour et/ou delete) en une seule fois grâce à la méthode [`bulkWrite()`](https://docs.mongodb.com/manual/reference/method/db.collection.bulkWrite/).

Cette méthode prend en paramètre un tableau d'objets représentant les opérations à réaliser. Par exemple si l'on souhaite catégoriser les personnes en "Junior" (moins de 40 ans) et "Master" (plus de 40 ans) et supprimer les personnes de moins de 25 ans :

```javascript
db.personnes.bulkWrite([
  {
    "updateMany": {
      "filter": {"age" : { "$gte" : 40 }},
      "update": {"$set" : {"categorie" : "Master"}}
    }
  },
  {
    "updateMany": {
      "filter": {"age" : { "$lt" : 40 }},
      "update": {"$set" : {"categorie" : "Junior"}}
    }
  },
  {
    "deleteMany": {
      "filter": {"age" : { "$lt" : 25 }}
    }
  }
])
```

## Next

Vous pouvez passer à l'étape suivante : [Les tableaux](./step-2.md)
