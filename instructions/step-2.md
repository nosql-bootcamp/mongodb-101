# Étape 2 - Les tableaux

Il est possible d'utiliser des tableaux dans les documents. Par exemple, on peut stocker les compétences des personnes de la manière suivante :

```javascript
db.personnes.insert({
    "_id": "jdupont",
    "prenom": "Jean",
    "nom": "DUPONT",
    "competences" : [
        "Java",
        "Javascript",
        "HTML"
    ]
})
```

Pour rechercher les personnes possédant la compétence "Java" :

```javascript
db.personnes.find({ "competences" : "Java" })
```

Pour ajouter une compétence :

```javascript
db.personnes.update({ "_id" : "jdupont" }, {"$push" : {"competences" : "CSS"}})
```

Pour éviter les doublons :

```javascript
db.personnes.update({ "_id" : "jdupont" }, {"$addToSet" : {"competences" : "CSS"}})
```

Pour enlever une compétence :

```javascript
db.personnes.update({ "_id" : "jdupont" }, {"$pull" : {"competences" : "CSS"}})
```

Pour limiter le nombre de compétences à 3 (plus de détails [ici](https://docs.mongodb.com/manual/reference/operator/update/slice/#up._S_slice)) :
```javascript
db.personnes.update(
   { "_id" : "jdupont" },
   {
     $push: {
        "competences": {
           $each: [ "Fortran", "Scala" ],
           $slice: -3
        }
     }
   }
)
```

Ici, nous avons ajouté "Fortran", "Scala" à droite du tableau et conservé les 3 derniers éléments (d'où le `-3`). Pour conserver les 3 premiers, on aurait utilisé la valeur `+3`.

## Next

Vous pouvez passer à l'étape suivante : [Les index](./step-3.md)
