# Exercice 1 — Méthodes et codes HTTP : solutions

Corrigé de l’[exercice sur les méthodes et codes HTTP](exercice-01-methodes-codes-http.md).

Les réponses des parties 1 à 3 suivent le contrat du Foodtruck défini dans l’énoncé. Les situations de la partie 4 concernent des services indépendants.

## 1. Choisir la requête et la réponse

| N° | Situation | Méthode HTTP | Chemin | Code HTTP |
| --- | --- | --- | --- | --- |
| 1 | Consulter la carte des pizzas. | `GET` | `/api/pizzas` | `200 OK` |
| 2 | Consulter les informations de la pizza 7. | `GET` | `/api/pizzas/7` | `200 OK` |
| 3 | Ajouter une pizza avec des données valides. | `POST` | `/api/pizzas` | `201 Created` |
| 4 | Remplacer les informations de la pizza 7. | `PUT` | `/api/pizzas/7` | `200 OK` |
| 5 | Supprimer la pizza 7. | `DELETE` | `/api/pizzas/7` | `204 No Content` |
| 6 | Consulter les informations de la pizza 999. | `GET` | `/api/pizzas/999` | `404 Not Found` |
| 7 | Ajouter une pizza dont le prix est négatif. | `POST` | `/api/pizzas` | `400 Bad Request` |
| 8 | Consulter la carte alors qu’elle ne contient aucune pizza. | `GET` | `/api/pizzas` | `200 OK` |
| 9 | Une erreur interne inattendue empêche l’enregistrement d’une nouvelle pizza valide. | `POST` | `/api/pizzas` | `500 Internal Server Error` |

La modification de la situation 4 renvoie `200` car le contrat prévoit la pizza modifiée en JSON. La suppression de la situation 5 renvoie `204` sans corps. La création de la situation 3 renvoie `201` car une nouvelle ressource a été créée.

## 2. Préparer le corps des requêtes

### 2.1. Situations avec un corps JSON

Les situations **3, 4, 7 et 9** nécessitent un corps de requête JSON.

- **3 :** les informations de la pizza à créer.
- **4 :** les informations modifiables complètes de la pizza à remplacer.
- **7 :** les informations transmises contiennent un prix négatif. Le JSON peut être syntaxiquement valide tout en ne respectant pas les règles de l’API.
- **9 :** les informations transmises sont valides ; l’échec survient pendant leur traitement par le serveur.

Ces requêtes utilisent l’en-tête `Content-Type: application/json`.

Les requêtes GET et DELETE de cet exercice ne nécessitent pas de corps : le chemin indique la ressource visée.

### 2.2. Création d’une pizza

Requête : `POST /api/pizzas`.

```json
{
  "name": "Margherita",
  "price": 14.5
}
```

Le serveur attribue l’identifiant de la nouvelle pizza. Le client ne doit donc pas en fournir dans cet exemple.

### 2.3. Remplacement d’une pizza

Requête : `PUT /api/pizzas/7`.

```json
{
  "name": "Margherita spéciale",
  "price": 16
}
```

Le chemin désigne la pizza 7. Le corps fournit l’ensemble des informations modifiables du modèle minimal utilisé ici : `name` et `price`.

D’autres noms et prix sont acceptables s’ils respectent le contrat de l’énoncé.

## 3. Expliquer ses choix

### 3.1. Une méthode, plusieurs codes de réponse

La méthode exprime l’opération demandée par le client ; le code de réponse décrit le résultat obtenu.

Dans les deux situations, le client demande une lecture avec GET :

- la pizza 7 existe : le serveur renvoie `200 OK` avec ses informations ;
- la pizza 999 n’existe pas : le serveur renvoie `404 Not Found`.

Une méthode HTTP ne détermine donc pas à elle seule le code de réponse.

### 3.2. Une carte vide

Le corps contient un tableau JSON vide :

```json
[]
```

Le code est `200 OK` : la collection `/api/pizzas` existe et sa consultation a réussi. L’absence de pizzas dans cette collection ne signifie pas que la ressource demandée est introuvable.

### 3.3. Une suppression réussie

Non. Le code `204 No Content` impose une réponse sans corps. On ne peut donc pas lui ajouter un message JSON. Voir [MDN — 204 No Content](https://developer.mozilla.org/fr/docs/Web/HTTP/Reference/Status/204).

Dans une autre API, une suppression pourrait renvoyer `200 OK` avec un message JSON. Pour cet exercice, le contrat impose une suppression réussie sans contenu : on conserve donc `204` et on n’envoie aucun corps.

## 4. Vrai ou faux : vérifier un échange HTTP

| N° | Verdict | Correction ou explication |
| --- | --- | --- |
| 1 | **Faux** | Conserver `HEAD /fichiers/guide.pdf` et `200 OK`, mais supprimer le corps de la réponse. HEAD renvoie les en-têtes sans transmettre le fichier. |
| 2 | **Vrai** | PATCH permet une modification partielle. L’adresse e-mail change, les autres informations sont conservées, et `200 OK` accompagne le compte modifié en JSON. |
| 3 | **Vrai** | Le service exige une authentification qui n’a pas été fournie. `401 Unauthorized` et l’en-tête `WWW-Authenticate: Bearer` indiquent ce besoin. |
| 4 | **Faux** | Répondre `403 Forbidden`. L’utilisateur est correctement authentifié, mais ne possède pas les droits nécessaires. Une nouvelle authentification ne résout pas ce manque de droits. |
| 5 | **Vrai** | Le format PDF envoyé n’est pas accepté par le service. `415 Unsupported Media Type` décrit ce refus du format du contenu de la requête. |
| 6 | **Faux** | Répondre `405 Method Not Allowed` avec l’en-tête `Allow: GET, HEAD`. La ressource existe, mais elle n’accepte pas POST. |
| 7 | **Vrai** | `202 Accepted` indique que la demande est prise en charge, mais que son traitement n’est pas encore terminé. Ce code ne garantit pas la réussite finale de la conversion. |
| 8 | **Vrai** | `409 Conflict` correspond au conflit entre le nom demandé et un compte existant, puisque le service impose l’unicité du nom d’utilisateur. |
| 9 | **Faux** | Utiliser `301 Moved Permanently` ou `308 Permanent Redirect`, avec `Location: /catalogue`. `302 Found` annonce une redirection temporaire. Les deux codes permanents sont acceptables pour cette requête GET. |
| 10 | **Faux** | Répondre `503 Service Unavailable`. Le service est temporairement indisponible et ne peut pas satisfaire la demande. Un corps JSON décrivant l’erreur ne justifie pas le code `200`. |
| 11 | **Faux** | Aucun code HTTP n’a été reçu : il s’agit d’une erreur de connexion. Un code `503` supposerait qu’une réponse HTTP ait effectivement été envoyée par un serveur ou un intermédiaire. |
| 12 | **Vrai** | OPTIONS permet de découvrir les options de communication. La réponse `204` est sans corps et l’en-tête `Allow` annonce les méthodes autorisées sur la ressource. |

Pour la situation 6, la réponse 405 doit préciser les méthodes autorisées dans l’en-tête `Allow` : [MDN — 405 Method Not Allowed](https://developer.mozilla.org/fr/docs/Web/HTTP/Reference/Status/405).

Pour la situation 9, le code 308 conserve la méthode lors de la redirection : [MDN — 308 Permanent Redirect](https://developer.mozilla.org/fr/docs/Web/HTTP/Reference/Status/308).

## Références

- [MDN — Un aperçu de HTTP](https://developer.mozilla.org/fr/docs/Web/HTTP/Guides/Overview)
- [MDN — Méthodes de requête HTTP](https://developer.mozilla.org/fr/docs/Web/HTTP/Reference/Methods)
- [MDN — Codes de statut de réponse HTTP](https://developer.mozilla.org/fr/docs/Web/HTTP/Reference/Status)
