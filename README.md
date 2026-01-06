# Kafka avec Kafka-UI - Docker Compose

Ce projet permet de démarrer rapidement un environnement Kafka complet avec une interface web de gestion.

## 📋 Prérequis

- Docker installé ([Documentation](https://docs.docker.com/get-docker/))
- Docker Compose installé ([Documentation](https://docs.docker.com/compose/install/))

## 🚀 Démarrage rapide

### Lancer l'environnement

```bash
docker-compose up -d
```

### Vérifier que tout fonctionne

```bash
docker-compose ps
```

Vous devriez voir 4 conteneurs en cours d'exécution :
- `zookeeper`
- `kafka`
- `kafka-init` (se termine après la création du topic)
- `kafka-ui`

### Accéder à Kafka-UI

Ouvrez votre navigateur et accédez à : **http://localhost:8080**

Vous verrez l'interface de gestion avec le topic `test-topic` déjà créé.

## 📦 Services

| Service | Description | Port(s) |
|---------|-------------|---------|
| **Zookeeper** | Coordination Kafka | 2181 |
| **Kafka** | Broker Kafka | 9092 (externe)<br>29092 (interne) |
| **Kafka-UI** | Interface web de gestion | 8080 |
| **kafka-init** | Création automatique du topic | - |

## 🔧 Configuration

### Topic par défaut

Un topic nommé `test-topic` est automatiquement créé au démarrage avec :
- **3 partitions**
- **Facteur de réplication : 1**

### Connexion à Kafka

**Depuis votre machine hôte :**
```
localhost:9092
```

**Depuis un autre conteneur Docker :**
```
kafka:29092
```

## 📝 Commandes utiles

### Voir les logs

```bash
# Tous les services
docker-compose logs -f

# Un service spécifique
docker-compose logs -f kafka
docker-compose logs -f kafka-ui
```

### Arrêter l'environnement

```bash
docker-compose down
```

### Arrêter et supprimer les volumes (données effacées)

```bash
docker-compose down -v
```

### Redémarrer un service

```bash
docker-compose restart kafka
```

## 🛠️ Opérations Kafka

### Créer un nouveau topic

```bash
docker exec -it kafka kafka-topics --bootstrap-server kafka:29092 \
  --create --topic mon-topic --partitions 3 --replication-factor 1
```

### Lister les topics

```bash
docker exec -it kafka kafka-topics --bootstrap-server kafka:29092 --list
```

### Produire des messages

```bash
docker exec -it kafka kafka-console-producer --bootstrap-server kafka:29092 \
  --topic test-topic
```

Tapez vos messages, appuyez sur Entrée après chaque message, puis Ctrl+C pour quitter.

### Consommer des messages

```bash
docker exec -it kafka kafka-console-consumer --bootstrap-server kafka:29092 \
  --topic test-topic --from-beginning
```

Appuyez sur Ctrl+C pour quitter.

### Décrire un topic

```bash
docker exec -it kafka kafka-topics --bootstrap-server kafka:29092 \
  --describe --topic test-topic
```

### Supprimer un topic

```bash
docker exec -it kafka kafka-topics --bootstrap-server kafka:29092 \
  --delete --topic test-topic
```

## 🐛 Dépannage

### Les conteneurs ne démarrent pas

Vérifiez que les ports ne sont pas déjà utilisés :
```bash
# Linux/Mac
lsof -i :9092
lsof -i :8080

# Windows (PowerShell)
netstat -ano | findstr :9092
netstat -ano | findstr :8080
```

### Kafka-UI ne se connecte pas

1. Vérifiez que Kafka est bien démarré :
```bash
docker-compose logs kafka
```

2. Attendez quelques secondes après le démarrage, Kafka peut prendre un peu de temps à être prêt

3. Redémarrez Kafka-UI :
```bash
docker-compose restart kafka-ui
```

### Le topic test-topic n'est pas créé

Vérifiez les logs du conteneur kafka-init :
```bash
docker-compose logs kafka-init
```

Vous devriez voir le message "Topic créé avec succès!"

## 📚 Ressources

- [Documentation Kafka](https://kafka.apache.org/documentation/)
- [Kafka-UI GitHub](https://github.com/provectus/kafka-ui)
- [Confluent Kafka Docker](https://docs.confluent.io/platform/current/installation/docker/image-reference.html)

## 📄 Licence

Ce projet est fourni tel quel pour usage personnel et éducatif.