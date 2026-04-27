# MSPR - Application d'analyse nutritionnelle

C'est une application complète permettant de suivre et d'analyser l'alimentation via une architecture microservices.

**Membres du groupe :** Youssef, Anass, Matthieu.

## Architecture Microservices (FastAPI)
*   **Gateway :** Point d'entrée principal.
*   **Auth :** Authentification.
*   **Meal :** Gestion des repas et utilisateurs.
*   **Kcal :** Analyse nutritionnelle automatique (via texte libre).
*   **ETL :** Traitement des données.
*   **DB :** PostgreSQL (base principale).

## Technologies
*   **Backend :** Python (FastAPI)
*   **Frontend :** TypeScript
*   **Base de données :** PostgreSQL & MongoDB
*   **Déploiement :** Docker / Docker Compose

## Installation
Le projet utilise Docker Compose pour le déploiement :
```bash
docker-compose up --build
```
