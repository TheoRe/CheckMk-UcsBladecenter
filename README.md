# CheckMk-UcsBladecenter
Agent spécial optimisé pour Cisco UCS palliant les limitations de l'agent natif. Il assure une surveillance hybride des lames B-Series et serveurs racks C-Series avec une segmentation précise par châssis, serveur et disque pour une isolation immédiate des états critiques.

Voici un guide complet pour installer et paramétrer votre nouveau plugin. Ce README est conçu pour accompagner votre fichier .mkp.

Cisco UCS Monitoring (V2 - Axians)
Présentation
Cet agent spécial est une solution optimisée pour la surveillance des infrastructures Cisco UCS (Manager). Il a été conçu pour pallier les limitations de l'agent natif Checkmk en offrant une surveillance hybride complète des lames (B-Series) et des serveurs racks (C-Series).

L'agent se connecte à l'API du UCS Manager pour récupérer les informations de santé de l'ensemble des composants physiques.

Fonctionnalités principales
Isolation des pannes : Chaque châssis, serveur et disque (Slot) est traité comme un service distinct pour une identification immédiate.

Santé Matérielle : Monitoring précis des ventilateurs (Fans), alimentations (PSUs) et adaptateurs réseau.

Segmentation des erreurs : Classification automatique des Fault Instances par catégories (Licences, Bootflash, Fabric Interconnect) et par affinité matérielle.

1. Installation du paquet (MKP)
Avant de créer la règle, vous devez installer le paquet sur votre site Checkmk :

Allez dans Setup > Maintenance > Extension packages.

Cliquez sur Upload package et sélectionnez votre fichier ucs_blade_v2-2.6.0.mkp.

Une fois listé, cliquez sur l'icône Install (la petite boîte).

Activez les changements dans Checkmk.

2. Paramétrage de la règle
Une fois le paquet installé, vous n'avez qu'une seule règle à configurer pour activer la remontée des données.

Allez dans Setup > Agents > Other integrations.

Recherchez et cliquez sur Cisco UCS Blade Monitoring (V2 - Axians).

Cliquez sur Add rule.

Remplissez les paramètres requis :

UCS Manager IP / Hostname : L'adresse de gestion de votre équipement UCS.

Username : Un compte utilisateur UCS (un accès en lecture seule est suffisant).

Password : Le mot de passe associé.

Connection Timeout : Par défaut 15 ou 20 secondes (recommandé pour les gros environnements).

Dans la section Conditions, sélectionnez l'hôte correspondant à votre UCS Manager dans le champ Explicit hosts.

Sauvegardez et activez les changements.

3. Optimisation recommandée
Par défaut, Checkmk exécute les agents toutes les minutes. Pour ne pas surcharger l'API du UCS Manager inutilement, il est vivement conseillé de passer l'intervalle à 30 minutes.

Allez dans Setup > Service monitoring rules > Normal check interval for service checks.

Créez une règle ciblant votre hôte UCS Manager.

Réglez l'intervalle sur 30 minutes.

4. Résultat de la surveillance
Après avoir lancé une découverte de services sur votre hôte, les services suivants apparaîtront automatiquement :

UCS_Blade_X/Y : État de la lame, Service Profile associé et erreurs spécifiques au slot.

UCS_Rack_Server_X : État complet des serveurs rack C-Series.

UCS_Chassis_X : Santé globale du châssis.

UCS_Faults_... : Services dédiés aux licences, au bootflash et aux Fabric Interconnects.

Composants individuels : Chaque Fan, PSU et Disque physique possède son propre service de santé.
