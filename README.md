# Stack Saltcorn + PostgreSQL + Nginx (Docker Compose)

Infrastructure minimale et prête à déployer pour exécuter Saltcorn derrière Nginx avec une base PostgreSQL.

## Prérequis
- Docker + Docker Compose Plugin (ou Docker Desktop)
- Accès au port 80 sur la machine cible (VPS)

## Arborescence
```
.
├─ docker-compose.yml
├─ nginx/
│  └─ conf.d/
│     └─ default.conf
├─ .env.example
└─ README.md
```

## Variables d'environnement
Copiez `.env.example` vers `.env` puis ajustez les valeurs :
- `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB` : identifiants PostgreSQL.
- `DATABASE_URL` : URL de connexion utilisée par Saltcorn (`postgres://USER:PASSWORD@postgres:5432/DB`).

## Déploiement
1) Préparer les variables :
```bash
cp .env.example .env
# éditez .env pour changer les identifiants si besoin
```

2) Lancer les services :
```bash
docker compose up -d
```

Déploiement direct via une URL `docker-compose.yml` (GitHub raw ou équivalent) :
```bash
docker compose -f https://exemple.tld/path/to/docker-compose.yml --env-file .env up -d
```

## Services
- **postgres** : PostgreSQL 16 avec volume nommé `pg_data`.
- **saltcorn** : image officielle `saltcorn/saltcorn`, démarrée avec `saltcorn serve`.
- **nginx** : reverse proxy exposant uniquement le port 80 et prêt pour Let’s Encrypt (webroot `/var/www/letsencrypt` via volume nommé `letsencrypt_challenges`).

Tous les services partagent le réseau interne `app_net`. Les volumes `pg_data`, `saltcorn_data` (fichiers utilisateurs Saltcorn) et `letsencrypt_challenges` sont nommés pour la persistance.

## Accès
Une fois `docker compose up -d` terminé, ouvrez `http://<ip_ou_domaine>` pour accéder à l'interface Saltcorn.
