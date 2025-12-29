# Mega TP : Infrastructure Haute Disponibilité et DevOps

![Ansible](https://img.shields.io/badge/Ansible-Automated-red?style=flat-square&logo=ansible)
![Vagrant](https://img.shields.io/badge/Vagrant-Infrastructure-blue?style=flat-square&logo=vagrant)
![Zabbix](https://img.shields.io/badge/Monitoring-Zabbix-DC143C?style=flat-square&logo=zabbix)
![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20Windows-lightgrey?style=flat-square)

> **Projet : Administration de systèmes répartis.**
> Objectif : Déploiement automatisé d'une infrastructure résiliente (HA), sécurisée et supervisée.

---

## Sommaire
- [Architecture Technique](#architecture-technique)
- [Accès aux Services](#accès-aux-services)
- [Guide de Démarrage](#guide-de-démarrage)
- [Détails des Missions](#détails-des-missions)
- [Structure du Projet](#structure-du-projet)

---

## Architecture Technique

L'infrastructure simule un environnement d'entreprise complet sur le réseau privé **192.168.50.0/24**.

| Machine | OS | IP | Rôle Principal |
| :--- | :--- | :--- | :--- |
| **Admin** | Ubuntu Server | \192.168.50.10\ | Contrôleur Ansible et Serveur Zabbix |
| **Node01** | RedHat EL | \192.168.50.20\ | Nœud Cluster 1 (Web et Data) |
| **Node02** | RedHat EL | \192.168.50.21\ | Nœud Cluster 2 (Web et Data) |
| **WinSrv** | Windows Server | \192.168.50.30\ | Contrôleur de Domaine (AD) |
| **VIP** | *Virtuelle* | **\192.168.50.100\** | Point d'entrée Haute Disponibilité |

---

## Accès aux Services

Une fois le déploiement terminé avec la commande \agrant up\, voici les points d'accès :

| Service | URL / Chemin | Identifiants |
| :--- | :--- | :--- |
| **Site Web (HA)** | http://192.168.50.100 | Accès Public |
| **Supervision Zabbix** | http://192.168.50.10/zabbix | User: \Admin\ / Pass: \zabbix\ |
| **Partage Samba** | \\\\192.168.50.100\\partage | Accès invité ou domaine |

---

## Guide de Démarrage

### 1. Prérequis
* **Hyperviseur :** VMware Workstation ou VirtualBox.
* **Outil :** Vagrant installé sur la machine hôte.

### 2. Installation Automatisée
Ouvrez votre terminal dans ce dossier et lancez la commande suivante :

\\\Bash
vagrant up
\\\

Note : Cette commande télécharge les images, crée les quatre machines virtuelles et lance automatiquement les playbooks Ansible pour configurer le cluster, la sécurité et la supervision.

---

## Détails des Missions Réalisées

### Mission 1 : Haute Disponibilité (Cluster HA)
Mise en place d'un cluster **Pacemaker/Corosync** en mode actif/passif.
- **IP Flottante (VIP) :** L'adresse \.100\ bascule automatiquement entre les nœuds en cas de défaillance.
- **Services :** Nginx (Web) et Samba (Fichiers) sont intégrés et monitorés par le cluster.
- **Résilience :** En cas d'arrêt du Node01, le Node02 prend le relais sans interruption de service.

### Mission 2 et 3 : Sécurisation (Hardening)
Application des politiques de sécurité via Ansible.
* **Linux :** Pare-feu restrictif (Firewalld), désactivation de l'accès SSH en root.
* **Windows (AD) :** Promotion en Contrôleur de Domaine, déploiement de **LAPS** et durcissement de la politique de complexité des mots de passe.

### Mission 4 : Supervision (Zabbix)
- **Monitoring :** Suivi des ressources (CPU, RAM) et de la disponibilité des services critiques.
- **Alerting :** Notifications visuelles sur le tableau de bord en cas d'indisponibilité d'un nœud ou d'un service.
- **Automatisation :** Installation et configuration automatique des agents Zabbix par Ansible.

---

## Structure du Dépôt

\\\	ext
TP-Infrastructure-HA
 ├── Vagrantfile          # Configuration des VM et du réseau
 ├── ansible
 │   ├── inventory.ini    # Inventaire des hôtes
 │   ├── site.yml         # Playbook principal d'orchestration
 │   └── roles            # Rôles (ha-cluster, security, windows, zabbix)
 └── README.md            # Documentation technique
\\\

---

