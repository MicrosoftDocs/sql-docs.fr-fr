---
title: Gérer des Clusters Big Data (BDC) avec des notebooks Jupyter et Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Gestion de Clusters Big Data (BDC) avec des notebooks Jupyter et Azure Data Studio sur le cluster Big Data SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3af391b2fe6a2c104a4854afdb57b4dc5eb95029
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489173"
---
# <a name="manage-big-data-clusters-bdc-the-cluster-with-notebooks"></a>Gérer des Clusters Big Data (BDC) avec des notebooks

Cette page est un index des notebooks pour les Clusters Big Data SQL Server. Ces notebooks exécutables (.ipynb) sont conçus pour SQL Server 2019 afin de faciliter la gestion des Clusters Big Data.

Chaque notebook est conçu pour vérifier ses propres dépendances. Une commande **Exécuter toutes les cellules** s’effectue correctement ou produit une exception avec un conseil en lien hypertexte vers un autre notebook qui va résoudre la dépendance manquante. Suivez le lien hypertexte vers le notebook suivant, appuyez sur **Exécuter toutes les cellules** puis, après une exécution réussie, revenez au notebook d’origine, et appuyez à nouveau sur **Exécuter toutes les cellules**.

Une fois toutes les dépendances installées, mais après l’échec de la commande **Exécuter toutes les cellules**, chaque notebook analyse les résultats et, dans la mesure du possible, produit un conseil de lien hypertexte vers un autre notebook pour faciliter la résolution du problème.


## <a name="installing-and-uninstalling-utilities-on-big-data-cluster-bdc"></a>Installation et désinstallation d’utilitaires sur un Cluster Big Data (BDC)

Cette section contient un ensemble de notebooks permettant d’installer et de désinstaller les outils en ligne de commande et packages nécessaires pour gérer des Clusters Big Data SQL Server.

|Nom |Description |
|---|---|
|SOP010 - Mettre à niveau un cluster Big Data|Utilisez ce notebook pour mettre à niveau un cluster Big Data à l’aide de azdata. |
|SOP012 - Installer unixodbc pour Mac|Utiliser ce notebook lorsque vous recevez des erreurs lors de l’utilisation Préparer l’installation ODBC pour SQL Server.|
|SOP036 - Installer l’interface de ligne de commande kubectl|Utilisez ce notebook pour installer l’interface de ligne de commande kubectl, quel que soit votre système d’exploitation.|
|SOP037 - Désinstaller l’interface de ligne de commande kubectl|Utilisez ce notebook pour désinstaller l’interface de ligne de commande kubectl, quel que soit votre système d’exploitation.|
|SOP038 - Installer l’interface de ligne de commande Azure|Utilisez ce notebook pour installer l’interface de ligne de commande Azure CLI, quel que soit votre système d’exploitation.|
|SOP039 - Désinstaller l’interface de ligne de commande Azure|Utilisez ce notebook pour désinstaller l’interface de ligne de commande Azure CLI, quel que soit votre système d’exploitation.|
|SOP040 - Mettre à niveau pip dans le bac à sable Python ADS|Utilisez ce notebook pour mettre à niveau pip dans le bac à sable Python ADS.|
|SOP054 - Installer l’interface de ligne de commande azdata|Utilisez ce notebook pour installer l’interface de ligne de commande azdata, quel que soit votre système d’exploitation.|
|SOP055 - Désinstaller l’interface de ligne de commande azdata|Utilisez ce notebook pour désinstaller l’interface de ligne de commande azdata, quel que soit votre système d’exploitation.|
|SOP059 - Installer le module Python Kubernetes|Utilisez ce notebook pour installer des modules Kubernetes avec Python.|
|SOP060 - Désinstaller le module Kubernetes|Utilisez ce notebook pour désinstaller des modules Kubernetes avec Python.|
|SOP061 - Supprimer un cluster Big Data|Utilisez ce notebook pour supprimer un cluster BDC.|
|SOP062 - Installer les modules ipython-sql et pyodbc|Utilisez ce notebook pour installer des modules ipython-sql et pyodbc.|
|SOP063 - Installer l’interface de ligne de commande azdata (à l’aide du gestionnaire de package)|Utilisez ce notebook pour installer l’interface de ligne de commande azdata (à l’aide du gestionnaire de package).|
|SOP064 - Désinstaller l’interface de ligne de commande azdata (à l’aide du gestionnaire de package)|Utilisez ce notebook pour désinstaller l’interface de ligne de commande azdata (à l’aide du gestionnaire de package).|
|SOP069 - Installer ODBC for SQL Server|Utilisez ce notebook pour installer le pilote ODBC, car certaines sous-commandes dans azdata requièrent le pilote ODBC SQL Server.|

## <a name="encryption-at-rest-utilities-on-big-data-cluster-bdc"></a>Utilitaires de chiffrement au repos sur un cluster Big Data

Cette section contient un ensemble de notebooks utiles pour la gestion des fonctionnalités de chiffrement au repos sur un cluster Big Data.

|Nom |Description |
|---|---|
|SOP0124 - Lister les clés de chiffrement au repos|Utilisez ce notebook pour lister toutes les clés HDFS.|
|SOP0128 - Activer des zones de chiffrement HDFS dans des clusters Big Data|Utilisez ce notebook pour activer les zones de chiffrement HDFS lors de la mise à niveau vers CU8 à partir de CU6 ou une version antérieure. Facultatif sur les nouveaux déploiements de CU8+ ou lors de la mise à niveau vers CU9.|
|SOP0125 - Supprimer la clé de chiffrement au repos|Utilisez ce notebook pour supprimer des clés de zone de chiffrement HDFS. __Attention !__|
|SOP0126 - Sauvegarder les clés de chiffrement au repos|Utilisez ce notebook pour sauvegarder des clés de zone de chiffrement HDFS.|
|SOP0127 - Restaurer des clés de chiffrement au repos|Utilisez ce notebook pour restaurer des clés de zone de chiffrement HDFS.|

## <a name="managing-certificates-on-big-data-clusters-bdc"></a>Gestion des certificats sur des Clusters Big Data (BDC)

Ensemble de notebooks permettant d’exécuter un notebook pour la gestion des certificats sur des Clusters Big Data.

|Nom |Description |
|---|---|
|CER001 - Générer un certificat d’autorité de certification racine|Générez un certificat d’autorité de certification racine. Envisagez d’utiliser un certificat d’autorité de certification racine pour tous les clusters de non-production dans chaque environnement, car cette technique réduit le nombre de certificats d’autorité de certification racines qui doivent être téléchargés vers les clients qui se connectent à ces clusters. |
|CER002 - Télécharger le certificat d’autorité de certification racine existant|Utilisez ce notebook pour télécharger un certificat d’autorité de certification racine généré à partir d’un cluster.|
|CER003 - Charger le certificat d’autorité de certification racine existant|CER003 - Charger le certificat d’autorité de certification racine existant.|
|CER004 - Télécharger et charger le certificat d’autorité de certification racine existant|Téléchargez et chargez le certificat d’autorité de certification racine existant. |
|CER010 - Installer l’autorité de certification racine générée localement|Ce notebook copie localement (à partir d’un cluster Big Data) le certificat d’autorité de certification racine généré qui a été installé à l’aide de **CER001 - Générer un certificat d’autorité de certification racine** ou **CER003 - Charger le certificat d’autorité de certification racine existant**, puis installe le certificat d’autorité de certification racine dans le magasin de certificats local de cet ordinateur.|
|CER020 - Créer un certificat de proxy de gestion|Ce notebook crée un certificat pour le point de terminaison du proxy de gestion.|
|CER021 - Créer un certificat Knox|Ce notebook crée un certificat pour le point de terminaison de la passerelle Knox.|
|CER022 - Créer un certificat de proxy d’application|Ce notebook crée un certificat pour le point de terminaison du proxy de déploiement des applications.|
|CER023 - Créer un certificat principal|Ce notebook crée un certificat pour le point de terminaison principal.|
|CER030 - Signer le certificat du proxy de gestion avec l’autorité de certification générée|Ce notebook signe le certificat créé à l’aide de **CER020 - Créer un certificat de proxy de gestion** avec le certificat de l’autorité de certification racine généré, créé à l’aide de **CER001 - Générer un certificat d’autorité de certification racine** ou **CER003 - Charger le certificat d’autorité de certification racine existant**|
|CER031 - Signer le certificat Knox avec l’autorité de certification générée|Ce notebook signe le certificat créé à l’aide de **CER021 - Créer un certificat Knox** avec le certificat de l’autorité de certification racine généré, créé à l’aide de **CER001 - Générer un certificat d’autorité de certification racine** ou **CER003 - Charger le certificat d’autorité de certification racine existant**|
|CER032 - Signer le certificat du proxy d’application avec l’autorité de certification générée|Ce notebook signe le certificat créé à l’aide de **CER022 - Créer un certificat de proxy d’application** avec le certificat de l’autorité de certification racine généré, créé à l’aide de **CER001 - Générer un certificat d’autorité de certification racine** ou **CER003 - Charger le certificat d’autorité de certification racine existant**.|
|CER033 - Signer le certificat principal avec l’autorité de certification générée|Ce notebook signe le certificat créé à l’aide de **CER023 - Créer un certificat principal** avec le certificat de l’autorité de certification racine généré, créé à l’aide de **CER001 - Générer un certificat d’autorité de certification racine** ou **CER003 - Charger le certificat d’autorité de certification racine existant**.|
|CER040 - Installer le certificat du proxy de gestion signé|Ce notebook installe dans le Cluster Big Data le certificat signé à l’aide de **CER030 - Signer le certificat du proxy de gestion avec l’autorité de certification générée**.|
|CER041 - Installer le certificat Knox signé|Ce notebook installe dans le Cluster Big Data le certificat signé à l’aide de **CER031 - Signer le certificat Knox avec l’autorité de certification générée**.|
|CER042 - Installer le certificat du proxy d’application signé|Ce notebook installe dans le Cluster Big Data le certificat signé à l’aide de **CER032 - Signer le certificat du proxy d’application avec l’autorité de certification générée**.|
|CER043 - Installer le certificat de contrôleur signé|Ce notebook est installé dans le Cluster Big Data, le certificat signé à l’aide de **CER034 - Signer le certificat de contrôleur avec l’autorité de certification racine du cluster** et note qu’à la fin de ce notebook, le pod de contrôleur et tous les pods qui utilisent Polybase (pool maître et pods de pool de calcul) sont redémarrés pour charger les nouveaux certificats.|
|CER050 - Attendre que le BDC soit sain|Ce notebook attend que le Cluster Big Data retrouve un état sain, après le redémarrage du pod de contrôleur et des pods qui utilisent Polybase pour charger les nouveaux certificats.|
|CER100 - Configurer un cluster avec des certificats auto-signés|Ce notebook génère une nouvelle autorité de certification racine dans le Cluster Big Data et crée de nouveaux certificats pour chaque point de terminaison (ces points de terminaison sont les suivants : Gestion, Passerelle, Proxy d’application et Contrôleur). Signez chaque nouveau certificat avec la nouvelle autorité de certification racine générée, sauf le certificat du contrôleur (qui est signé avec l’autorité de certification racine du cluster existant), puis installez chaque certificat dans le Cluster Big Data. Téléchargez la nouvelle autorité de certification racine générée dans le magasin de certificats des autorités de certification racines de confiance de cet ordinateur. Tous les certificats auto-signés générés seront stockés dans le pod de contrôleur à l’emplacement test_cert_store_root.|
|CER101 - Configurer un cluster avec des certificats auto-signés à l’aide de l’autorité de certification racine existante|Ce notebook utilise une autorité de certification racine générée existante dans le Cluster Big Data (chargé avec **CER003**) et crée de nouveaux certificats pour chaque point de terminaison (Gestion, Passerelle, Proxy d’application et Contrôleur), puis signe chaque nouveau certificat avec la nouvelle autorité de certification racine générée, sauf le certificat du contrôleur (qui est signé avec l’autorité de certification racine du cluster existant) et installe chaque certificat dans le Cluster Big Data. Tous les certificats auto-signés générés seront stockés dans le pod de contrôleur (à l’emplacement test_cert_store_root). À la fin de ce notebook, tous les accès https:// au Cluster Big Data à partir de cet ordinateur (et de tout ordinateur qui installe la nouvelle autorité de certification racine) s’affichent comme sécurisés. Le chapitre Excuteur de notebook permet de s’assurer que les CronJobs créés (OPR003) pour exécuter le déploiement d’applications installera l’autorité de certification racine du cluster pour permettre d’obtenir en toute sécurité des jetons JWT et le Swagger.json.|

## <a name="backup-and-restore-from-big-data-cluster-bdc"></a>Sauvegarde et restauration à partir d’un Cluster Big Data (BDC)

Cette section contient un ensemble de notebooks utiles pour les opérations de sauvegarde et de restauration pour les Clusters Big Data SQL Server.

| Nom | Description |
|--|--|
| SOP008 - Sauvegarder des fichiers HDFS vers Azure Data Lake Store Gen2 avec distcp | Cette procédure de fonctionnement standard sauvegarde les données du système de fichiers HDFS source du cluster BDC SQL Server 2019 vers le compte Azure Data Lake Store Gen2 que vous spécifiez. Vérifiez que le compte Azure Data Lake Store Gen2 est configuré avec l’option « espace de noms hiérarchique » activée. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md).
