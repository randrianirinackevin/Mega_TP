# 🚀 Mega TP : Infrastructure Haute Disponibilité & DevOps

![Ansible](https://img.shields.io/badge/Ansible-Automated-red?style=flat-square&logo=ansible)
![Vagrant](https://img.shields.io/badge/Vagrant-Infrastructure-blue?style=flat-square&logo=vagrant)
![Zabbix](https://img.shields.io/badge/Monitoring-Zabbix-DC143C?style=flat-square&logo=zabbix)
![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20Windows-lightgrey?style=flat-square)

> **Projet : Administration de systèmes répartis.**
> Objectif : Déploiement automatisé d'une infrastructure résiliente (HA), sécurisée et supervisée.

---

## 📑 Sommaire
- [Architecture Technique](#-architecture-technique)
- [Accès Rapide (Dashboard)](#-accès-rapide-dashboard)
- [Guide de Démarrage](#-guide-de-démarrage)
- [Détails des Missions](#-détails-des-missions)
- [Structure du Projet](#-structure-du-projet)

---

## 🏗 Architecture Technique

L'infrastructure simule un environnement d'entreprise complet sur le réseau privé **192.168.50.0/24**.

| Machine | OS | IP | Rôle Principal |
| :--- | :--- | :--- | :--- |
| **Admin** | Ubuntu Server | \192.168.50.10\ | 🕹️ Contrôleur Ansible & Serveur Zabbix |
| **Node01** | RedHat EL | \192.168.50.20\ | 🔄 Nœud Cluster 1 (Web + Data) |
| **Node02** | RedHat EL | \192.168.50.21\ | 🔄 Nœud Cluster 2 (Web + Data) |
| **WinSrv** | Windows Server | \192.168.50.30\ | 🔐 Contrôleur de Domaine (AD) |
| **VIP** | *Virtuelle* | **\192.168.50.100\** | 🌐 **Point d'entrée Haute Dispo** |

---

## ⚡ Accès Rapide (Dashboard)

Une fois le déploiement terminé avec \agrant up\, voici les liens d'accès :

| Service | URL / Commande | Identifiants |
| :--- | :--- | :--- |
| **Site Web (HA)** | http://192.168.50.100 | *Accès Public* |
| **Supervision Zabbix** | http://192.168.50.10/zabbix | User: \Admin\ / Pass: \zabbix\ |
| **Partage Samba** | \\\\192.168.50.100\\partage | *Accès invité ou domaine* |

---

## 🚀 Guide de Démarrage

### 1. Prérequis
* **Hyperviseur :** VMware Workstation ou VirtualBox.
* **Outil :** Vagrant installé sur la machine hôte.

### 2. Installation Automatisée
Ouvrez votre terminal dans ce dossier et lancez :

\\\ash
vagrant up
\\\

> **Note :** Cette commande unique télécharge les images, crée les 4 VMs et lance automatiquement les playbooks Ansible pour configurer le Cluster, la Sécurité et Zabbix.

---

## 🛠 Détails des Missions Réalisées

### 🔄 Mission 1 : Haute Disponibilité (Cluster HA)
[cite_start]Mise en place d'un cluster **Pacemaker/Corosync** actif/passif[cite: 10, 38].
- [x] [cite_start]**IP Flottante (VIP) :** L'adresse \.100\ bascule automatiquement entre les nœuds[cite: 40].
- [x] [cite_start]**Services :** Nginx (Web) et Samba (Fichiers) sont monitorés par le cluster[cite: 38, 39].
- [x] [cite_start]**Résilience :** Si Node01 tombe, Node02 prend le relais sans interruption[cite: 42].

### 🛡️ Mission 2 & 3 : Sécurisation (Hardening)
[cite_start]Application des règles de sécurité via Ansible[cite: 43].
* [cite_start]**Linux :** Pare-feu strict (Ports SSH, HTTP, 10050 uniquement), Root SSH désactivé[cite: 46, 47].
* [cite_start]**Windows (AD) :** Contrôleur de Domaine, mise en place de **LAPS** et durcissement des mots de passe[cite: 51, 55, 58].

### 👁️ Mission 4 : Supervision (Zabbix)
- [cite_start]**Monitoring :** CPU, RAM et disponibilité des services Web[cite: 67, 68].
- [cite_start]**Alerting :** Notification immédiate sur le Dashboard si un nœud ne répond plus[cite: 69].
- [cite_start]**Automatisation :** Les agents sont déployés et configurés automatiquement par Ansible[cite: 64].

---

## 📂 Structure du Dépôt

\\\	ext
📦 TP-Infrastructure-HA
 [cite_start]┣ 📜 Vagrantfile          # Définition des 4 VMs et du réseau [cite: 12]
 ┣ 📂 ansible
 ┃ ┣ 📜 inventory.ini      # Inventaire des hôtes
 ┃ ┣ 📜 site.yml           # Playbook principal
 ┃ ┗ 📂 roles              # Rôles (ha-cluster, security, windows, zabbix)
 ┗ 📜 README.md            # Ce fichier de documentation
\\\

---
**Date de rendu :** 30 Décembre
**Statut :** ✅ Fonctionnel et Testé
