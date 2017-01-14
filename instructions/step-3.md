# Étape 3 - Les index

Pour comprendre l'utilité des index, rien de tel qu'[un petit dessin plutôt qu'un long discours](http://www.commitstrip.com/en/2014/06/03/the-problem-is-not-the-tool-itself/)

![commitstrip](http://www.commitstrip.com/wp-content/uploads/2014/06/Strip-Probl%C3%A8me-dIndex-650-finalenglish.jpg)

## Création d'un index

La méthode `createIndex()` permet d'ajouter un index sur une collection de documents.

Par exemple, pour créer un index ascendant sur le nom des personnes :

```javascript
db.personnes.createIndex({"nom": 1})
```

Remarque : utilisez `{"nom": -1}` pour créer un index descendant.

Vous pouvez vérifier que l'index a bien été créé grâce à la méthode `getIndexes()` :

```javascript
db.personnes.getIndexes()
```

Voici le résultat (vous pouvez remarquer qu'il existe un index créé par défaut sur l'identifiant `_id`) :

```json
[
  {
    "v": 2,
    "key": {
      "_id": 1
    },
    "name": "_id_",
    "ns": "workshop.personnes"
  },
  {
    "v": 2,
    "key": {
      "nom": 1
    },
    "name": "nom_1",
    "ns": "workshop.personnes"
  }
]
```

Il est également possible de créer des index composites (sur plusieurs attributs) :

```javascript
db.personnes.createIndex({"nom": 1, "age": -1})
```

## Explain

La méthode `explain()` permet de savoir si un index a été utilisé lors de l'exécution d'une requête.

```javascript
db.personnes.find({"nom": "DUPONT"}).explain()
```

## Next

Mongo-101 est terminé ! Essayez d'autres workshops de [nosql-bootcamp](https://github.com/nosql-bootcamp) ;-)
