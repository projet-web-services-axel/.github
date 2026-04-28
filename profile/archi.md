# Architecture

## Front

Site web en Next.js, framework moderne et simple à utiliser.

## Microservices

Toutes les APIs, à l'exception de la Gateway, sont faites en Go à partir de la librairie Fiber qui permet une création rapide et efficace. Les performances de Fiber sont excellentes et la customisation laisse la liberté de gérer ses microservices comme souhaité.

### Gateway

La Gateway utilise GraphQL pour permettre au front de ne récupérer que les données dont il a besoin avec des performances élevées.

### Chat (WS)

Sur le site il y a un chat pour les participants de chaque tournoi, des WebSocket sont utilisés pour communiquer de manière instantanée.

### Cache

Afin de permettre de récupérer rapidement la liste des tournois lors de la navigation sur le site, un cache avec Redis le garde en mémoire.

### Pub/Sub

Redis est également utilisé pour publier un évènement à l'inscription d'un joueur à un tournoi, afin que le service de notification réagisse.

### Server-sent event

Quand le service de notification reçoit un évènement, il envoie par server-sent event (SSE) l'information de l'inscription afin d'afficher une notification sur le site à l'utilisateur.

### gRPC

Lors de l'inscription à un tournoi, l'information est également envoyé à l'API des historiques de participation des joueurs afin que le site le récupère.

## BDD

Une base de données PostgreSQL est utilisé pour sa très bonne compatibilité avec Go qui assure des bonnes performances.

### Tournament table

|      Field       |     Type     |
|:----------------:|:------------:|
|        id        |     UUID     |
|      title       | VARCHAR(255) |
|       game       | VARCHAR(255) |
|       date       |  TIMESTAMP   |
|     location     | VARCHAR(255) |
|   participants   |     INT      |
| max_participants |     INT      |
|      prize       | VARCHAR(255) |
|      status      | VARCHAR(50)  |
|   description    |     TEXT     |
|      rules       |    TEXT[]    |
|   organizer_id   |     UUID     |
|    created_at    |  TIMESTAMP   |
|    updated_at    |  TIMESTAMP   |

### User Table
|   Field    |     Type     |
|:----------:|:------------:|
|     id     |     UUID     |
|    name    | VARCHAR(255) |
|   email    | VARCHAR(255) |
|  password  | VARCHAR(255) |
|    role    | VARCHAR(50)  |
| created_at |  TIMESTAMP   |
| updated_at |  TIMESTAMP   |

### Registrations table

|     Field     |    Type     |
|:-------------:|:-----------:|
|      id       |    UUID     |
|    user_id    |    UUID     |
| tournament_id |    UUID     |
|    status     | VARCHAR(50) |
| registered_at |  TIMESTAMP  |
|  updated_at   |  TIMESTAMP  |

### Messages table

|   Field    |     Type     |
|:----------:|:------------:|
|     id     |     UUID     |
|  user_id   |     UUID     |
| channel_id | VARCHAR(255) |
|  content   |     TEXT     |
| timestamp  |  TIMESTAMP   |

### Notifications table

|   Field   |     Type     |
|:---------:|:------------:|
|    id     |     UUID     |
|  user_id  |     UUID     |
|   type    | VARCHAR(50)  |
|   title   | VARCHAR(255) |
|  message  |     TEXT     |
|   read    |   BOOLEAN    |
| timestamp |  TIMESTAMP   |
