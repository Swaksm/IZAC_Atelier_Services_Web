# JARMY — Application d'analyse nutritionnelle

Dépôt principal du projet **JARMY**, regroupant le Frontend (Next.js) et le Backend (microservices Python) via des sous-modules Git.

---

## Membres du groupe — IZAC

| Membre | Rôle |
|---|---|
| Matthieu IZAC | Frontend & service web externe (SSO) |
| Anass KABOURI | Architecture microservices, Gateway & mise en production |
| Youssef BELATAR | Base de données NoSQL, ETL & service IA (Kcal) |

---

## Ce que nous développons

### Application
JARMY est une application d'analyse nutritionnelle permettant aux utilisateurs de suivre leurs repas et d'obtenir des informations caloriques à partir du langage naturel.

### Service web externe — SSO (Matthieu)
Nous intégrons un service d'authentification SSO via **Google OAuth 2.0** (`@react-oauth/google`). Ce service web externe permet aux utilisateurs de se connecter sans créer de compte dédié, simplifiant l'accès à l'application et sécurisant la gestion des identités.

### Base de données NoSQL — MongoDB (Youssef)
Nous utilisons **MongoDB** pour stocker les logs d'activité et les historiques de repas. Contrairement à PostgreSQL (utilisé pour les données utilisateurs structurées), MongoDB permet de stocker des structures de repas variées et des métadonnées d'analyse IA sans schéma rigide. Les données nutritionnelles sont enrichies via un pipeline ETL à partir de datasets externes (OpenFoodFacts / Kaggle).

---

## Organisation du groupe

Chaque membre est responsable d'une brique fonctionnelle de bout en bout :

- **Matthieu** — Développement du Frontend (Next.js) et intégration du SSO Google OAuth
- **Youssef** — Mise en place de MongoDB, pipeline ETL et service NLP d'analyse calorique (SpaCy)
- **Anass** — Architecture de la Gateway, coordination des microservices et déploiement en production

Communication quotidienne via **Discord/Teams**, versioning sur **GitHub**.

---

## Étapes de mise en œuvre

Les étapes ont été réalisées dans l'ordre suivant :

1. **SSO** — Mise en place de l'authentification Google OAuth (Frontend + service Auth)
2. **NoSQL** — Intégration de MongoDB et du pipeline ETL pour l'enrichissement des données nutritionnelles
3. **Mise en production** — Conteneurisation complète avec Docker Compose et déploiement

---

## Lancer le projet en local

### Prérequis

- [Git](https://git-scm.com/)
- [Docker](https://www.docker.com/) & Docker Compose

### 1. Cloner le projet avec les sous-modules

```bash
git clone --recursive https://github.com/Swaksm/IZAC_Atelier_Services_Web.git
cd IZAC_Atelier_Services_Web
```

> Si vous avez déjà cloné sans `--recursive` :
> ```bash
> git submodule update --init --recursive
> ```

### 2. Lancer le Backend

```bash
cd Back
docker compose up --build
```

Services démarrés :

| Service | URL locale |
|---|---|
| Gateway (point d'entrée API) | http://localhost:8000 |
| Service Kcal (NLP) | http://localhost:8001 |
| Service ETL | http://localhost:8002 |
| Service Meal | http://localhost:8003 |
| Service Auth | http://localhost:8004 |
| Service Admin | http://localhost:8006 |
| PostgreSQL | localhost:5432 |
| Adminer (interface BDD) | http://localhost:8080 |

### 3. Lancer le Frontend

Dans un **nouveau terminal** depuis la racine du projet :

```bash
cd Front
docker compose up --build
```

Application accessible sur : **http://localhost:3000**

### Variables d'environnement

Les fichiers `.env` nécessaires pour les services (Auth, Meal, Kcal) contiennent des clés JWT et credentials de base de données. Ils vous ont été transmis à l'adresse **daniel.vermonden@gmail.com**.

---

## Architecture

```
IZAC_Atelier_Services_Web/
├── Back/               # Sous-module — microservices Python
│   ├── services/
│   │   ├── gateway/    # API Gateway (port 8000)
│   │   ├── kcal/       # NLP + analyse calorique (port 8001)
│   │   ├── etl/        # Import & enrichissement des données (port 8002)
│   │   ├── meal/       # Gestion des repas (port 8003)
│   │   ├── auth/       # Authentification SSO (port 8004)
│   │   └── admin/      # Administration (port 8006)
│   └── docker-compose.yml
└── Front/              # Sous-module — application Next.js (port 3000)
    └── docker-compose.yml
```

**Stack :** Next.js 16, React 19, Tailwind CSS, shadcn/ui — Python (microservices) — PostgreSQL + MongoDB — SpaCy — Docker Compose

---

## URL de production

*(Non disponible pour le moment)*
