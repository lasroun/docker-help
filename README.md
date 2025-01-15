# Docker Cheat Sheet

Ce guide couvre les principales commandes et concepts de Docker, depuis la création d'un Dockerfile jusqu'à la configuration d'un fichier `docker-compose.yml`.

---

## Création d'un Dockerfile

Un `Dockerfile` est un fichier texte contenant les instructions pour construire une image Docker.

### Structure de base d'un Dockerfile
```Dockerfile
# 1. Image de base
FROM node:lastest

# 2. Variables d'environnement
ENV NODE_ENV=production

# 3. Répertoire de travail
WORKDIR /usr/src/app

# 4. Copie des fichiers locaux dans l'image
COPY package*.json ./

# 5. Installation des dépendances
RUN npm install

# 6. Copie du reste des fichiers
COPY . .

# 7. Port exposé
EXPOSE 3000

# 8. Commande à exécuter
CMD ["npm", "start"]
```

### Commandes clés
- `FROM` : Définit l'image de base.
- `WORKDIR` : Définit le répertoire de travail pour les commandes suivantes.
- `COPY` : Copie les fichiers locaux vers l'image.
- `RUN` : Exécute une commande pendant la construction de l'image.
- `CMD` : Spécifie la commande par défaut lors du démarrage du conteneur.
- `EXPOSE` : Informe Docker des ports à exposer.

---

## Gestion des images et conteneurs

### Construction d'une image
```bash
docker build -t mon-application:1.0 .
```
- `-t` : Donne un nom et un tag à l'image.

### Lister les images disponibles
```bash
docker images
```

### Supprimer une image
```bash
docker rmi <image_id>
```

### Lancer un conteneur
```bash
docker run -d -p 8080:3000 --name mon-conteneur mon-application:1.0
```
- `-d` : Mode détaché (en arrière-plan).
- `-p` : Mappe un port local à un port du conteneur.
- `--name` : Attribue un nom au conteneur.

### Lister les conteneurs actifs
```bash
docker ps
```

### Lister tous les conteneurs (même arrêtés)
```bash
docker ps -a
```

### Arrêter un conteneur
```bash
docker stop <container_id>
```

### Supprimer un conteneur
```bash
docker rm <container_id>
```

---

## Docker Compose

`docker-compose.yml` permet de définir et déployer des applications multi-conteneurs.

### Exemple de `docker-compose.yml`
```yaml
services:
  web:
    image: mon-application:1.0
    build:
      context: .
    ports:
      - "8080:3000"
    volumes:
      - ./:/usr/src/app
    environment:
      NODE_ENV: development
    depends_on:
      - db

  db:
    image: postgres:15
    volumes:
      - db-data:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: appdb

volumes:
  db-data:
```

### Commandes Docker Compose
- **Démarrer les services :**
  ```bash
  docker compose up -d
  ```
  
- **Arrêter les services :**
  ```bash
  docker compose down
  ```

- **Recréer les services (par exemple, après modification du `Dockerfile`) :**
  ```bash
  docker compose up --build
  ```

- **Vérifier les logs des services :**
  ```bash
  docker compose logs -f
  ```

- **Lister les services actifs :**
  ```bash
  docker compose ps
  ```

---

## Bonnes pratiques

- **Minimisez la taille des images :** Utilisez des images de base égées (e.g., `node:18-alpine`).
- **Utilisez `.dockerignore` :** Ajoutez un fichier `.dockerignore` pour exclure les fichiers inutiles lors de la construction.
  ```
  node_modules
  *.log
  .env
  ```
- **Versionnez vos images :** Utilisez des tags explicites comme `1.0` ou `latest` pour vos images.
- **Testez vos conteneurs :** Vérifiez le bon fonctionnement des conteneurs avec des commandes comme `docker logs` ou `docker exec`.
  ```bash
  docker exec -it <container_id> sh
  ```

---

Ce cheat sheet fournit les bases essentielles et des exemples concrets pour bien démarrer avec Docker et Docker Compose. Vous pouvez le personnaliser selon vos besoins.

