# nspawn-client: Crée un container dédié pour travailler sur un réseau "client"

## Cas d'usage

- Vous êtes consultant informatique
- Vous travailler chez vos clients avec votre laptop personnel
- Vous ne voulez pas mélanger les données clientes et vos informations personnelles

## Fonctionnalités

- Le container est isolé aux niveaux :
  - stockage
  - réseau
  - process
  - mémoire
- Le container reste dans le même "username-space"
- Le container accède à X11
- Le container dispose d'une interface réseaux physique dédiée
- Le reste de votre machine peut avoir accès à un autre réseau (type partage de connection avec votre portable)

## Installation des pré-requis

```bash
sudo apt-get update
sudo apt-get install -y git ansible
git clone https://github.com/sebt3/nspawn-client.git
cd nspawn-client
ansible-playbook -i config.ini install.yaml -K
```

## Création d'un container client

Validez, modifiez le contenu du fichier `config.ini` puis :

```bash
ansible-playbook -i config.ini create.yaml -K
```

## Utiliser ce container client

Disons que vous avez appelé le client (dans le fichier `config.ini`) **myClient**.

### Le démarrer

Il est important de déconnecter l'interface dédiée avant de lancer le container (probablement `ifdown eth0`), puis :

```bash
sudo systemctl start systemd-nspawn@myClient
```

### S'y connecter

```bash
sudo machinectl login myClient
```

puis utiliser vos login/pass de votre machine.

### Première configuration

Une fois connecté au container, il est recommandé de faire les manipulations suivantes :

```bash
sudo dpkg-reconfigure locales
```

Et choisissez la langue française.

### Lancement du panel

Une fois connecté au container, pour lancer `plank` :

```bash
nohup plank &
```

On lance la configuration de `plank` avec un [ctrl]+click droit dessus (initialement en bas, caché).
L'émulateur de terminal `terminology` est installé par défaut dans le conteneur.

### Arrêter le contener
```bash
sudo systemctl stop systemd-nspawn@myClient
```
