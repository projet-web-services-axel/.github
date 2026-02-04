# Site d'organisation de tournoi esport

Projet par Axel Senecal.

## TODO List

- [ ] Site web
- [ ] API Gateway
- [ ] Message broker
- [ ] Base de données
- [ ] Service tournoi
- [ ] Service notification
- [ ] Service chat
- [ ] Service utilisateur
- [ ] Dockeriser l'ensemble du projet

## Site web

Site web en NextJS, servira de front end.  
Communique directement avec l'API Gateway.

## API Gateway

API en Golang, servira de gateway entre le site web et les micro services.  
Utilisera du GraphQL avec un système d'authentification.

## Message broker

Avoir de la programmtion évènementielle entre les micro services.  
Utilisera Redis Pub/Sub.

## Base de données

Base de données PostgreSQL pour stocker les données des tournois, utilisateurs, inscriptions, etc.  
Puis un cache Redis pour les statistiques.

## Service tournoi

Micro services en Golang, API avec Fiber.  
Depuis un compte organisateur, permettre la création et la gestion des tournois, ainsi que la visualisation du dashboard.  
Depuis un compte utilisateur, permettre l'inscription aux tournois &rarr; envoie un évènement aux services profil utilisateur et notification.  
Suivre les places restantes d'un tournoi en temps réel.

## Service notification

Micro services en Golang, utilisera un server-sent events.
Reçoit un évènement du service tournoi &rarr; envoie des notifications aux joueurs par rapport aux tournois inscrits.  

## Service chat

Web socket pour que les joueurs puissent discuter entre eux.

## Service profil utilisateur

Historique des tournois inscrits avec les résultats pour ceux terminés.  
Reçoit un évènement du service tournoi.

## Dockeriser l'ensemble du projet

L'ensemble du projet doit être exécutable via un docker compose.

## Architecture

![Architecture](web_services_architecture.svg)