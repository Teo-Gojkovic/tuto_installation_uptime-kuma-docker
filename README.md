## Comment télécherger et utiliser [Uptime Kuma](https://github.com/louislam/uptime-kuma) avec docker

Uptime kuma est un service de monitoring opensource qui vous permet de surveiller l'état de vos services (cloud / serveur minecraft / plex ...) et il peux vous notifier via plusieurs moyens comme par mail avec le protocole SMTP ou via discord.

## Installation de docker sur Ubuntu Server
Le paquet d'installation de Docker disponible dans le dépôt officiel d'Ubuntu peut ne pas être la dernière version. Pour être sûr d'obtenir la dernière version, nous allons installer Docker à partir du dépôt officiel de Docker. Pour ce faire, nous allons ajouter une nouvelle source de paquets, ajouter la clé GPG de Docker pour nous assurer que les téléchargements sont valides, puis installer le paquet.

Tout d'abord, mettez à jour votre liste de paquets existante :
```bash
sudo apt update
```
Ensuite, installez quelques paquets prérequis qui permettent à apt d'utiliser des paquets via HTTPS :
```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
Ajoutez ensuite la clé GPG du dépôt officiel de Docker à votre système :
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
Ajoutez le dépôt Docker aux sources APT :
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
Mettez à nouveau à jour votre liste de paquets existante pour que l'ajout soit reconnu :
```bash
sudo apt update
```
Assurez-vous que vous êtes sur le point d'installer à partir du repo Docker au lieu du repo Ubuntu par défaut :
```bash
apt-cache policy docker-ce
```
Vous obtiendrez un résultat comme celui-ci, bien que le numéro de version de Docker puisse être différent :
```bash
docker-ce:
  Installed: (none)
  Candidate: 5:20.10.14~3-0~ubuntu-jammy
  Version table:
     5:20.10.14~3-0~ubuntu-jammy 500
        500 https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
     5:20.10.13~3-0~ubuntu-jammy 500
        500 https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
```
Notez que `docker-ce` n'est pas installé, mais que le candidat à l'installation provient du dépôt Docker pour Ubuntu 22.04 (`jammy`).

Enfin, installez Docker :
```bash
sudo apt install docker-ce
```
Docker devrait maintenant être installé, le démon démarré, et le processus activé pour démarrer au démarrage. Vérifiez qu'il fonctionne :
```bash
sudo systemctl status docker
```
La sortie devrait être similaire à la suivante, montrant que le service est actif et fonctionne :
```bash
Output
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2022-04-01 21:30:25 UTC; 22s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 7854 (dockerd)
      Tasks: 7
     Memory: 38.3M
        CPU: 340ms
     CGroup: /system.slice/docker.service
             └─7854 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```

Voila vous avez parfaitement installer docker.

## Déploment du contenair de [Uptime Kuma](https://github.com/louislam/uptime-kuma)
Pour déployer Uptime Kuma executez simplement cette commande :
```bash
docker run -d --restart=always -p 3001:3001 -v uptime-kuma:/app/data --name uptime-kuma louislam/uptime-kuma:1
```

Mainteant vous pouvez allez sur le site en local en tapan ***@iphost*:3001** sur votre navigateur.

## Auteur

[![Teo GOJKOVIC](https://img.shields.io/badge/Louis_Lam-222e45?style=for-the-badge&logo=github&logoColor=white)](https://github.com/louislam)
