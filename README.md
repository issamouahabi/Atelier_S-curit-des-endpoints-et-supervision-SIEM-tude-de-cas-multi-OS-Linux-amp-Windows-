# Atelier_S-curit-des-endpoints-et-supervision-SIEM-tude-de-cas-multi-OS-Linux-amp-Windows-


1- Introduction

L’objectif de ce projet est de mettre en place une plateforme de supervision et de détection de sécurité basée sur une approche SIEM et EDR, déployée dans un environnement Cloud. Cette plateforme vise à centraliser, analyser et corréler des événements de sécurité provenant de plusieurs systèmes afin d’identifier des comportements suspects et des incidents potentiels.
Le lab s’appuie sur la solution Wazuh, utilisée comme composant central d’un SOC moderne, permettant la collecte et l’analyse des journaux de sécurité, ainsi que la surveillance des endpoints. Deux types de systèmes sont supervisés : un client Linux et un client Windows, représentant des environnements couramment rencontrés en entreprise.
À travers des scénarios réalistes (tentatives d’authentification échouées, élévation de privilèges, création d’utilisateurs, événements système), ce projet démontre la capacité de la plateforme à détecter des événements de sécurité réels, à les centraliser dans un tableau de bord unique et à fournir une visibilité claire sur l’état de sécurité des systèmes.
Ce travail permet ainsi d’illustrer le fonctionnement opérationnel d’un SOC, en montrant comment les solutions SIEM et EDR peuvent être utilisées conjointement pour améliorer la détection, l’analyse et la compréhension des menaces dans un environnement Cloud.












2- Architecture du lab

L’architecture du projet est déployée dans un environnement AWS Cloud, au sein d’un VPC dédié (10.0.0.0/16) comprenant un subnet public (10.0.1.0/24). Cette architecture vise à simuler un environnement d’entreprise supervisé par un SOC centralisé.
Serveur Wazuh (EC2-1)
Le cœur de l’architecture repose sur une instance EC2 Ubuntu jouant le rôle de serveur Wazuh All-in-One. Cette instance regroupe :
le Wazuh Manager (analyse et corrélation des événements),


l’Indexer (stockage et recherche),


le Dashboard (visualisation SIEM).


Le serveur Wazuh est accessible par l’analyste sécurité via le port HTTPS 443, permettant l’accès au tableau de bord de supervision.
Clients supervisés
Deux endpoints sont intégrés à la plateforme :
Client Linux (EC2-2)
 Une instance Ubuntu équipée de l’agent Wazuh, représentant un serveur Linux d’entreprise.
 L’administration se fait via SSH (port 22).


Client Windows (EC2-3)
 Une instance Windows Server équipée de l’agent Wazuh, représentant un poste ou serveur Windows en environnement professionnel.
 L’accès administrateur se fait via RDP (port 3389).


Communication et flux réseau
Les communications entre les agents et le serveur Wazuh reposent sur des ports dédiés :
1515/TCP : utilisé pour l’enrôlement des agents (Linux et Windows) vers le serveur Wazuh.


1514/TCP : utilisé pour la transmission des logs et événements de sécurité des agents vers le serveur.


443/TCP : accès au dashboard Wazuh pour l’analyste sécurité.


Ces flux permettent la centralisation des journaux, la corrélation des événements et la détection d’activités suspectes en temps réel.
Rôles des utilisateurs
Administrateur : accède aux instances via SSH et RDP pour la configuration et la génération d’événements.


Analyste sécurité : accède uniquement au dashboard Wazuh pour analyser les alertes et mener des activités de threat hunting.




3- Mise en place technique
Installation de Wazuh sur le serveur


Création des agents Linux et Windows


Visualisation des agents Linux et Windows


















4- Démonstrations de détection 
4.1 Détection SIEM côté Linux












4.2 Détection SIEM / EDR côté Windows




5- SIEM vs EDR 
Ce tableau montre les différences entre SIEM et EDR








6- IAM / PAM
La gestion des identités et des accès (IAM – Identity and Access Management) constitue un élément central de la sécurité des systèmes d’information. Elle permet de contrôler qui peut accéder aux ressources, avec quels droits et dans quelles conditions. Dans ce projet, plusieurs événements détectés par Wazuh illustrent directement les enjeux liés à l’IAM et au PAM (Privileged Access Management).
Création d’un utilisateur local
La création d’un nouvel utilisateur sur un système est considérée comme un événement critique, car elle peut indiquer :
l’arrivée d’un nouvel utilisateur légitime,


ou une action malveillante visant à établir une persistance sur le système.


Un attaquant qui crée un compte local peut réutiliser cet accès ultérieurement sans exploiter à nouveau une vulnérabilité. La détection de cet événement permet donc d’identifier rapidement une tentative de compromission ou une mauvaise gestion des accès.
Ajout au groupe Administrators
L’ajout d’un utilisateur au groupe Administrators représente une élévation de privilèges. Cet événement est particulièrement sensible, car un compte administrateur dispose de droits étendus :
modification de la configuration système,


installation de logiciels,


désactivation des mécanismes de sécurité,


accès à des données sensibles.


Dans un contexte de sécurité, ce type de changement peut révéler une escalade de privilèges non autorisée, souvent associée à une attaque post-compromission.
Lien avec IAM et PAM
Ces événements illustrent concrètement les principes de l’IAM et du PAM :
IAM : contrôle de l’identité des utilisateurs et de leurs droits d’accès.


PAM : surveillance et limitation des comptes à privilèges élevés afin de réduire les risques liés à leur abus.


Grâce à Wazuh, ces actions sont détectées, centralisées et analysées, permettant au SOC d’identifier rapidement des comportements à risque et de réagir avant qu’un incident majeur ne se produise.



















7- Threat Hunting
Filtrer seulement Windows client

Filtrer seulement Linux client




Dans Windows, chercher pour les alertes “Windows Security” seulement

Dans Linux, chercher pour les alertes “SSHD” seulement








Conclusion
Ce lab a permis de démontrer la mise en œuvre concrète d’une plateforme de supervision et de détection de sécurité basée sur une approche SIEM et EDR, déployée dans un environnement Cloud. À travers la supervision de systèmes Linux et Windows, la solution Wazuh a montré sa capacité à centraliser les journaux, détecter des événements de sécurité réels et fournir une visibilité unifiée sur l’état de sécurité des endpoints.
L’association du SIEM (corrélation et analyse centralisée des événements) et de l’EDR (surveillance détaillée des activités des endpoints) constitue un atout majeur dans le Cloud, où les environnements sont dynamiques et distribués. Cette complémentarité permet une détection plus efficace des attaques, une meilleure compréhension des incidents et une réaction plus rapide face aux menaces.
Cependant, ce projet reste un lab pédagogique. Les scénarios de détection sont limités et ne couvrent pas l’ensemble des attaques possibles en environnement réel. De plus, l’architecture mise en place ne prend pas en compte certains aspects avancés tels que la haute disponibilité, la scalabilité ou l’automatisation complète des réponses. Malgré ces limites, ce travail offre une base solide pour comprendre le fonctionnement opérationnel d’un SOC moderne et l’intérêt des solutions SIEM et EDR dans un contexte Cloud.

