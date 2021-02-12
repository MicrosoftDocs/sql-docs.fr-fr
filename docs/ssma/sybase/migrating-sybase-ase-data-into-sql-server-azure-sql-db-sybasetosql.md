---
description: Migration de données Sybase ASE vers SQL Server Azure SQL Database (SybaseToSQL)
title: Migrer des données Sybase ASE vers SQL Server-Azure SQL Database | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Migrating data,Client Side Data Migration
- Migrating data,Server Side Data Migration
ms.assetid: 54a39f5e-9250-4387-a3ae-eae47c799811
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 0406997ade10e6ddfc9b13c76240973662255f78
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076030"
---
# <a name="migrating-sybase-ase-data-into-sql-server---azure-sql-database--sybasetosql"></a>Migration de données Sybase ASE vers SQL Server Azure SQL Database (SybaseToSQL)
Une fois que vous avez chargé avec succès les objets de base de données Sybase Adaptive Server Enterprise (ASE) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database, vous pouvez migrer des données de ASE vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database.  
  
> [!IMPORTANT]  
> Si le moteur utilisé est le moteur de migration des données côté serveur, avant de migrer les données, vous devez installer le pack d’extension SSMA pour Sybase ASE et les fournisseurs d’ASE Sybase sur l’ordinateur qui exécute SSMA. Le service SQL Server Agent doit également être en cours d’exécution. Pour plus d’informations sur l’installation du pack d’extension, consultez [installation des composants SSMA sur SQL Server (SybaseToSQL)](./installing-ssma-components-on-sql-server-sybasetosql.md)  
  
## <a name="setting-migration-options"></a>Définition des options de migration  
Avant de migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database, passez en revue les options de migration de projet dans la boîte de dialogue **paramètres du projet** .  
  
-   À l’aide de cette boîte de dialogue, vous pouvez définir des options telles que la taille de lot de migration, le verrouillage de table, la vérification des contraintes, la gestion des valeurs NULL et la gestion des valeurs d’identité. Pour plus d’informations sur les paramètres de migration de projet, consultez [paramètres du projet (migration) (Sybase)](./project-settings-migration-sybasetosql.md).  
  
    Pour plus d’informations sur les **paramètres de migration de données étendues**, consultez [paramètres de migration de données](data-migration-settings-sybasetosql.md) .  
  
-   Le **moteur de migration** de la boîte de dialogue **paramètres du projet** permet à l’utilisateur d’effectuer le processus de migration à l’aide de deux types de moteurs de migration de données, à savoir :  
  
    1.  Moteur de migration de données côté client  
  
    2.  Moteur de migration de données côté serveur  
  
**Migration des données côté client :**  
  
-   Pour lancer la migration des données côté client, sélectionnez l’option **moteur de migration de données côté client** dans la boîte de dialogue **paramètres du projet** .  
  
-   Dans les **paramètres du projet**, l’option moteur de migration de **données côté client** est définie par défaut.  
  
    > [!NOTE]  
    > Le moteur de migration de données Client-Side réside à l’intérieur de l’application SSMA et, par conséquent, n’est pas dépendant de la disponibilité du pack d’extension.  
  
**Migration des données côté serveur :**  
  
-   Pendant la migration des données côté serveur, le moteur réside sur la base de données cible. Il est installé par le biais du pack d’extension. Pour plus d’informations sur la façon d’installer le pack d’extension, consultez [installation des composants SSMA sur SQL Server (SybaseToSQL)](./installing-ssma-components-on-sql-server-sybasetosql.md)  
  
-   Pour lancer la migration côté serveur, sélectionnez l’option **moteur de migration de données côté serveur** dans la boîte de dialogue **paramètres du projet** .  
  
> [!NOTE]  
> Lorsque Azure SQL Database est utilisé en tant que base de données cible, seule la **migration des données côté client** est autorisée et la migration des données côté serveur n’est pas prise en charge.  
  
## <a name="migrating-data-to-sql-server-or-azure-sql-database"></a>Migration de données vers SQL Server ou Azure SQL Database  
La migration des données est une opération de chargement en masse qui déplace des lignes de données à partir des tables ASE vers SQL Server tables de transactions. Le nombre de lignes chargées dans SQL Server ou Azure SQL Database dans chaque transaction est configuré dans les paramètres du projet.  
  
Pour afficher les messages de migration, assurez-vous que le volet de sortie est visible. Dans le cas contraire, sélectionnez **sortie** dans le menu **affichage** .  
  
**Pour migrer des données**  
  
1.  Vérifiez les éléments suivants :  
  
    -   Les fournisseurs ASE sont installés sur l’ordinateur qui exécute SSMA.  
  
    -   Vous avez synchronisé les objets convertis avec la base de données cible (SQL Server ou Azure SQL Database).  
  
2.  Dans l’Explorateur de métadonnées Sybase, sélectionnez les objets qui contiennent les données que vous souhaitez migrer :  
  
    -   Pour migrer des données pour tous les schémas, activez la case à cocher en regard de **schémas**.  
  
    -   Pour migrer des données ou omettre des tables individuelles, commencez par développer le schéma, développez **tables**, puis activez ou désactivez la case à cocher en regard de la table.  
  
3.  Pour migrer des données, deux cas se produisent :  
  
    **Migration des données côté client :**  
  
    Pour effectuer la **migration des données côté client**, sélectionnez l’option **moteur de migration de données côté client** dans la boîte de dialogue **paramètres du projet** .  
  
    **Migration des données côté serveur :**  
  
    -   Avant de procéder à la migration des données côté serveur, vérifiez les éléments suivants :  
  
        1.  Le pack d’extension SSMA pour Sybase est installé sur l’instance de SQL Server.  
  
        2.  Le service SQL Server Agent s’exécute sur l’instance de SQL Server  
  
    -   Pour effectuer la **migration des données côté serveur**, sélectionnez l’option **moteur de migration de données côté serveur** dans la boîte de dialogue **paramètres du projet** .  
  
4.  Cliquez avec le bouton droit sur **schémas** dans l’Explorateur de métadonnées Sybase, puis cliquez sur **migrer les données**. Vous pouvez également migrer des données pour des objets individuels ou des catégories d’objets : cliquez avec le bouton droit sur l’objet ou sur son dossier parent, puis sélectionnez l’option **migrer les données** .  
  
    > [!NOTE]  
    > Si le pack d’extension SSMA pour Sybase n’est pas installé sur l’instance de SQL Server et si le **moteur de migration de données côté serveur** est sélectionné, lors de la migration des données vers la base de données cible, l’erreur suivante s’est produite : «les composants de migration de données SSMA n’ont pas été trouvés sur SQL Server, la migration des données côté serveur n’est pas possible. Vérifiez si le pack d’extension est correctement installé. Cliquez sur **Annuler** pour mettre fin à la migration des données.  
  
5.  Dans la boîte de dialogue **connexion à Sybase ASE** , entrez les informations d’identification de connexion, puis cliquez sur **se connecter**. Pour plus d’informations sur la connexion à Sybase ASE, consultez [se connecter à sybase &#40;SybaseToSQL&#41;](../../ssma/sybase/connect-to-sybase-sybasetosql.md)  
  
    Si la base de données cible est SQL Server, entrez les informations d’identification de connexion dans la boîte de dialogue **se connecter à SQL Server** , puis cliquez sur **se connecter**. Pour plus d’informations sur la connexion à SQL Server, consultez [connexion à des SQL Server (SybaseToSQL)](./connecting-to-sql-server-sybasetosql.md) .  
  
    Si la base de données cible est Azure SQL Database, entrez les informations d’identification de connexion dans la boîte de dialogue **se connecter à Azure SQL Database** , puis cliquez sur **se connecter**. Pour plus d’informations sur la connexion à Azure SQL Database, consultez [connexion à Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-azure-sql-db-sybasetosql.md)  
  
    Les messages s’affichent dans le volet de **sortie** . Une fois la migration terminée, le **rapport de migration des données** s’affiche. Si des données n’ont pas été migrées, cliquez sur la ligne qui contient les erreurs, puis cliquez sur **Détails**. Lorsque vous avez terminé avec le rapport, cliquez sur **Fermer**. Pour plus d’informations sur le rapport de migration de données, consultez [rapport de migration des données (SSMA commun)](./data-migration-report-sybasetosql.md)  
  
> [!NOTE]  
> Lorsque SQL Express Edition est utilisé comme base de données cible, seule la migration des données côté client est autorisée et la migration des données côté serveur n’est pas prise en charge.  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données Sybase ASE vers SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
