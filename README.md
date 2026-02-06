# Groupe23-Deploiement-d-un-serveur-de-Base-de-donn-es-avec-replication

[![Platform: Ubuntu](https://img.shields.io/badge/Platform-Ubuntu-orange.svg)](https://ubuntu.com/)
[![Database: MariaDB](https://img.shields.io/badge/Database-MariaDB-blue.svg)](https://mariadb.org/)

##Présentation du Projet
Ce projet a été réalisé dans le cadre du cours de **Système d'Exploitation** (L2 LMD) à l'**Université de Kinshasa (UNIKIN)**, Mention Mathématiques, Statistique et Informatique.

L'objectif est de mettre en place une architecture de base de données redondante. En configurant une réplication **Maître-Esclave**, nous assurons la haute disponibilité des données : toute modification effectuée sur le serveur principal (Maître) est instantanément répercutée sur le serveur secondaire (Esclave).

## 👥 Membres du Groupe (N°23)
* NGALAMULUME TSHIMANGA Jonas
* MPONGO ILEMBA Peter
* LOKALEMA LOMENGO Uriel
* KABANGU EYATO Bradel
* KABENGELE MUDINGAY Sylvain
* MUTOTODI SAKUAKU Agrée
* MANDI MABUNGA Jedidia
* MANGAYA NLANDU Emdy
* MBONGO TSUKA Geoffrey
* MUKENDI MUKENGESHAYI Sunamite

**Sous la direction du :** Prof. Dr Kasengedia Motumbe Pierre.
**Collaborateurs :** Dr. Junior Kaningini, Ass. Ferdinand Djungu & Fipa Bukusu.

## Guide d'Installation

### 1. Prérequis
* Deux machines (physiques ou virtuelles) sous Ubuntu.
* Accès SSH ou Terminal.
* MariaDB Server installé (`sudo apt install mariadb-server`).

### 2. Configuration du Maître (Master)
Éditez le fichier `/etc/mysql/mariadb.conf.d/50-server.cnf` :
```ini
[mysqld]
server-id = 1
log_bin = /var/log/mysql/mariadb-bin
bind-address = 0.0.0.0 # Autoriser les connexions distantes

Créez l'utilisateur de réplication dans MariaDB :
SQL

CREATE USER 'replique'@'%' IDENTIFIED BY 'votre_mot_de_passe';
GRANT REPLICATION SLAVE ON *.* TO 'replique'@'%';
FLUSH PRIVILEGES;.

###3. Configuration de l'Esclave (Slave)

Éditez le fichier 50-server.cnf sur le second PC :
```ini

[mysqld]
server-id = 2

## Liez l'esclave au maître :
SQL

CHANGE MASTER TO 
MASTER_HOST='IP_DU_MAITRE',
MASTER_USER='replique',
MASTER_PASSWORD='votre_mot_de_passe',
MASTER_LOG_FILE='mariadb-bin.000001', -- À vérifier via SHOW MASTER STATUS
MASTER_LOG_POS=1234;
START SLAVE;.

 Problèmes Résolus

    Pare-feu (UFW) : Nous avons dû configurer le pare-feu pour autoriser le trafic sur le port 3306, ce qui était le blocage principal lors de nos premiers tests de connexion.

    Synchronisation Multi-Esclaves : Expérimentation de la gestion de plusieurs nœuds esclaves pour observer la cohérence des données.

Perspectives d'Évolution

    -> Mise en place d'une réplication Multi-Maîtres.

    -> Sécurisation des flux via SSL/TLS.

    -> Automatisation du déploiement via Docker et Docker-Compose.


C'est ainsi donc que notre travaill a été effectual.