# JARMY — Application d'analyse nutritionnelle

Dépôt principal du projet **JARMY**, regroupant le Frontend (Next.js) et le Backend (microservices Python) via des sous-modules Git.

---

## Membres du groupe — IZAC

| Membre | Rôle |
|---|---|
| Matthieu IZAC | Frontend & service web externe (SSO Google OAuth) |
| Anass KABOURI | Architecture microservices, Gateway & mise en production |
| Youssef BELATAR | Base de données NoSQL (MongoDB), ETL & service IA (Kcal) |

---

## Ce que nous développons

JARMY est une application d'analyse nutritionnelle permettant aux utilisateurs de suivre leurs repas, d'obtenir des estimations caloriques par langage naturel et de visualiser leur activité.

### Service web externe — SSO Google OAuth (Matthieu)

L'authentification est déléguée à **Google OAuth 2.0**, un service web externe géré par Google.

**Pourquoi ?** Plutôt que de gérer mots de passe et sessions nous-mêmes, on s'appuie sur l'infrastructure d'identité de Google. L'utilisateur clique "Se connecter avec Google", Google vérifie l'identité et nous renvoie un **ID Token JWT** signé. Le service `auth` vérifie ce token côté backend via la librairie `google-auth`, puis crée ou retrouve le compte utilisateur en base.

**Côté Frontend** (`@react-oauth/google`) : le composant Google renvoie un `credential` (JWT). On l'envoie à `POST /auth/google-login`. Le backend valide le token avec `id_token.verify_oauth2_token()`, extrait l'email/nom/prénom, et crée le compte si besoin. Aucun mot de passe ne transite.

### Base de données NoSQL — MongoDB (Youssef)

En plus de PostgreSQL (données structurées : utilisateurs, repas, aliments), on utilise **MongoDB** pour les logs d'activité utilisateur.

**Pourquoi le NoSQL ici ?** Les logs d'activité ont une structure variable selon l'action :
- Un `login` n'a pas les mêmes champs qu'un `add_meal` ou un `search_food`
- Le champ `detail` est un objet JSON libre, différent à chaque action

Imposer un schéma SQL fixe pour ces données hétérogènes serait contraignant et peu adapté à l'évolution. MongoDB permet de stocker chaque log comme un document JSON indépendant, sans migration de schéma. C'est le cas d'usage typique du NoSQL : **données semi-structurées à fort volume, en écriture rapide (fire-and-forget)**.

**Fonctionnement concret :** quand un utilisateur se connecte ou ajoute un repas, les services `auth` et `meal` envoient de manière asynchrone (thread daemon, sans bloquer la réponse) un document à `activity-logs` (port 8005), qui l'insère dans la collection `activity_logs` de la base `healthai_logs`. Ces logs sont consultables dans la page admin (onglet **Activité MongoDB**) en temps réel.

---

## Organisation du groupe

Chaque membre est responsable d'une brique fonctionnelle de bout en bout :

- **Matthieu** — Frontend Next.js et intégration SSO Google OAuth
- **Youssef** — MongoDB, pipeline ETL et service NLP d'analyse calorique (SpaCy)
- **Anass** — Architecture Gateway, coordination des microservices et déploiement

Communication via **Discord/Teams**, versioning sur **GitHub**.

---

## Étapes de mise en œuvre

1. **SSO** — Intégration Google OAuth 2.0 (Frontend `@react-oauth/google` + backend `google-auth`)
2. **NoSQL** — MongoDB + service `activity-logs` + pipeline ETL pour l'enrichissement nutritionnel
3. **Production** — Conteneurisation Docker Compose et déploiement

---

## Lancer le projet en local

### Prérequis

- [Git](https://git-scm.com/)
- [Docker](https://www.docker.com/) & Docker Compose

### 1. Cloner avec les sous-modules

```bash
git clone --recursive https://github.com/Swaksm/IZAC_Atelier_Services_Web.git
cd IZAC_Atelier_Services_Web
```

> Si déjà cloné sans `--recursive` :
> ```bash
> git submodule update --init --recursive
> ```

### 2. Variables d'environnement

```bash
cp Front/.env.example Front/.env.local   # puis remplir les valeurs
cp Back/.env.example Back/.env           # puis remplir les valeurs
```

> Les valeurs sensibles (Google Client ID, mot de passe BDD) ont été transmises par mail à **daniel.vermonden@gmail.com**.

### 3. Lancer le Backend

```bash
cd Back
docker compose up --build
```

| Service | URL |
|---|---|
| Gateway (point d'entrée API) | http://localhost:8000 |
| Activity Logs (MongoDB) | http://localhost:8005 |
| Adminer (interface SQL) | http://localhost:8080 |

### 4. Lancer le Frontend

```bash
cd Front
docker compose up --build
```

Application : **http://localhost:3000** — Admin : **http://localhost:3000/admin**

---

## Architecture

```
IZAC_Atelier_Services_Web/
├── Back/               # Sous-module (MSPR-backend) — microservices Python/FastAPI
│   └── services/
│       ├── gateway/        # Point d'entrée API (port 8000)
│       ├── auth/           # Authentification + SSO Google (port 8004)
│       ├── meal/           # Journal alimentaire (port 8003)
│       ├── kcal/           # Analyse calorique NLP/SpaCy (port 8001)
│       ├── etl/            # Import & enrichissement données (port 8002)
│       ├── admin/          # Administration (port 8006)
│       └── activity-logs/  # Logs MongoDB fire-and-forget (port 8005)
└── Front/              # Sous-module (MSPR-Frontend) — Next.js 16 / React 19
```

**Stack :** Next.js 16, React 19, shadcn/ui — FastAPI (Python) — PostgreSQL + MongoDB — SpaCy — Docker Compose

---

## URL de production

| Service | URL |
|---|---|
| Frontend (Vercel) | https://mspr-frontend-xi.vercel.app |
| Backend Gateway (Railway) | https://mspr-backend-production.up.railway.app |

---

## Déploiement en production

### Frontend — Vercel

Le frontend est déployé automatiquement sur **Vercel** à chaque push sur `main` du repo `MSPR-Frontend`.

- Build : Next.js détecté automatiquement
- Variables d'environnement à configurer dans Vercel → Settings → Environment Variables :
  ```
  NEXT_PUBLIC_GOOGLE_CLIENT_ID=<Google OAuth Client ID>
  NEXT_PUBLIC_JARMY_API_URL=<URL publique du gateway Railway>
  ```
- Le SSO Google OAuth nécessite d'ajouter l'URL Vercel dans les **Authorized JavaScript origins** sur [console.cloud.google.com](https://console.cloud.google.com) → APIs & Services → Credentials → OAuth 2.0 Client ID

### Backend — Railway

Le backend (microservices Python/FastAPI) est déployé sur **Railway** avec un service par microservice.

**Services déployés :**

| Service Railway | Rôle | Port |
|---|---|---|
| gateway | Point d'entrée API public | 8000 |
| auth | Authentification + SSO Google | 8004 |
| meal | Journal alimentaire | 8003 |
| admin | Administration | 8006 |
| activity-logs | Logs MongoDB | 8005 |
| PostgreSQL | Base de données relationnelle | 5432 |
| MongoDB | Base de données logs NoSQL | 27017 |

**Variables d'environnement Railway à configurer par service :**

- **auth, meal, admin** : `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, `DB_PASSWORD` (depuis le plugin PostgreSQL Railway)
- **activity-logs** : `MONGO_URL` (depuis le plugin MongoDB Railway), `MONGO_DB=healthai_logs`
- **gateway** : URLs internes des services via `*.railway.internal`
- **auth** : `GOOGLE_CLIENT_ID`

Les services communiquent entre eux via le réseau privé Railway (`http://<service>.railway.internal:<port>`).
