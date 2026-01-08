# Atelier – Sécurité des Endpoints & Supervision SIEM (Wazuh)

## Présentation
Ce projet met en œuvre une plateforme complète de **supervision de la sécurité** basée sur **Wazuh**, combinant les approches **SIEM (Security Information and Event Management)** et **EDR (Endpoint Detection and Response)** dans un environnement **Cloud AWS**.

L’objectif est de démontrer, à travers des scénarios concrets, la détection et l’analyse d’événements de sécurité sur des systèmes **Linux** et **Windows**, comme dans un SOC réel.

---

## Objectifs
- Déployer une architecture SIEM + EDR fonctionnelle
- Superviser des endpoints Linux et Windows
- Centraliser et corréler les logs de sécurité
- Détecter des incidents courants (authentification, privilèges, intégrité)
- Introduire le threat hunting

---

## Architecture

### Composants
- **Wazuh Server (Ubuntu 22.04)**
  - Manager
  - Indexer
  - Dashboard
- **Client Linux**
  - Ubuntu 22.04
  - Wazuh Agent
- **Client Windows**
  - Windows Server
  - Wazuh Agent
  - *(Optionnel)* Sysmon

### Flux & ports
| Port | Description |
|-----|-------------|
| 22/TCP | SSH (Linux) |
| 3389/TCP | RDP (Windows) |
| 443/TCP | Wazuh Dashboard |
| 1514/TCP | Communication agents |
| 1515/TCP | Enrôlement agents |

---

## Prérequis

- Compte **AWS Learner Lab**
- 3 instances EC2
- Connaissances de base :
  - Linux / Windows
  - Réseau
  - Cybersécurité

---

## Déploiement

### Installation du serveur Wazuh
```bash
sudo apt update && sudo apt -y upgrade
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
sudo bash wazuh-install.sh -a
````

### Vérification des services

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```

Accès au dashboard :

```
https://<IP_WAZUH_SERVER>
```

---

## Enrôlement des agents

### Linux

* Déploiement depuis le **Wazuh Dashboard**
* Vérification :

```bash
sudo systemctl status wazuh-agent
```

### Windows

* Installation via PowerShell (Admin)
* Vérifier que le service **Wazuh Agent** est actif

---

## Scénarios de sécurité

### Linux (SIEM)

* Tentatives SSH échouées
* Élévation de privilèges (`sudo`)
* Modification de fichiers sensibles (FIM)

### Windows (EDR)

* Échecs de connexion (Event ID 4625)
* Création d’utilisateur local
* Ajout à un groupe Administrateurs
* *(Optionnel)* Analyse Sysmon

---

## Analyse & supervision

Depuis le **Wazuh Dashboard** :

* Security Events
* Threat Hunting
* Filtres par agent, type d’événement et MITRE ATT&CK

---

## Améliorations possibles

* Règles personnalisées Wazuh
* Alerting (email, Slack)
* Déploiement multi-agents
* Attaques avancées
* Infrastructure as Code (Terraform)

---

## ✅ Conclusion

Ce projet montre qu’une sécurité efficace repose sur la **visibilité**, la **corrélation** et l’**analyse continue**.
Sans SIEM et EDR, une infrastructure est aveugle.

---

## 📚 Références

* [https://documentation.wazuh.com](https://documentation.wazuh.com)
* [https://aws.amazon.com](https://aws.amazon.com)
