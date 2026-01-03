# Déploiement Budibase + PostgreSQL (Docker Compose minimal)

Infrastructure minimale prête pour un VPS (Hostinger ou équivalent) afin d'exécuter Budibase avec une base PostgreSQL persistante. Les images sont tirées depuis Docker Hub (aucune construction locale) et fonctionnent avec `docker compose up -d`.

## Prérequis
- Docker et le plugin Docker Compose installés sur le VPS
- Accès au port HTTP que vous souhaitez exposer (80 en production, 8080 en local par défaut)

## Arborescence
```
.
├─ docker-compose.yml
├─ .env.example
└─ README.md
```

## Configuration
1. Copier le fichier d'exemple puis ajuster les valeurs :
```bash
cp .env.example .env
# éditez .env pour changer les mots de passe et le port exposé
```
2. Variables principales dans `.env` :
   - `POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD` : base, utilisateur et mot de passe PostgreSQL.
   - `BUDIBASE_PORT` : port interne Budibase (dans le conteneur), par défaut `10000`.
   - `PUBLIC_HTTP_PORT` : port HTTP exposé sur l'hôte (ex. `80` pour la prod ou `8080` pour un test local).

## Démarrage rapide
```bash
docker compose up -d
```
Les volumes sont créés automatiquement (volume nommé `pg_data` pour PostgreSQL) et le réseau Docker reste interne ; seul Budibase est exposé sur le port HTTP choisi.

## Accès
- URL : `http://<adresse_du_vps>:<PUBLIC_HTTP_PORT>` (ex. `http://serveur:8080` par défaut).
- Budibase utilise PostgreSQL pour son stockage interne ; aucune autre base n'est nécessaire.

## Sécurité et mots de passe
- Changez immédiatement `POSTGRES_PASSWORD` dans `.env` avant un déploiement public.
- Pour modifier ultérieurement les identifiants, mettez à jour `.env` puis relancez :
```bash
docker compose down
docker compose up -d
```

## Sauvegarde rapide de la base
Exemple de sauvegarde au format SQL (fichier `backup.sql` généré sur l'hôte) :
```bash
docker compose exec postgres sh -c 'pg_dump -U "$POSTGRES_USER" "$POSTGRES_DB"' > backup.sql
```
Adaptez le nom du fichier ou script de rotation selon vos besoins. Restaurations possibles avec `psql` ou `pg_restore` selon le format utilisé.
