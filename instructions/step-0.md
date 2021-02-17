# Étape 0 - Installation et prise en main

## Installation

Téléchargez la dernière version stable de MongoDB sur [mongodb.com/download-center](https://www.mongodb.com/download-center/community) correspondant à votre système d'exploitation.

Le workshop est basé sur la version 4.4.3 de MongoDB.

Dézippez le bundle dans le dossier de votre choix, par exemple `$HOME/progs/mongodb-4.4.3`.

Les exécutables nécessaires au fonctionnement de MongoDB se trouvent dans le dossier `$HOME/progs/mongodb-4.4.3/bin`, en particulier :

* `mongod` : démon permettant de démarrer une instance de la base de données MongoDB.
* `mongo` : shell JavaScript interactif permettant de se connecter à une instance `mongod`.

Pour plus de facilités, vous pouvez ajouter ce dossier à votre `PATH`, afin que les commandes `mongod` et `mongo` soient directement accessibles.

Par exemple sous Linux, ajoutez les lignes suivantes à votre fichier `.profile` :

```bash
# Path to MongoDB binaries
PATH="$HOME/progs/mongodb-4.4.3/bin:$PATH"
export PATH
```

Par défaut :

* MongoDB démarre sur le port `27017`. Cela peut être modifié via le paramètre `--port`.
* MongoDB stocke ses données dans le dossier `/data/db`. Cela peut être modifié via le paramètre `--dbpath`

Vous pouvez donc créer un dossier spécifique pour stocker les données du workshop, par exemple `$HOME/data/mongo-101-data` :

```bash
mkdir -p "$HOME/data/mongo-101-data"
```

Démarrez MongoDB à l'aide de la commande suivante :

```bash
mongod --dbpath="$HOME/data/mongo-101-data"
```

## Prise en main du shell

MongoDB propose un shell Javascript interactif permettant de se connecter à une instance (démarrée via la commande `mongod`, comme précédemment).

Pour lancer le shell :

```bash
mongo
```

Par défaut, le shell se connecte à l'instance `localhost` sur le port `27017`, sur une base de données de test. Voici les logs correspondant au lancement du shell :

```
MongoDB shell version v4.4.3
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("df8fb556-3e18-474f-8c56-32359d8fd2a3") }
MongoDB server version: 4.4.3
Welcome to the MongoDB shell.
For interactive help, type "help".
```

Il est possible de préciser un `host` et un `port` particulier :

```bash
mongo --host 192.168.xxx.xxx --port 27666
```

Le shell met à disposition un objet Javascript `db` qui permet d'interagir avec la base de données. Par exemple pour obtenir de l'aide :

```javascript
db.help()
```

Pour visualiser les bases disponibles :

```
show dbs
```

Pour changer de base de données, par exemple `mongo101` (MongoDB crée automatiquement la base si elle n'existe pas) :

```
use mongo101
```

Pour insérer un document dans une collection `personnes` (la collection est créée automatiquement si elle n'existe pas encore) :

```javascript
db.personnes.insert({ "prenom" : "Jean", "nom" : "DUPONT" })
```

Pour afficher un document de la collection `personnes` :

```javascript
db.personnes.findOne()
```

Le résultat ressemble à :

```
{
  "_id": ObjectId("5a2a52f1d889d3a80676c311"),
  "prenom": "Jean",
  "nom": "DUPONT"
}
```

MongoDB génère automtiquement un identifiant unique pour chaque document, dans l'attribut `_id`.

Cet identifiant est une chaîne hexadécimale composée des éléments suivants :

* Les 4 premiers octets représentent le nombre de secondes écoulées depuis Epoch
  * `5a2a52f1`, soit `1512723185` en décimal, soit le timestamp `2017-12-08T08:53:05+00:00`
* Les 3 octets suivants représentent l'identifiant de la machine (`d889d3`)
* Les 2 octets suivants représentent l'identifiant du processus (`a806`)
* Les 3 derniers octets correspondent à un compteur incrémental, initialisé à une valeur aléatoire (`76c311`)

Cet identifiant peut être défini manuellement (privilégiez des clés naturelles quand cela est possible) :

```javascript
db.personnes.insert({ "_id" : "jdupont", "prenom" : "Jean", "nom" : "DUPONT" })
```

Pour voir la liste des collections d'une base de données :

```
show collections
```

## Next

Vous pouvez passer à l'étape suivante : [Opérations CRUD](./step-1.md)
