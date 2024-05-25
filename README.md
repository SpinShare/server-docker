# SpinShare Server 
## Components
- App on :80
- Database on :3306
- PhpMyAdmin on :8080

## Docker Setup
### Set Environment
- Copy .env.example to .env
- Set Variables

### Build Docker Image
- ``docker build -t spinshare-web -f .docker/Dockerfile .``

### Run Server
- ``docker compose up -d``

### Install Composer Dependencies
- ``docker exec -it server-docker-web-1 bash``
- ``composer install``

### Create DB
- ``docker exec -it server-docker-web-1 bash``
- ``php bin/console doctrine:schema:create``

### Update DB
- ``docker exec -it server-docker-web-1 bash``
- ``php bin/console doctrine:schema:update --force``