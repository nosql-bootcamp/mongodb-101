# MongoDB 101

![mongo-logo](https://fr.wikipedia.org/wiki/Fichier:MongoDB-Logo.svg)

**MongoDB 101** est un workshop permettant de découvrir la base de données NoSQL MongoDB et son écosystème, étape par étape.

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons Licence" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a>

<span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">mongodb-101</span> par <a xmlns:cc="http://creativecommons.org/ns#" href="https://github.com/nosql-bootcamp/mongodb-101" property="cc:attributionName" rel="cc:attributionURL">Chris WOODROW, Sébastien PRUNIER et Benjamin CAVY</a> est distribué sous les termes de la licence <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons - Attribution - NonCommercial - ShareAlike</a>.

## Introduction

**MongoDB** est une base de données **NoSQL** créée en 2007 par la société 10gen (maintenant MongoDB Inc.) et était [Open Source](https://github.com/mongodb/mongo) jusqu'en 2018 (Licence AGPL), un peu moins depuis (License SSPL).

MongoDB est une base de données **orientée documents** et fait partie des bases [les plus populaires du marché](http://db-engines.com/en/ranking), toutes catégories confondues.

Les documents sont des documents [JSON](http://www.json.org/). Voici un exemple simple de document pouvant être stocké dans MongoDB :

```json
{
  "name": "Sébastien",
  "age": 35,
  "likes": ["MongoDB", "Javascript", "Scala"]
}
```

*Plus précisément, le format de stockage des documents est [BSON](http://bsonspec.org/) (Binary JSON), une représentation binaire de JSON, proposant plus de types que JSON (le type Date notamment).*

Les documents sont stockés dans des **collections**. Une **database** peut contenir plusieurs collections de documents. Un **serveur** MongoDB peut contenir plusieurs databases.

Ce workshop est basé sur la **version 4.4.3** de MongoDB.

## Étapes du workshop

* Étape 0 - [Installation et prise en main](./instructions/step-0.md)
* Étape 1 - [Opérations CRUD](./instructions/step-1.md)
* Étape 2 - [Les tableaux](./instructions/step-2.md)
* Étape 3 - [Les index](./instructions/step-3.md)

## Liens utiles

* Site officiel : https://www.mongodb.com/
* Robo 3T (GUI - ex Robomongo) : https://robomongo.org/
* Mongo Hacker (Mongo shell « on steroids ») : https://github.com/TylerBrock/mongo-hacker
