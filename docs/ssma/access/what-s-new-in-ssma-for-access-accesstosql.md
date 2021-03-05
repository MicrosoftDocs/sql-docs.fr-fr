---
title: Nouveautés de SSMA for Access (AccessToSQL) | Microsoft Docs
description: Découvrez les modifications apportées à Assistant Migration SQL Server (SSMA) for Access (AccessToSQL) pour chaque version.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: a24d3fc0-6911-4bfa-828a-197abf222e02
ms.author: alexiva
ms.openlocfilehash: b6d660d00b036c1bbc5008411a9f8e9a0bc77ea0
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102185846"
---
# <a name="whats-new-in-ssma-for-access-accesstosql"></a>Nouveautés de SSMA for Access (AccessToSQL)

Cet article répertorie les Assistant Migration SQL Server (SSMA) pour les modifications d’accès dans chaque version.

## <a name="ssma-v817"></a>SSMA v 8.17

La version v 8.17 de SSMA pour Access contient les modifications suivantes :

* Mettre à jour les rapports d’évaluation HTML pour utiliser l’éditeur moderne pour afficher du texte SQL

## <a name="ssma-v816"></a>SSMA v 8.16

La version v 8.16 de SSMA pour Access contient les modifications suivantes :

* Afficher le texte SQL pour les requêtes dans le rapport de conversion HTML
* Supprimer la prise en charge de l’analyseur hérité
* Résoudre le problème lié à l’actualisation des objets à partir de la base de données

## <a name="ssma-v815"></a>SSMA v 8.15

En plus de plusieurs améliorations de l’accessibilité, la version v 8.15 de SSMA pour Access contient les modifications suivantes :

* Ignorer les index créés automatiquement pour les clés étrangères
* Remodelez les rapports d’évaluation pour travailler dans des navigateurs modernes
* Utiliser l’autorité fournie par la base de données pour l’authentification Azure AD
* Améliorer la dénomination des instructions chargées à partir de fichiers

## <a name="ssma-v814"></a>SSMA v 8.14

En plus de plusieurs améliorations visant à garantir une plus grande accessibilité pour les personnes handicapées, la version v 8.14 de SSMA pour Access requiert une mise à niveau de projet, car elle stocke désormais la version complète du serveur source/cible dans les métadonnées du projet.

## <a name="ssma-v813"></a>SSMA v 8.13

La version v 8.13 de SSMA pour Access contient les modifications suivantes :

* Corriger la `ORDER BY` conversion avec la `UNION` clause
* Prise en charge des index uniques filtrés
* Considérer les casts de type implicite lors de la conversion des appels de procédure et de fonction

## <a name="ssma-v812"></a>SSMA v 8.12

La version v 8.12 de SSMA pour Access contient les modifications suivantes :

* Prise en charge du `BigInt` `Large Number` type de données ()
* Résolution améliorée du type de colonne
* Amélioration de la conversion des règles de validation de colonne
* Utilisation du dernier fournisseur ACE OLE DB disponible pour la migration des données

## <a name="ssma-v811"></a>SSMA v 8.11

La version 8.11 de SSMA pour Access contient les modifications suivantes :

* Utiliser la bibliothèque MSAL.NET pour l’authentification Azure Active Directory interactive

## <a name="ssma-v810"></a>SSMA v 8.10

La version 8.10 de SSMA pour l’accès contient des améliorations de performances mineures et des correctifs de bogues.

## <a name="ssma-v89"></a>SSMA v 8.9

La version v 8.9 de SSMA pour Access contient les modifications suivantes :

* Conversion améliorée pour les requêtes à référence automatique
* Correction du problème avec les caractères spéciaux dans le nom du projet

## <a name="ssma-v88"></a>SSMA v 8.8

La version v 8.8 de SSMA pour Access comprend les éléments suivants :

* Améliorations de la stabilité de la synchronisation des objets SQL Server
* Améliorations des performances de l’interface graphique lors de l’évaluation et de la conversion
* Nouvel analyseur syntaxique de la nouvelle syntaxe d’accès pour améliorer les performances de la conversion

## <a name="ssma-v87"></a>SSMA v 8.7

La version v 8.7 de SSMA pour Access a amélioré la conversion pour la `IIF` fonction dans les requêtes, ainsi que les correctifs mineurs et les améliorations des performances dans l’interface graphique utilisateur.

> [!IMPORTANT]
> Avec SSMA v 8.5 et versions ultérieures, .NET 4.7.2 est une condition préalable à l’installation. Si vous avez besoin d’installer cette version, vous pouvez télécharger le fichier d’exécution [ici](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v86"></a>SSMA v 8.6

En plus d’un ensemble ciblé de correctifs conçus pour améliorer la facilité d’utilisation et les performances, la version 8.6 de SSMA pour l’accès a été améliorée en ajoutant un paramètre qui permet aux utilisateurs d’omettre les propriétés étendues SSMA dans le code converti.

Pour tirer parti de ce paramètre, dans SSMA pour Access, accédez à **Outils**  >  **paramètres du projet**  >    >  **conversion** générale, puis sous **divers**, mettez à jour la valeur du paramètre **omettre les propriétés étendues** sur **Oui**.

![Paramètre d’omission des propriétés étendues](../access/media/ssma-omit-extended-properties.png)

> [!IMPORTANT]
> Avec SSMA v 8.5 et versions ultérieures, .NET 4.7.2 est une condition préalable à l’installation. Si vous avez besoin d’installer cette version, vous pouvez télécharger le fichier d’exécution [ici](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v85"></a>SSMA v 8.5

La version v 8.5 de SSMA pour Access est améliorée grâce à la prise en charge de l’authentification Azure Active Directory et de la prise en charge de base des fonctionnalités JSON dans SQL Server, ainsi qu’à un ensemble ciblé de correctifs conçus pour améliorer l’utilisation et les performances.

En outre, SSMA pour Access prend désormais en charge la conversion de plusieurs fonctions standard ( `ISNULL` , `IIF` , etc.).

> [!IMPORTANT]
> Avec SSMA v 8.5, .NET 4.7.2 est une condition préalable à l’installation. Si vous avez besoin d’installer cette version, vous pouvez télécharger le fichier d’exécution [ici](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v84"></a>SSMA v 8.4

La version v 8.4 de SSMA pour Access est améliorée avec des correctifs ciblés conçus pour résoudre des problèmes d’accessibilité et corriger un bogue lié à des colonnes d’index max (pour autoriser 32 au lieu de 16) pour SQL Server 2016 et versions ultérieures.

> [!IMPORTANT]
> Avec les versions de SSMA 7,4 à 8,4, .NET 4.5.2 est une condition préalable à l’installation.

## <a name="ssma-v83"></a>SSMA v 8.3

La version 8.3 de SSMA pour l’accès est améliorée avec des correctifs ciblés conçus pour améliorer les mesures de qualité et de conversion. En outre, cette version de SSMA pour Access fournit des correctifs qui :

* Résoudre les problèmes d’accessibilité.
* Ajoutez la prise en charge de base pour le `hierarchyid` type dans SQL Server.

## <a name="ssma-v82"></a>SSMA v 8.2

La version 8.2 de SSMA pour l’accès a été améliorée avec des correctifs ciblés conçus pour améliorer les mesures de qualité et de conversion.

> [!NOTE]
> Un problème connu avec la mise à jour automatique peut entraîner l’échec d’une mise à jour de SSMA v 8.1 à v 8.2. Si vous rencontrez cette erreur, téléchargez la nouvelle version et installez-la manuellement.

## <a name="ssma-v81"></a>SSMA v 8.1

La version 8.1 de SSMA pour l’accès a été améliorée avec des correctifs ciblés conçus pour améliorer les mesures de qualité et de conversion.

> [!NOTE]
> Un problème connu avec la mise à jour automatique peut entraîner l’échec d’une mise à jour de SSMA v 8.0 à v 8.1. Si vous rencontrez cette erreur, téléchargez la nouvelle version et installez-la manuellement.

## <a name="ssma-v80"></a>SSMA v 8.0

La version 8.0 de SSMA pour Access a été améliorée avec des correctifs ciblés conçus pour améliorer les mesures de qualité et de conversion. Cette version offre également les nouvelles fonctionnalités suivantes :

* Prise en charge d' **Azure SQL Managed instance** en tant que cible. Vous pouvez maintenant créer des projets ciblant Azure SQL Managed Instance :

  ![Projet MI SQL](../media/ssma-newproject-sqldbmi.png)

* **Conseiller de réparation** après conversion. En savoir plus à ce sujet [ici](https://techcommunity.microsoft.com/t5/Microsoft-Data-Migration/Accelerate-your-Oracle-migrations-with-new-machine-learning/ba-p/368733).

* Sélection préliminaire de base de données/schéma.

    Lors de la connexion à la source, l’utilisateur peut désormais sélectionner les bases de données/schémas intéressants. La sélection de seuls les schémas que vous envisagez de migrer permet de gagner du temps lors de la connexion initiale et d’améliorer les performances globales de SSMA.

   ![Objets de filtre SSMA](../media/ssma-filter-objects.png)

## <a name="ssma-v710"></a>SSMA v 7.10

La version de SSMA pour l’accès a été améliorée avec les correctifs ciblés conçus pour fournir des protections de sécurité et de confidentialité supplémentaires afin de répondre aux changements d’exigences globales.

## <a name="ssma-v79"></a>SSMA v 7.9

La version v 7.9 de SSMA pour Access contient les modifications suivantes :

* Correctifs ciblés qui améliorent les mesures de qualité et de conversion.
* Prise en charge dans la ligne de commande SSMA pour modifier le mappage du type de données et les préférences du projet.
* La boîte de dialogue de connexion Azure SQL Database dans SSMA a également été modifiée pour spécifier le nom de serveur complet. Dans les versions précédentes de SSMA, le préfixe Azure SQL Database devait être mentionné explicitement dans les paramètres des projets.

## <a name="ssma-v78"></a>SSMA v 7.8

La version 7.8 de SSMA pour l’accès contient les modifications suivantes :

* Modifiez le mappage de type mis en surbrillance dans les paramètres du projet.
* La possibilité pour les utilisateurs de désactiver la télémétrie.

## <a name="ssma-v77"></a>SSMA v 7.7

La version de SSMA pour Access du v 7.7 contient les modifications suivantes :

* Correctifs ciblés qui améliorent les mesures de qualité et de conversion.
* En fonction de la demande populaire, la version 32 bits de SSMA pour l’accès est de nouveau. Par rapport à l’implémentation précédente (avant la version 7.4), il existe deux packages d’installation, mais ils ne peuvent pas être installés côte à côte. Par conséquent, vous devez choisir la version la plus appropriée en fonction des composants de connectivité dont vous disposez. Il est toujours préférable d’utiliser la version 64 bits, si possible.

## <a name="ssma-v76"></a>SSMA v 7.6

La version de SSMA pour l’accès à v 7.6 est améliorée avec des correctifs ciblés qui améliorent les mesures de qualité et de conversion, ainsi que la prise en charge de SQL Server 2017 (version préliminaire publique). La prise en charge de SQL Server 2017 sur Windows et Linux est en version préliminaire publique et ne doit pas être utilisée pour les migrations de production.

## <a name="ssma-v75"></a>SSMA v 7.5

La version 7.5 de SSMA pour Access a été améliorée avec plusieurs améliorations pour garantir une meilleure accessibilité aux personnes handicapées.

## <a name="ssma-v74"></a>SSMA v 7.4

La version 7.4 de SSMA pour Access contient les modifications suivantes :

* L’option **délai de requête** est désormais disponible pendant la découverte d’objets de schéma à la source et à la cible.

  ![option délai d’expiration de la requête](../media/query-timeout_red.png)

* La mesure de qualité et de conversion a été améliorée avec les correctifs ciblés, en fonction des commentaires des clients.

  > [!IMPORTANT]
  > .NET 4.5.2 est une condition préalable à l’installation de SSMA v 7.4. En outre, à compter de la version 7.4, la version 32 bits de SSMA a été supprimée.

## <a name="ssma-v73"></a>SSMA v 7.3

La version 7.3 de SSMA pour Access contient les modifications suivantes :

* Amélioration de la qualité et de la mesure de conversion avec des correctifs ciblés basés sur les commentaires des clients.
* Infrastructure d’extensibilité SSMA exposée via les éléments suivants :
  * Exportez les fonctionnalités vers un projet SQL Server Data Tools (SSDT).
    * Vous pouvez maintenant exporter des scripts de schéma de SSMA vers un projet SSDT. Vous pouvez utiliser les scripts de schéma pour apporter des modifications de schéma supplémentaires et déployer votre base de données.

        ![Enregistrer en tant que projet SSDT, commande](../media/export-schema-scripts_red.png)
  * Bibliothèques pouvant être consommées par SSMA pour effectuer des conversions personnalisées.
    * Vous pouvez désormais construire du code capable de gérer les conversions de syntaxe personnalisées et les conversions qui n’ont pas été traitées précédemment par SSMA.
      * Des instructions sur la construction d’un convertisseur personnalisé, ainsi qu’un exemple de projet pour la conversion, sont disponibles dans le billet de blog [extension des fonctionnalités de conversion de Assistant Migration SQL Server](https://techcommunity.microsoft.com/t5/Microsoft-Data-Migration/Extending-SQL-Server-Migration-Assistant-s-conversion/ba-p/1004181).

## <a name="ssma-v72"></a>SSMA v 7.2

La version 7.2 de SSMA pour Access contient les modifications suivantes :

* Amélioration de la qualité et de la mesure de conversion avec des correctifs ciblés basés sur les commentaires des clients.
* Améliorations de la télémétrie pour fournir de meilleurs points de données afin de résoudre les problèmes des clients et d’améliorer les taux de conversion de SSMA.

## <a name="ssma-v71"></a>SSMA v 7.1

La version 7.1 de SSMA pour Access contient les modifications suivantes :

* SQL Server 2017 sur Windows et Linux CTP1 est désormais une plateforme cible prise en charge pour la migration. Cette fonctionnalité est en version d’évaluation technique et prend en charge le déplacement des schémas et des données vers les serveurs SQL cibles.
* SSMA prend désormais en charge les mises à jour automatiques pour télécharger la dernière version de SSMA dès qu’elle est disponible.
* Les fichiers binaires de SSMA installables sont désormais fournis via des fichiers de package Windows Installer (. msi).

## <a name="may-2016"></a>Mai 2016

La version 2016 de SSMA pour Access contient les modifications suivantes :

* Ajout de la prise en charge officielle de SQL Server 2016.
* Vérification de l’installation de .NET 2,0 supprimée.
* Correction `save-project` des `open-project` commandes et de la console SSMA.
* Correction `securepassword` de la commande pour la console SSMA.
* Correction du décompte des objets pour le chargement initial.
* Le chargement des données des tables fixes pour les onglets de l’interface utilisateur pour l’accès.
* Correction du bogue dans les paramètres globaux.

## <a name="march-2016"></a>Mars 2016

La version préliminaire de de SSMA pour Access de mars 2016 ajoute la prise en charge de la migration vers SQL Server 2016.

## <a name="january-2016"></a>Janvier 2016

La version de maintenance de SSMA pour l’accès de janvier 2016 contient les modifications suivantes :

* Correction de la fonction non valide pour la valeur par défaut d’un champ GUID (RFC 3894811).
* Correction du problème où le système ne répond plus lors de l’importation d’enregistrements dans SQL Database (Azure) (RFC 4919573).
* Ajout de l’élément de menu de l’affichage du journal à SSMA (RFC 5706203).
* Ajout de données de télémétrie.

## <a name="july-2014"></a>Juillet 2014

La version 2014 de SSMA pour Access contient les modifications suivantes :

* Amélioration de la conversion du code Azure SQL Database.
* La fonctionnalité du pack d’extension a été déplacée vers le schéma pour prendre en charge Azure SQL Database.
* Amélioration des performances testées pour les bases de données avec plus de 10 000 objets.
* Ajout d’améliorations de l’interface utilisateur pour le traitement d’un grand nombre d’objets.
* Ajout de la prise en charge de la mise en surbrillance des schémas LOB « bien connus » (afin qu’ils puissent être ignorés lors de la conversion).
* Amélioration de la vitesse de conversion ajoutée.
* Ajout de la prise en charge de l’indication du nombre d’objets dans l’interface utilisateur.
* Réduction de la taille du rapport de plus de 25%.
* Amélioration des messages d’erreur pour les constructions non analysées.

## <a name="april-2014"></a>Avril 2014

La version d’avril 2014 de SSMA pour Access contient les modifications suivantes :

* Ajout de la prise en charge de MS SQL Server 2014.
* Correction des bogues liés à la conversion en Azure.
* Correction des bogues liés aux pages de rapport invisibles dans IE 10.

## <a name="january-2012"></a>janvier 2012

La version du 2012 janvier de SSMA pour Access contient les modifications suivantes :

* A fourni l’option de ne pas conserver le nom d’utilisateur et le mot de passe pour les tables liées MS Access après la migration.
* Définissez des actions en cascade pour les références circulaires sur aucune action.
* Fourni des messages appropriés indiquant que les actions en cascade pour les références circulaires ont été définies sur aucune action.

## <a name="july-2011"></a>juillet 2011

La version de juillet 2011 de SSMA pour Access ajoute un rapport d’erreurs amélioré pendant la migration des données.

## <a name="april-2011"></a>2011 avril

La version d’avril 2011 de SSMA pour Access contient les modifications suivantes :

* Ajout d’un seul installable de « SSMA for Access », qui prend en charge [!INCLUDE [ssVersion2005](../../includes/ssversion2005-md.md)] , [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)] [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] et Azure SQL.
* Ajout de la possibilité de se connecter à [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] .
* Ajout de SSMA pour la prise en charge de la version de la console Access pour la compatibilité descendante. Vous pouvez ouvrir les projets créés par les versions antérieures à SSMA v 5.0.
* Ajout de la possibilité d’installer le produit SSMA v 5.0 côte à côte (SxS) avec les versions antérieures du produit SSMA.

## <a name="july-2010"></a>Juillet 2010

La version 2010 de SSMA pour Access contient les modifications suivantes :

* Ajout de la prise en charge de la migration vers SQL Server 2008 R2 et Azure SQL.
* Ajout d’une connexion sécurisée aux SQL Server et à SQL Azure.
* Ajout de la prise en charge des bases de données Access 2010.
* Ajout d’une nouvelle application console SSMA pour l’exécution de la ligne de commande.
* Ajout de la prise en charge du `DateTime2` type de données SQL Server.

## <a name="june-2008"></a>2008 juin

La version de 2008 de SSMA pour Access ajoute la prise en charge des bases de données Access 2007.

## <a name="may-2007"></a>2007 mai

La version 2007 de SSMA pour Access contient les modifications suivantes :

* Ajout de la prise en charge des bases de données Access qui utilisent des stratégies de groupe de travail.
* A fourni la possibilité de supprimer des objets convertis de l’Explorateur de métadonnées SQL Server.
* Ajout de la prise en charge des commentaires entrés par l’utilisateur en mode SQL mis en forme SQL Server.
* Améliorations de la conversion d’objets ajoutées.

## <a name="november-2006"></a>2006 novembre

La version de novembre 2006 de SSMA pour Access contient les modifications suivantes :

* Ajout d’un nouvel Assistant Migration de base de données qui vous guide tout au long de la migration d’une base de données à partir d’un accès à [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] .
* Ajout d’une nouvelle commande de conversion, de chargement et de migration qui convertit les bases de données Access, charge les objets convertis dans [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] et migre les données en [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] une seule étape.
* Migration améliorée des requêtes. La migration des requêtes convertit désormais davantage de requêtes SELECT en vues. Pour plus d’informations, consultez [conversion d’objets de base de données Access](converting-access-database-objects-accesstosql.md).
* Ajout de la possibilité de modifier les propriétés des tables et des index sous l' [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] onglet **table** .
* Nouveaux paramètres globaux ajoutés :
  * Vous pouvez choisir d’afficher les numéros de ligne dans les fenêtres de l’éditeur.
  * Vous pouvez configurer SSMA pour inviter à remplacer des objets dupliqués, ou toujours ou jamais remplacer des objets en double lors de la conversion de schéma.
* Ajout d’une nouvelle option de conversion qui vous permet de spécifier si SSMA affiche un avertissement quand une requête complexe contient un caractère générique.

## <a name="july-2006"></a>2006 juillet

La version de 2006 de SSMA pour Access était la version initiale.
