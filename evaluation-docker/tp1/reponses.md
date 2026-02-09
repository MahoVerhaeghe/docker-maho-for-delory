# TP1 : Manipulation des Conteneurs

## Contexte

Vous venez d'être embauché comme DevOps junior dans une startup. Votre première mission est de démontrer votre maîtrise des commandes Docker de base en manipulant des conteneurs.

---

## Exercice 1 : Premiers pas

### 1.1 Vérification de l'installation 

Affichez la version de Docker installée et notez-la dans votre fichier de réponses.

**Commande attendue :**

```bash
docker --version
```

### 1.2 Téléchargement d'images 

Téléchargez les images suivantes :
- `nginx:alpine`
- `redis:7-alpine`

**Questions :**

- Quelle est la taille de chaque image ?
```bash
docker images
```
    `nginx:alpine` => 62.1Mb
    `redis:7-alpine` => 41.4Mb
- Pourquoi utilise-t-on des images `alpine` ?
    Plus légère

### 1.3 Liste des images 

Affichez la liste de toutes les images présentes sur votre système.

**Questions :**

- Quelle commande avez-vous utilisée ?
```bash
docker images [-ls]
```
- Combien d'images sont présentes ?
    24

---

## Exercice 2 : Gestion des conteneurs 

### 2.1 Lancer un conteneur Nginx 

Lancez un conteneur Nginx avec les caractéristiques suivantes :
- Nom : `web-eval`
- Mode détaché
- Port 8080 de l'hôte mappé vers le port 80 du conteneur

**Vérification :** Accédez à `http://localhost:8080` dans votre navigateur.

### 2.2 Inspection du conteneur 

Répondez aux questions suivantes sur le conteneur `web-eval` :
- Quelle est son adresse IP ?
    172.17.0.2
- Quel est son état (status) ?
    running
- Quand a-t-il été créé ?
    2026-02-09T09:33:11.624756727Z

### 2.3 Logs et processus 

- Affichez les 10 dernières lignes de logs du conteneur
```bash
docker logs --tail 10 web-eval
```
2026/02/09 09:33:12 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2026/02/09 09:33:12 [notice] 1#1: start worker processes
2026/02/09 09:33:12 [notice] 1#1: start worker process 30
2026/02/09 09:33:12 [notice] 1#1: start worker process 31
2026/02/09 09:33:12 [notice] 1#1: start worker process 32
2026/02/09 09:33:12 [notice] 1#1: start worker process 33
2026/02/09 09:33:12 [notice] 1#1: start worker process 34
2026/02/09 09:33:12 [notice] 1#1: start worker process 35
2026/02/09 09:33:12 [notice] 1#1: start worker process 36
2026/02/09 09:33:12 [notice] 1#1: start worker process 37
- Affichez les processus en cours d'exécution dans le conteneur
```bash
docker top web-eval
```
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                482                 459                 0                   09:33               ?                   00:00:00            nginx: master process nginx -g daemon off;
statd               526                 482                 0                   09:33               ?                   00:00:00            nginx: worker process
statd               527                 482                 0                   09:33               ?                   00:00:00            nginx: worker process
statd               528                 482                 0                   09:33               ?                   00:00:00            nginx: worker process
statd               529                 482                 0                   09:33               ?                   00:00:00            nginx: worker process
statd               530                 482                 0                   09:33               ?                   00:00:00            nginx: worker process
statd               531                 482                 0                   09:33               ?                   00:00:00            nginx: worker process
statd               532                 482                 0                   09:33               ?                   00:00:00            nginx: worker process
statd               533                 482                 0                   09:33               ?                   00:00:00            nginx: worker process

### 2.4 Exécution de commandes 

Exécutez les actions suivantes dans le conteneur `web-eval` :
1. Ouvrez un shell interactif
```bash
docker exec -it web-eval /bin/sh
```
2. Créez un fichier `/tmp/evaluation.txt` contenant votre nom
```shell
echo "Maho Verhaeghe" > /tmp/evaluation.txt
```
3. Vérifiez que le fichier existe
```shell
ls /tmp/evaluation.txt
cat /tmp/evaluation.txt
```
Reponse:
/tmp/evaluation.txt
Maho Verhaeghe
4. Quittez le shell
```shell
exit
```

---

## Exercice 3 : Cycle de vie 

### 3.1 Arrêt et redémarrage 

- Arrêtez le conteneur `web-eval`
- Vérifiez qu'il est bien arrêté
- Redémarrez-le
- Vérifiez que le fichier `/tmp/evaluation.txt` existe toujours
```bash
docker exec web-eval cat /tmp/evaluation.txt
```
**Question :** Le fichier existe-t-il toujours ? Pourquoi ?
Le conteneur conserve les modifications même lorsqu'on le ferme/arrête

### 3.2 Création d'un conteneur Redis 

Lancez un conteneur Redis avec :
- Nom : `cache-eval`
- Mode détaché
- Pas de mapping de port

Connectez-vous au CLI Redis et exécutez :

```
SET evaluation "reussie"
GET evaluation
```

### 3.3 Gestion multiple 

**Questions :**
- Quelles commandes avez-vous utilisées ?
- Listez tous les conteneurs (actifs et inactifs)
```bash
docker ps -a
```
- Arrêtez tous les conteneurs en une seule commande
```bash
docker stop $(docker ps -a -q)
```
- Supprimez tous les conteneurs arrêtés en une seule commande
```bash
docker rm $(docker ps -a -q)
```
- Quelle est la différence entre `docker stop` et `docker rm` ?
    docker stop arrête un conteneur qui est lancé (enregistre les modifs comme on a vu avant), docker rm le supprime totalement

---

## Exercice 4 : Volumes et persistance 

### 4.1 Création d'un volume 
Créez un volume Docker nommé `data-eval`.

### 4.2 Utilisation du volume 

Lancez un conteneur `alpine` qui :
- Monte le volume `data-eval` sur `/data`
- Crée un fichier `/data/persistant.txt` avec du contenu
- Se termine après exécution
```bash
docker run --rm -v data-eval:/data alpine sh -c "echo 'Maho Verhaeghe Volume' > /data/persistant.txt"
```

### 4.3 Vérification de la persistance 

- Lancez un nouveau conteneur `alpine` montant le même volume
- Vérifiez que le fichier `persistant.txt` existe et contient les données
```bash
docker run --rm -v data-eval:/data alpine cat /data/persistant.txt
```

**Question :** Expliquez pourquoi les données persistent entre les conteneurs.
Les volumes ne fonctionnent pas comme les conteneurs. Les données sont eenregistrés dans un volume et non dans les fichiers d'un counteur (malgré la suppression du conteneur après execution)

---

## Nettoyage

À la fin du TP, nettoyez votre environnement :

```bash
# Supprimez tous les conteneurs créés
docker stop $(docker ps -q)
docker rm $(docker ps -aq)
# Supprimez le volume créé
docker volume rm data-eval
# Conservez les images pour les TPs suivants
```

