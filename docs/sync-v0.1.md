# Synchronisation, ACK et déduplication — v0.1

## Identifiants
Chaque objet créé hors ligne reçoit immédiatement un identifiant globalement unique généré par le client. Un objet conserve son identifiant pendant toute sa durée de vie.

## Horodatage
Les timestamps transmis sont ISO 8601 UTC. L'heure client ne sert pas seule à arbitrer un conflit : le serveur conserve également l'heure de réception.

## Idempotence et déduplication
Une opération de création portant un identifiant déjà connu est idempotente si son contenu est identique. Si le contenu diverge, le serveur signale un conflit au lieu de créer un doublon.

## ACK
L'ACK applicatif référence explicitement message_id et actor_id. Les états initiaux sont received, read, accepted et rejected. Un ACK de transport ne doit pas être confondu avec un ACK métier.

## File offline
Le client conserve une outbox persistante. Chaque opération possède :
- operation_id
- object_id
- object_type
- action
- client_time
- payload
- retry_count

Après reconnexion, les opérations sont rejouées dans l'ordre local. Le serveur répond par accepted, duplicate, conflict ou rejected.

## Reprise
Le client conserve un curseur de synchronisation serveur. Il demande les changements depuis ce curseur. Le serveur renvoie un nouveau curseur seulement lorsque le lot a été traité.

## Conflits
V0.1 privilégie des règles simples :
- positions : append-only
- messages et ACK : append-only
- POI : dernière version serveur + conflit explicite si modifications concurrentes
- Event/Team : modification contrôlée par version
- suppression : tombstone plutôt que disparition immédiate

Aucun last-write-wins silencieux n'est autorisé pour les objets opérationnels sensibles.

## Bas débit
Le modèle de synchronisation est indépendant du transport. Une Gateway pourra fragmenter, compacter ou stocker/transférer les opérations sans changer leurs identifiants ni leur sémantique.
