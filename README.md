# tp-coaching-webforce3

Instruction pour la session de coaching

#### exercice 1

Réalisation d'un baclog du projet
une fois le screen fait et ajouté au dossier on peut faire les commandes suivantes

```
git add . (pour ajouter tous les fichiers modifier ou nouveau depuis ledernier commit)
git commit -m "nom_de_mon_commit" (pour commit tous les fichiers ensemble)
git push (pour push le commit dans le repo github)
```

#### exercice 2

Mettre à jour les packages de la VM

- sudo apt update
- sudo apt upgrade

vérifier la version de python3 déjà instalée

- python3 --version

créer un alias python pour appeler python3

- sudo vi /home/ubuntu/.bash_aliases
  - #!/bin/bash
  - alias python='python3'

tester l'alias

- python -v

installer flask pour python

- sudo apt install pip
- sudo pip install flask
- flask --version

#### exercice 3

rechercher le disque dur connecté

- sudo fdisk -l | grep "1 GiB"

formater le disque

- sudo mkfs -t ext4 /dev/vdc

créer un point de montage

- sudo mkdir /home/tp-coaching-webforce3/log

monter le disque sur le point de montage

- sudo mount /dev/vdc /home/tp-coaching-webforce3/log

#### exercice 4

configuration déjà réalisée

#### exercice 5

création du fichier blogs + affectation des droits

- touch blogs.py | sudo chmod +777 blogs.py

ouverture puis on colle le texte à l'intérieur

- nano blogs.py
  ![Alt text](screen-file-blogs.png)

ajouter une variable d'environnement pour le bon fonctionnement

- cd /home/tp-coaching-webforce3
- export FLASK_APP=blogs.py

configurer de manière permanente

- nano / home/ubuntu/.bashrc - export FLASK_APP=blogs.py

mettre en marche la modification

- source /home/ubuntu/.bashrc

ajuster les permissions du répertoire log pour que Flask puisse s'y exécuter

- sudo chmod 777 log

Lancer l'application Flask

- flask run --host=0.0.0.0 -p 30101

Depuis un navigateur, ouvrir l'url

- http://<ip_de_votre_vm>:30101/blogs.

Verifier que le fichier record.log existe

- cd /home/tp-coaching-webforce3/log
- ls

#### exercice 6

on vérifie la présence des firewall de base de linux (ici ufw qui est le plus simple à utiliser)

- ufw --version

on voit qu'il est bine installé on check ensuite son status

- sudo ufw status

On autorise le SSH en entrée pour ne pas perdre l'accès à la VM puis on autorise le port 30101 et fermer le 5000

- sudo ufw allow ssh
- sudo ufw allow 30101
- sudo ufw deny 5000

on rend actif ces règles

- sudo ufw enable

Lancer l'application Flask sur le port 30101

- flask run --host=0.0.0.0 -p 30101

Depuis un navigateur, ouvrir l'url

- http://<ip_de_votre_vm>:30101/blogs.

on constate que l'on a toujours accès

Lancer l'application Flask sur le port 5000

- flask run --host=0.0.0.0 -p 5000

Depuis un navigateur, ouvrir l'url

- http://<ip_de_votre_vm>:5000/blogs.

on constate que le site ne se charge pas et que l'on obtient un timeout



### Ansible
## TP 1
```shell
---
- name: TP ansible 1
  hosts: localhost
  become: true
  tasks:
    - name: Update / Upgrade
      ansible.builtin.apt:
        name: "*"
        state: latest

    - name: Vérifier la version de Python
      debug:
        msg: "{{ ansible_python_version }}"

    - name: Créer un alias bash
      ansible.builtin.lineinfile:
        path: /home/hugues/.bashrc
        line: "alias python='python3'"

    - name: Installer Pip
      ansible.builtin.package:
        name: pip
        state: present
```

## TP 2
# 1 analyser le fichier ansible-2-filtre.yml
On est ici en présence d'un fichier YAML qui respecte la manière d'écreir du langage et devrait pouvoir être interprété sans soucis.
pn se rend compte que l'on a à faire à un playbook pour Ansible qui aura pour bu te dofmrater un disque dur.
les actions s'éffectueront avec l'utilsateurs root sur la machine cible qui est la notre (become:true et host:localhost)
les tâches à effectuer sont ensuite décrite sous tasks
 - get fisk structure, en tant que root lance la commande fdisk -l et enregistre dans get_disk
 - pour la mide au point, on affiche un message de debug ou on affiche le nom du disque dur enregistré dans get_disk grâce au filtre get_device

# 2 Dans la directory filter_plugins etudier le code de la fonction get_device
la fonction get_device prend deux arguments : 
- self
- list_device
# 3 Regardez egalement le fichier ansible.cfg, mettre des commentaires dans le README.md
Ce script est un fichier de configuration pour Ansible. Il est destiné à donner à Ansible ses paramètres par défaults.
- deprecation_warnings=False 
  - désactive les avertissement de dépréciation dans Ansible
- host_key_checking=False
  - désactive la vérification de la clé d'h$ote SSH
- ssh_args = -o ControlMaster=auto -o ControlPersist=30m
  - arguments à utiliser lors de la connection SSH. Ici activation du contrôle de session (CtrlMstr=auto) et durée d'activité de 30 minutes après la deconnexion (Ctrlpersist)
- command_warnings=False 
  - désative les avertissements liés aux commandes dans Ansible
- pipelining=True
  - Active le pipeline d'Ansible permettant une optimisation des performances d'Ansible
- callback_whitelist = profile_tasks 
  - seul le callback profile_tasks permet d'afficher les temps d'execution des tâches d'Ansible
- filter_plugins = filter_plugins
  - spécifie le répertoires ou les plugins de filtres sont situé pour qu'Ansible sache chreché en cas de besoin


