# JARMY — Application d'analyse nutritionnelle

Dépôt principal du projet **JARMY**, regroupant le Frontend (Next.js) et le Backend (microservices Python) via des sous-modules Git.

## Membres du groupe — IZAC

| Membre | Rôle principal |
|---|---|
| Matthieu IZAC | Frontend & expérience utilisateur |
| Anass KABOURI | Architecture microservices & Gateway |
| Youssef BELATAR | Service IA (Kcal), ETL & intégration données |

---

## Prérequis

- [Git](https://git-scm.com/)
- [Docker](https://www.docker.com/) & Docker Compose

---

## Installation

### 1. Cloner le projet avec les sous-modules

```bash
git clone --recursive https://github.com/Swaksm/IZAC_Atelier_Services_Web.git
cd IZAC_Atelier_Services_Web
```

> Si vous avez déjà cloné sans `--recursive`, initialisez les sous-modules :
> ```bash
> git submodule update --init --recursive
> ```

---

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

---

### 3. Lancer le Frontend

Dans un **nouveau terminal** depuis la racine du projet :

```bash
cd Front
docker compose up --build
```

L'application sera accessible sur : **http://localhost:3000**

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
│   │   ├── auth/       # Authentification (port 8004)
│   │   └── admin/      # Administration (port 8006)
│   └── docker-compose.yml
└── Front/              # Sous-module — application Next.js (port 3000)
    └── docker-compose.yml
```

## Stack technique

- **Frontend :** Next.js 16, React 19, Tailwind CSS, shadcn/ui
- **Backend :** Microservices Python, API Gateway
- **IA / NLP :** SpaCy (reconnaissance d'entités nutritionnelles)
- **Base de données :** PostgreSQL (données structurées), MongoDB (logs & historiques)
- **Données :** ETL sur datasets nutritionnels (OpenFoodFacts / Kaggle)
- **Conteneurisation :** Docker Compose

## Variables d'environnement

Les fichiers `.env` nécessaires pour les services (Auth, Meal, Kcal) contiennent des variables sensibles (clés JWT, credentials BDD) transmises séparément.

## URL de production

*(Non disponible pour le moment)*
