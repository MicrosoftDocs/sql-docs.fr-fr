---
title: Nouveautés de SSMA pour MySQL (MySQLToSql) | Microsoft Docs
description: Découvrez les modifications apportées à Assistant Migration SQL Server (SSMA) pour MySQL (MySQLToSQL) pour chaque version.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 1451a0b0-6713-4d0c-954f-ea3d8fce1d31
ms.author: alexiva
ms.openlocfilehash: 138cb90558bfb73d0b9e0cd3ba75a7a41fc84229
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186455"
---
# <a name="whats-new-in-ssma-for-mysql-mysqltosql"></a>Nouveautés de SSMA pour MySQL (MySQLToSql)

Cet article répertorie les Assistant Migration SQL Server (SSMA) pour les modifications MySQL dans chaque version.

## <a name="ssma-v817"></a>SSMA v 8.17

La version v 8.17 de SSMA pour MySQL contient les modifications suivantes :

* Mettre à jour les rapports d’évaluation HTML pour utiliser l’éditeur moderne pour afficher du texte SQL

## <a name="ssma-v816"></a>SSMA v 8.16

La version v 8.16 de SSMA pour MySQL contient les modifications suivantes :

* Ajouter la prise en charge des colonnes calculées
* Résoudre les problèmes lors `INSERT` de la conversion d’une instruction pour les tables avec des contraintes et des clés primaires uniques
* Mettre à jour l’analyseur pour respecter `ANSI_QUOTES` et les `NO_BACKSLASH_ESCAPES` modes serveur
* Supprimer la prise en charge de l’analyseur hérité
* Résoudre le problème lié à l’actualisation des objets à partir de la base de données

## <a name="ssma-v815"></a>SSMA v 8.15

En plus de plusieurs améliorations de l’accessibilité, la version v 8.15 de SSMA pour MySQL contient les modifications suivantes :

* Remodelez les rapports d’évaluation pour travailler dans des navigateurs modernes
* Utiliser l’autorité fournie par la base de données pour l’authentification Azure AD
* Améliorer la dénomination des instructions chargées à partir de fichiers

## <a name="ssma-v814"></a>SSMA v 8.14

En plus de plusieurs améliorations visant à garantir une plus grande accessibilité pour les personnes handicapées, la version v 8.14 de SSMA pour MySQL requiert une mise à niveau de projet, car elle stocke désormais la version complète du serveur source/cible dans les métadonnées du projet.

## <a name="ssma-v813"></a>SSMA v 8.13

La version v 8.13 de SSMA pour MySQL contient les modifications suivantes :

* Considérer les casts de type implicite lors de la conversion des appels de procédure et de fonction
* Améliorer la journalisation de la chaîne de connexion source pour aider à résoudre les problèmes de connexion

## <a name="ssma-v812"></a>SSMA v 8.12

La version v 8.12 de SSMA pour MySQL contient les modifications suivantes :

* Conversion de tables temporaires DDL

## <a name="ssma-v811"></a>SSMA v 8.11

La version 8.11 de SSMA pour MySQL contient les modifications suivantes :

* Utiliser la bibliothèque MSAL.NET pour l’authentification Azure Active Directory interactive

## <a name="ssma-v810"></a>SSMA v 8.10

La version v 8.10 de SSMA pour MySQL contient des améliorations mineures en matière de performances et des correctifs de bogues.

## <a name="ssma-v89"></a>SSMA v 8.9

La version v 8.9 de SSMA pour MySQL contient les modifications suivantes :

* Correctif pour la migration de données de types spatiaux
* Correction du problème avec les caractères spéciaux dans le nom du projet

## <a name="ssma-v88"></a>SSMA v 8.8

La version v 8.8 de SSMA pour MySQL comprend les éléments suivants :

* Améliorations de la stabilité de la synchronisation des objets SQL Server
* Améliorations des performances de l’interface graphique lors de l’évaluation et de la conversion

## <a name="ssma-v87"></a>SSMA v 8.7

La version v 8.7 de SSMA pour MySQL présente des correctifs mineurs et des améliorations des performances dans l’interface utilisateur graphique.

En outre, SSMA pour MySQL fournit désormais la conversion pour la `LIMIT` clause lors du ciblage d’Azure SQL.

> [!IMPORTANT]
> Avec SSMA v 8.5 et versions ultérieures, .NET 4.7.2 est une condition préalable à l’installation. Si vous avez besoin d’installer cette version, vous pouvez télécharger le fichier d’exécution [ici](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v86"></a>SSMA v 8.6

En plus d’un ensemble ciblé de correctifs conçus pour améliorer la facilité d’utilisation et les performances, la version 8.6 de SSMA pour MySQL a été améliorée en ajoutant un paramètre qui permet aux utilisateurs d’omettre les propriétés étendues SSMA dans le code converti.

Pour tirer parti de ce paramètre, dans SSMA pour MySQL, accédez à **Outils**  >  **paramètres du projet**  >    >  **conversion** générale, puis sous **divers**, mettez à jour la valeur du paramètre **omettre les propriétés étendues** sur **Oui**.

![Paramètre d’omission des propriétés étendues](../mysql/media/ssma-omit-extended-properties.png)

> [!IMPORTANT]
> Avec SSMA v 8.5 et versions ultérieures, .NET 4.7.2 est une condition préalable à l’installation. Si vous avez besoin d’installer cette version, vous pouvez télécharger le fichier d’exécution [ici](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v85"></a>SSMA v 8.5

La version v 8.5 de SSMA pour MySQL est améliorée grâce à la prise en charge de l’authentification Azure Active Directory et de la prise en charge de base des fonctionnalités JSON dans SQL Server, ainsi qu’à un ensemble ciblé de correctifs conçus pour améliorer l’utilisation et les performances.

> [!IMPORTANT]
> Avec SSMA v 8.5, .NET 4.7.2 est une condition préalable à l’installation. Si vous avez besoin d’installer cette version, vous pouvez télécharger le fichier d’exécution [ici](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v84"></a>SSMA v 8.4

La version v 8.4 de SSMA pour MySQL a été améliorée avec des correctifs ciblés conçus pour résoudre des problèmes d’accessibilité et corriger un bogue lié aux colonnes d’index max (pour autoriser 32 au lieu de 16) pour SQL Server 2016 et versions ultérieures.

> [!IMPORTANT]
> Avec les versions de SSMA 7,4 à 8,4, .NET 4.5.2 est une condition préalable à l’installation.

## <a name="ssma-v83"></a>SSMA v 8.3

La version 8.3 de SSMA pour MySQL a été améliorée avec des correctifs ciblés conçus pour améliorer les mesures de qualité et de conversion. En outre, cette version de SSMA pour MySQL fournit des correctifs qui :

* Résoudre les problèmes d’accessibilité.
* Ajoutez la prise en charge de base pour le `hierarchyid` type dans SQL Server.

## <a name="ssma-v82"></a>SSMA v 8.2

La version 8.2 de SSMA pour MySQL est améliorée avec un ensemble ciblé de correctifs conçus pour améliorer les mesures de qualité et de conversion, ainsi que les correctifs pour :

* Un problème avec les index non cluster désactivés après la migration des données.
* Détection des .NET Framework pendant l’installation sans assistance.
* Incident intermittent qui se produit lorsqu’une nouvelle version est téléchargée.

> [!NOTE]
> Un problème connu avec la mise à jour automatique peut entraîner l’échec d’une mise à jour de SSMA v 8.1 à v 8.2. Si vous rencontrez cette erreur, téléchargez la nouvelle version et installez-la manuellement.

## <a name="ssma-v81"></a>SSMA v 8.1

La version 8.1 de SSMA pour MySQL a été améliorée avec des correctifs ciblés conçus pour améliorer les mesures de qualité et de conversion.

> [!NOTE]
> Un problème connu avec la mise à jour automatique peut entraîner l’échec d’une mise à jour de SSMA v 8.0 à v 8.1. Si vous rencontrez cette erreur, téléchargez la nouvelle version et installez-la manuellement.

## <a name="ssma-v80"></a>SSMA v 8.0

La version v 8.0 de SSMA pour MySQL est améliorée avec des correctifs ciblés conçus pour améliorer les mesures de qualité et de conversion. Cette version offre également les nouvelles fonctionnalités suivantes :

* Prise en charge d' **Azure SQL Managed instance** en tant que cible. Vous pouvez maintenant créer des projets ciblant Azure SQL Managed Instance :

  ![Projet MI SQL](../media/ssma-newproject-sqldbmi.png)

* **Conseiller de réparation** après conversion. En savoir plus à ce sujet [ici](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/).

* Sélection préliminaire de base de données/schéma.

  Lors de la connexion à la source, l’utilisateur peut désormais sélectionner les bases de données/schémas intéressants. La sélection de seuls les schémas que vous envisagez de migrer permet de gagner du temps lors de la connexion initiale et d’améliorer les performances globales de SSMA.

  ![Objets de filtre SSMA](../media/ssma-filter-objects.png)

## <a name="ssma-v710"></a>SSMA v 7.10

La version v 7.10 de SSMA pour MySQL contient les modifications suivantes :

* Des correctifs ciblés conçus pour fournir des protections de sécurité et de confidentialité supplémentaires pour répondre aux modifications des exigences globales.
* Correctif pour la conversion d’espaces entre le nom de fonction et la liste d’arguments.

## <a name="ssma-v79"></a>SSMA v 7.9

La version v 7.9 de SSMA pour MySQL contient les modifications suivantes :

* Correctifs ciblés qui améliorent les mesures de qualité et de conversion.
* Prise en charge partielle de la migration des types de données spatiales de MySQL vers Azure SQL Database.
* Prise en charge dans la ligne de commande SSMA pour modifier le mappage du type de données et les préférences du projet.
* Prise en charge de la migration de données à l’aide de SQL Server Integration Services (SSIS). Après la conversion du schéma, il est possible de créer un package SSIS à l’aide d’un clic droit de la bouton du menu contextuel.
* La boîte de dialogue de connexion Azure SQL Database dans SSMA a également été modifiée pour spécifier le nom de serveur complet. Dans les versions précédentes de SSMA, le préfixe Azure SQL Database devait être mentionné explicitement dans les paramètres des projets.

## <a name="ssma-v78"></a>SSMA v 7.8

La version 7.8 de SSMA pour MySQL contient les modifications suivantes :

* Modifiez le mappage de type mis en surbrillance dans les paramètres du projet.
* La possibilité pour les utilisateurs de désactiver la télémétrie.

## <a name="ssma-v77"></a>SSMA v 7.7

La version 7.7 de SSMA pour MySQL contient les modifications suivantes :

* SSMA pour MySQL a été amélioré avec des correctifs ciblés qui améliorent la qualité et les mesures de conversion.
* En fonction de la demande populaire, la version 32 bits de SSMA pour MySQL est de retour. Par rapport à l’implémentation précédente (avant la version 7.4), il existe deux packages d’installation, mais ils ne peuvent pas être installés côte à côte. Par conséquent, vous devez choisir la version la plus appropriée en fonction des composants de connectivité dont vous disposez. Il est toujours préférable d’utiliser la version 64 bits, si possible.
* SSMA pour MySQL dispose désormais du mode de connexion de chaîne de connexion ODBC, qui vous permet d’utiliser des pilotes ODBC tiers compatibles avec MySQL.

## <a name="ssma-v76"></a>SSMA v 7.6

La version v 7.6 de SSMA pour MySQL a été améliorée avec des correctifs ciblés qui améliorent les mesures de qualité et de conversion, ainsi que la prise en charge de SQL Server 2017 (version préliminaire publique). La prise en charge de SQL Server 2017 sur Windows et Linux est en version préliminaire publique et ne doit pas être utilisée pour les migrations de production.

## <a name="ssma-v75"></a>SSMA v 7.5

La version 7.5 de SSMA pour MySQL a été améliorée avec plusieurs améliorations pour garantir une meilleure accessibilité aux personnes handicapées.

## <a name="ssma-v74"></a>SSMA v 7.4

La version 7.4 de SSMA pour MySQL contient les modifications suivantes :

* L’option **délai de requête** est désormais disponible pendant la découverte d’objets de schéma à la source et à la cible.

    ![option délai d’expiration de la requête](../media/query-timeout_red.png)
* La mesure de qualité et de conversion a été améliorée avec les correctifs ciblés, en fonction des commentaires des clients.

> [!IMPORTANT]
> .NET 4.5.2 est une condition préalable à l’installation de SSMA v 7.4. En outre, à compter de la version 7.4, la version 32 bits de SSMA n’est plus disponible.

## <a name="ssma-v73"></a>SSMA v 7.3

La version 7.3 de SSMA pour MySQL contient les modifications suivantes :

* Amélioration de la qualité et de la mesure de conversion avec des correctifs ciblés basés sur les commentaires des clients.
* Infrastructure d’extensibilité SSMA exposée via les éléments suivants :
  * Exportez les fonctionnalités vers un projet SQL Server Data Tools (SSDT).
    * Vous pouvez maintenant exporter des scripts de schéma de SSMA vers un projet SSDT. Vous pouvez utiliser les scripts de schéma pour apporter des modifications de schéma supplémentaires et déployer votre base de données.

      ![Enregistrer en tant que projet SSDT, commande](../media/export-schema-scripts_red.png)
  * Bibliothèques pouvant être consommées par SSMA pour effectuer des conversions personnalisées.
    * Vous pouvez désormais construire du code capable de gérer les conversions de syntaxe personnalisées et les conversions qui n’ont pas été traitées précédemment par SSMA.
      * Des instructions sur la construction d’un convertisseur personnalisé sont disponibles dans ce billet de blog, qui [étend les fonctionnalités de conversion de Assistant Migration SQL Server](https://blogs.msdn.microsoft.com/datamigration/2017/02/21/2185/).
      * Téléchargez un exemple de projet pour la conversion à partir de ce billet de [blog](https://blogs.msdn.microsoft.com/datamigration/ssmafororacleconversionsample/).

## <a name="ssma-v72"></a>SSMA v 7.2

La version 7.2 de SSMA pour MySQL contient les modifications suivantes :

* Amélioration de la qualité et de la mesure de conversion avec des correctifs ciblés basés sur les commentaires des clients.
* Améliorations de la télémétrie pour fournir de meilleurs points de données afin de résoudre les problèmes des clients et d’améliorer les taux de conversion de SSMA.

## <a name="ssma-v71"></a>SSMA v 7.1

La version 7.1 de SSMA pour MySQL contient les modifications suivantes :

* SQL Server 2017 sur Windows et Linux CTP1 est désormais une plateforme cible prise en charge pour la migration. Cette fonctionnalité est en version d’évaluation technique et permet le déplacement des schémas et des données vers les serveurs SQL Server.
* SSMA prend désormais en charge les mises à jour automatiques pour télécharger la dernière version de SSMA dès qu’elle est disponible.
* Les fichiers binaires de SSMA installables sont désormais fournis via des fichiers de package Windows Installer (. msi).

## <a name="may-2016"></a>Mai 2016

La version 2016 de SSMA pour MySQL contient les modifications suivantes :

* Ajout de la prise en charge de SQL Server 2016.
* Analyseur et résolveur améliorés.
* Vérification de l’installation de .NET 2,0 supprimée.
* Dépendance de Pack d’extension mise à jour entre .NET 3,5 et .NET 4,0.
* Correction du mappage de type BigInt par défaut pour MySql.
* Correction `save-project` des `open-project` commandes et de la console SSMA.
* Correction `securepassword` de la commande pour la console SSMA.
* Correction du décompte des objets pour le chargement initial.
* Le chargement d’objets MsSql fixes.
* Correction du bogue dans les paramètres globaux.

## <a name="march-2016"></a>Mars 2016

La version préliminaire de de SSMA pour MySQL de mars 2016 ajoute la prise en charge de la migration vers SQL Server 2016.
  
## <a name="january-2016"></a>Janvier 2016

La version de maintenance de SSMA pour MySQL de janvier 2016 contient les modifications suivantes :

* Ajout de l’élément de menu de l’affichage du journal à SSMA (RFC 5706203).
* Ajout de données de télémétrie.
  
## <a name="july-2014"></a>Juillet 2014

La version de juillet 2014 de SSMA pour MySQL contient les modifications suivantes :
  
* Amélioration de la conversion du code Azure SQL Database.
* Fonctionnalités du pack d’extension déplacées vers le schéma pour prendre en charge Azure SQL Database.
* Améliorations des performances testées pour les bases de données avec plus de 10 000 objets.
* Améliorations de l’interface utilisateur pour le traitement d’un grand nombre d’objets.
* Mise en surbrillance des schémas LOB bien connus (afin qu’ils puissent être ignorés lors de la conversion).
* Améliorations de la vitesse de conversion.
* Affiche le nombre d’objets dans l’interface utilisateur.
* Réduction de la taille des rapports de plus de 25%.
* Amélioration des messages d’erreur pour les constructions non analysées.
  
## <a name="april-2014"></a>Avril 2014

La version d’avril 2014 de SSMA pour MySQL contient les modifications suivantes :
  
* Ajout de la prise en charge de SQL Server 2014.
* Correction des bogues concernant la conversion vers Azure.
* Correction des bogues concernant les pages de rapport invisibles dans IE 10.
  
## <a name="july-2011"></a>juillet 2011

La version de juillet 2011 de SSMA pour MySQL contient les modifications suivantes :
  
* Prise en charge de la conversion de `LIMIT` en [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] `OFFSET` .
* Amélioration des rapports d’erreurs lors de la migration des données.
  
## <a name="april-2011"></a>2011 avril

La version d’avril 2011 de SSMA pour MySQL contient les modifications suivantes :
  
* Installation unique de « SSMA pour MySQL », qui prend en [!INCLUDE [ssVersion2005](../../includes/ssversion2005-md.md)] charge [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)] , [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] et Azure SQL.
* La possibilité de se connecter [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] .
* Moteur de migration de données côté client amélioré, prenant en charge la migration parallèle des données.
* Amélioration des performances de migration des données avec des modes de récupération simple et journalisée en bloc.
* SSMA pour la version de la console MySQL prend en charge la compatibilité descendante. Vous pouvez ouvrir les projets créés par les versions antérieures à SSMA v 5.0.
* SSMA pour MySQL v 5.0 Product peut être installé côte à côte (SxS) avec des versions antérieures du produit SSMA.
  
## <a name="july-2010"></a>Juillet 2010

La version de juillet 2010 de SSMA pour MySQL contient les fonctionnalités suivantes :
  
**1. améliorations apportées à l’interface utilisateur :**

* Onglet « modes SQL » pour les objets de base de données MySQL
* Onglet’paramètres’pour les objets de base de données MySQL
* Onglet « données » pour les tables MySQL
* Paramètres du projet mis à jour dans les pages de conversion et de migration
* « Paramètres de migration de données » au niveau de la table
  
**2. améliorations de la connexion à MySQL et SQL Server :**
  
* Connectivité SSL dans MySQL
* Connectivité chiffrée dans SQL Server
  
**3. améliorations apportées à MySQL Metabase Explorer :**
  
* Chargement de tous les objets de base de données MySQL et de leurs onglets respectifs.
  
**4. améliorations apportées à la conversion d’objets :**
  
* Conversion d’objets de la métabase MySQL (procédures, fonctions, vues, déclencheurs et instructions).
* Prise en charge limitée des types de données spatiales dans les tables.
* Option permettant de convertir des fonctions MySQL en SQL Server des procédures stockées
* Option permettant d’appliquer les modes SQL et le mappage de jeux de caractères lors de la conversion d’objets
  
**5. améliorations de la migration des données :**  
  
* Prise en charge de la migration des données à l’aide des Server-Side et des moteurs de migration de données Client-Side
* Prise en charge de la migration de données spatiales
* SQL personnalisé pour la migration de données pour les tables
  
**6. SSMA pour la console MySQL :**  
  
* Fonctionnalité de console de prise en charge pour SSMA pour MySQL  
* Prise en charge de Script-Level interfaçage  
  
## <a name="january-2010"></a>2010 janvier

La version de 2010 janvier de SSMA pour MySQL était la version initiale. Il contenait les fonctionnalités suivantes :
  
* Ajout de la prise en charge de la migration vers les SQL Server locaux et Azure SQL.  
  
* **Instantané de fonctionnalité :** Migration du schéma et des données des tables/index/contraintes MySQL.
