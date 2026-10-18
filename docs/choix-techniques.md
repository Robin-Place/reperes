# Choix de conception de Repères

## Des règles métier indépendantes de l’interface

Un journal paraît simple jusqu’à devoir déplacer une tâche du 31 janvier au mois suivant, conserver une échéance après un changement d’heure ou reprendre une sauvegarde interrompue. Repères isole ces règles pour pouvoir les vérifier sans ouvrir l’interface.

Les dates correspondent à des jours du calendrier, pas à des instants UTC. Les récurrences distinguent une échéance fixe d’un délai calculé depuis la réalisation. Une réalisation tardive ne crée pas une accumulation de copies de la même tâche.

## Deux usages, deux stockages

Le mode local utilise SQLite et conserve les données sur l’ordinateur qui exécute l’application. Le mode hébergé utilise Supabase pour retrouver son compte depuis plusieurs appareils. Les espaces locaux et en ligne sont distincts ; une sauvegarde permet de transférer leur contenu.

Ce choix évite d’imposer un service externe pour essayer l’application localement. Il demande en contrepartie de maintenir les mêmes règles de validation pour les deux stockages. Le mode local est destiné à un ordinateur personnel ; sa base n’est pas un coffre chiffré et reste lisible par une personne ayant accès aux fichiers.

## Des comptes et des photos privés

Le serveur détermine l’identité de l’utilisateur connecté et rattache chaque enregistrement à son propriétaire. En ligne, les règles d’accès de la base limitent aussi les données au compte concerné. Les photos sont stockées dans un espace privé et leur lecture nécessite une authentification.

Avant envoi, les photos sont réduites pour limiter l’espace utilisé et les temps de transfert. Ces versions servent à la consultation ; les originaux doivent être conservés séparément.

## Des sauvegardes transportables

L’export rassemble les éléments d’un compte et leurs photos, sans mot de passe. La restauration réassocie les tags déjà présents et attribue de nouveaux identifiants aux photos. Un second import met à jour les éléments de même identifiant.

La restauration peut être partielle si la connexion s’interrompt. Les éléments enregistrés restent disponibles et l’import peut être relancé. Ce comportement est documenté et testé ; il ne s’agit pas d’une transaction unique couvrant tout le compte.

## Vérifier les évolutions

Les tests couvrent notamment les années bissextiles, les fins de mois, les changements d’heure, la recherche en français et les sauvegardes. Les essais API utilisent deux comptes dans une base locale temporaire pour vérifier les accès, les références et les photos.

Les tests, l’analyse du code et la construction sont automatisés sur GitHub. Les réglages d’authentification du service hébergé et l’usage sur un téléphone réel demandent des vérifications complémentaires.

## Compromis actuels

Repères privilégie un espace personnel simple. L’écran se met à jour à l’actualisation lorsqu’une modification provient d’un autre appareil. Les modifications simultanées du même élément conservent le dernier enregistrement. Le partage entre comptes, les notifications et l’accès hors connexion restent des évolutions possibles.
