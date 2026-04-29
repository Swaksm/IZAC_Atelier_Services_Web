
### Installation
1.  Cloner le projet et les sous-modules :
    ```bash
    git clone --recursive https://github.com/Swaksm/IZAC_Atelier_Services_Web.git
    cd IZAC_Atelier_Services_Web
    ```
2.  Lancer chaque service  :
    ```bash
    docker-compose up --build
    `

# MSPR - Application d'analyse nutritionnelle (JARMY)

Ce dépôt est le point d'entrée global de l'application **JARMY**, regroupant les services Backend et Frontend.

## 👥 Membres du groupe : IZAC
*   **Matthieu IZAC**
*   **Anass KABOURI**
*   **Youssef BELATAR**

## 🚀 Développement du projet

### Utilisation de la BDD NoSQL
Nous intégrons **MongoDB** pour la gestion flexible des logs d'activité et des historiques de repas. Contrairement à PostgreSQL (utilisé pour les données structurées comme les utilisateurs), MongoDB nous permet de stocker des structures de repas variées et des métadonnées d'analyse IA sans schéma rigide, facilitant l'évolution du service **Meal**.

### Services Web Externes
*   **API de Nutrition (via ETL) :** Exploitation de datasets externes (type OpenFoodFacts ou Kaggle) pour enrichir notre base de connaissances nutritionnelles.
*   **Modèle SpaCy (Service Kcal) :** Utilisation de bibliothèques de NLP pour transformer le langage naturel en données structurées exploitables par nos services.

## 🤝 Organisation du groupe
Nous utilisons une approche agile simplifiée :
*   **Matthieu :** Focus sur le Frontend et l'expérience utilisateur.
*   **Anass :** Focus sur l'architecture Microservices et la Gateway.
*   **Youssef :** Focus sur le service IA (Kcal) et l'intégration des données (ETL/DB).
*   **Outils :** GitHub pour le versioning, Discord/Teams pour la communication quotidienne.

## 📅 Étapes de mise en œuvre (Roadmap)
1.  **Phase 1 :** Mise en place de l'architecture microservices (Gateway + Auth).
2.  **Phase 2 :** Développement du moteur de reconnaissance d'entités (NLP) avec SpaCy.
3.  **Phase 3 :** Intégration de MongoDB pour le stockage des historiques.
4.  **Phase 4 :** Connexion du Frontend au backend via la Gateway.
5.  **Phase 5 :** Conteneurisation complète avec Docker Compose pour un déploiement unifié.

## 💻 Lancement du projet en local

### Prérequis
*   Docker & Docker Compose
*   Git
``

### Configuration (Variables d'environnement)
Les fichiers `.env` sont nécessaires pour chaque microservice (Auth, Meal, Kcal).
**Note :** Les variables sensibles (clés JWT, credentials DB) vous ont été transmises par mail (daniel.vermonden@gmail.com).

## 🌐 URL de Production
*(Pas encore d'URL de production publique disponible pour le moment).*
