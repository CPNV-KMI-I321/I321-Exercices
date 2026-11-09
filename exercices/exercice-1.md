# Exercice 1 — Méthodes et codes HTTP

**Durée :** 45 minutes · **Organisation :** en binômes · **Travail :** sans coder

L’objectif est de choisir une méthode HTTP pour exprimer l’intention du client, puis un code de réponse pour décrire le résultat du traitement par le serveur.

## Contexte

Vous concevez l’API d’un Foodtruck. Le pizzaïolo doit pouvoir consulter et gérer sa carte des pizzas.

La collection de pizzas est accessible à l’adresse `/api/pizzas`. Une pizza est identifiée par un nombre entier, par exemple `/api/pizzas/7`.

Pour cet exercice, le contrat de l’API précise que :

- les situations sont indépendantes les unes des autres ;
- la pizza 7 existe lorsqu’elle est mentionnée ; la pizza 999 n’existe pas ;
- les lectures, les créations et les modifications réussies renvoient des données JSON ;
- une modification remplace toutes les informations modifiables de la pizza ;
- une suppression réussie ne renvoie aucun contenu ;
- les données invalides produisent une réponse **400** ;
- une pizza possède au minimum un champ `name` contenant un texte non vide et un champ `price` contenant un nombre positif ou nul ;
- l’identifiant `id` d’une nouvelle pizza est attribué par le serveur.

## 1. Choisir la requête et la réponse

Pour chaque situation :

1. Choisissez la méthode HTTP adaptée.
2. Indiquez le chemin de la ressource.
3. Choisissez le code HTTP de la réponse.

Vous pouvez consulter les références MDN sur les [méthodes HTTP](https://developer.mozilla.org/fr/docs/Web/HTTP/Reference/Methods) et les [codes de réponse HTTP](https://developer.mozilla.org/fr/docs/Web/HTTP/Reference/Status).

| N° | Situation | Méthode HTTP | Chemin | Code HTTP |
| --- | --- | --- | --- | --- |
| 1 | Consulter la carte des pizzas. | | | |
| 2 | Consulter les informations de la pizza 7. | | | |
| 3 | Ajouter une pizza avec des données valides. | | | |
| 4 | Remplacer les informations de la pizza 7. | | | |
| 5 | Supprimer la pizza 7. | | | |
| 6 | Consulter les informations de la pizza 999. | | | |
| 7 | Ajouter une pizza dont le prix est négatif. | | | |
| 8 | Consulter la carte alors qu’elle ne contient aucune pizza. | | | |
| 9 | Ajouter une pizza avec des données valides, mais une erreur interne inattendue empêche son enregistrement. | | | |

## 2. Préparer le corps des requêtes

Présentez vos deux exemples dans les blocs de code Markdown de type `json` prévus ci-dessous.

### 2.1. Situations avec un corps JSON

Indiquez les numéros des situations qui nécessitent un corps de requête JSON.

**Réponse :**

_À compléter._

### 2.2. Création d’une pizza

Rédigez un corps JSON valide pour la création de la situation 3.

**Réponse :**

```json

```

### 2.3. Remplacement d’une pizza

Rédigez un corps JSON valide pour le remplacement de la situation 4.

**Réponse :**

```json

```

## 3. Expliquer ses choix

Répondez brièvement aux questions suivantes :

### 3.1. Une méthode, plusieurs codes de réponse

Dans les situations 2 et 6, pourquoi une même méthode HTTP peut-elle conduire à deux codes de réponse différents ?

**Réponse :**

_À compléter._

### 3.2. Une carte vide

Dans la situation 8, que contient le corps de la réponse ? Justifiez le code HTTP choisi.

**Réponse :**

_À compléter._

### 3.3. Une suppression réussie

Dans la situation 5, peut-on ajouter un message JSON à la réponse tout en conservant le code choisi ? Justifiez.

**Réponse :**

_À compléter._

## 4. Vrai ou faux : vérifier un échange HTTP

Dans cette partie, vous analysez d’autres services : une bibliothèque, des comptes utilisateurs, des fichiers et un service de conversion vidéo. Chaque situation précise ses propres conditions ; le contrat du Foodtruck des parties précédentes ne s’applique pas.

Pour chaque proposition, déterminez si la méthode, le chemin et la réponse sont cohérents avec la situation et la signification des méthodes et codes HTTP.

- Indiquez **Vrai** si l’ensemble de la proposition est correct.
- Indiquez **Faux** si au moins un élément est incorrect.
- Pour chaque proposition fausse, indiquez la correction nécessaire : méthode, chemin, code ou contenu de la réponse.

Les situations sont indépendantes. La mention « JSON » indique que la réponse contient un corps JSON. La mention « sans corps » indique qu’aucun contenu n’est envoyé dans la réponse.

Cette partie introduit notamment `HEAD`, `PATCH`, `OPTIONS` et de nouveaux codes de réponse. Consultez les références MDN pour rechercher leur signification.

| N° | Situation | Requête proposée | Réponse proposée | Vrai ou faux ? | Correction si faux |
| --- | --- | --- | --- | --- | --- |
| 1 | Vérifier les informations d’un fichier existant sans télécharger son contenu. | `HEAD /fichiers/guide.pdf` | `200 OK` avec le fichier PDF dans le corps de la réponse | | |
| 2 | Modifier uniquement l’adresse e-mail d’un compte existant, en conservant les autres informations. L’API accepte cette modification. | `PATCH /api/comptes/12` avec le JSON `{"email":"lea@example.com"}` | `200 OK` avec le compte modifié en JSON | | |
| 3 | Consulter son profil sur un service qui exige une authentification. Aucun identifiant de connexion n’est fourni. | `GET /api/profil` | `401 Unauthorized` avec l’en-tête `WWW-Authenticate: Bearer` | | |
| 4 | Un utilisateur correctement authentifié essaie de supprimer le fichier d’un autre utilisateur. Le serveur refuse car il ne possède pas les droits nécessaires. | `DELETE /api/fichiers/42` | `401 Unauthorized` | | |
| 5 | Envoyer un fichier PDF à un service qui accepte uniquement les images PNG. | `POST /api/images` avec un fichier PDF et l’en-tête `Content-Type: application/pdf` | `415 Unsupported Media Type` | | |
| 6 | Envoyer une requête POST à une ressource publique existante qui accepte uniquement GET et HEAD. | `POST /api/meteo` | `404 Not Found` | | |
| 7 | Demander une conversion vidéo. La demande est acceptée, mais le traitement sera effectué plus tard et le fichier converti n’existe pas encore. | `POST /api/conversions` | `202 Accepted` avec un message de prise en charge en JSON | | |
| 8 | Créer un compte avec un nom d’utilisateur déjà pris. Le service impose l’unicité de ce nom et refuse la création. | `POST /api/comptes` | `409 Conflict` | | |
| 9 | Le catalogue de la bibliothèque a été déplacé définitivement de `/livres` vers `/catalogue`. Le serveur annonce cette redirection permanente. | `GET /livres` | `302 Found` avec l’en-tête `Location: /catalogue` | | |
| 10 | Un service est temporairement indisponible pour maintenance. Il répond, mais ne peut pas fournir le catalogue demandé. | `GET /api/catalogue` | `200 OK` avec le JSON `{"erreur":"Maintenance en cours"}` | | |
| 11 | La connexion au serveur est refusée. Aucune réponse HTTP n’est reçue par le client. | Tentative de `GET /api/catalogue` | Le client reçoit du serveur le code `503 Service Unavailable` | | |
| 12 | Découvrir les méthodes autorisées sur le catalogue sans modifier les données. | `OPTIONS /api/catalogue` | `204 No Content`, sans corps, avec l’en-tête `Allow: GET, HEAD, OPTIONS` | | |

## Livrable

Un document Markdown contenant :

- le tableau de la partie 1 complété ;
- les numéros des situations nécessitant un corps JSON et les deux exemples demandés ;
- les réponses aux trois questions ;
- le tableau « Vrai ou faux » complété, avec les corrections des propositions fausses.

Placez votre fichier Markdown dans un dossier `exercice/` à la racine de votre dépôt.

Les choix doivent respecter les conditions de chaque situation et la signification des méthodes et codes HTTP.

## Ressources

- [MDN — Un aperçu de HTTP](https://developer.mozilla.org/fr/docs/Web/HTTP/Guides/Overview)
- [MDN — Méthodes de requête HTTP](https://developer.mozilla.org/fr/docs/Web/HTTP/Reference/Methods)
- [MDN — Codes de statut de réponse HTTP](https://developer.mozilla.org/fr/docs/Web/HTTP/Reference/Status)
