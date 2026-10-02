# Identité, organisations et rôles — v0.1

## Séparation des concepts
SARCADE sépare :
1. **authentification** : prouver l'identité via compte SARCADE, OIDC, Google, Apple, passkey, etc.
2. **User** : profil opérationnel stable.
3. **Organization** : structure à laquelle un utilisateur peut appartenir.
4. **Membership** : rattachement et rôles dans une organisation, un événement et éventuellement une équipe.

Aucun mot de passe, token OAuth, secret ou credential n'est transporté dans les objets métier du protocole.

## Rôles initiaux
- viewer
- operator
- team_leader
- dispatcher
- event_manager
- organization_admin
- system_admin

Les permissions effectives seront définies côté serveur. Les transports radio ne doivent jamais être considérés comme une source d'autorisation.

## Multilingue
Le champ locale est une préférence d'affichage. Les valeurs métier restent des codes neutres et sont traduites localement.
