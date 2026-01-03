# Joget DX – Back-office clés en main

Dépôt minimal pour déployer **Joget DX** (mode all-in-one) avec **PostgreSQL** via Docker Compose. L’objectif est de disposer d’un back-office métier prêt à démontrer à des collectivités locales : architecture simple, exploitable par des citizen users et sans dépendances superflues.

## Prérequis
- Docker + Docker Compose

## Démarrage (3 commandes maximum)
```bash
cp .env.example .env
# Ajustez les variables au besoin puis lancez :
docker compose up -d
```
Joget sera disponible dès que les conteneurs sont démarrés.

## Accès par défaut
- URL : http://localhost:8080/
- Identifiants initiaux : `admin` / `admin`

## Structure du dépôt
- `docker-compose.yml` : orchestration Joget DX + PostgreSQL, volumes persistants.
- `.env.example` : variables d’environnement à copier dans `.env`.
- `README.md` : ce guide.

## Notes d’architecture
- **Blackbox back-office** : uniquement Joget et PostgreSQL.
- **Persistance** : volumes Docker `db_data` et `joget_data` pour conserver la base et les artefacts.
- **PostgreSQL unique** : prêt pour héberger plusieurs applications Joget (multi-collectivités logique) via la même instance.
- **Aucune surcouche** : pas de Kubernetes, pas de scripts auxiliaires, configuration lisible par une DSI.

## Arrêt et maintenance
- Arrêt : `docker compose down`
- Mise à jour d’image : `docker compose pull` puis `docker compose up -d`
