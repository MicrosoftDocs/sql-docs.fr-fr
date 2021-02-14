---
description: Assistant Migration (AccessToSQL)
title: Assistant Migration (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Migration Wizard dialog box
- Migration Wizard, adding Access databases
- Migration Wizard, Connect to SQL Azure
- Migration Wizard, Connect to SQL Server
- Migration Wizard, Link Tables
- Migration Wizard, Migration status
- Migration Wizard, New Project
- Migration Wizard, Selecting objects to migrate
ms.assetid: 5bab5914-b2ae-4795-8cf5-83e42d64bef2
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2f5f97b55b4d357c2caa2314c993806c61f51e07
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066664"
---
# <a name="migration-wizard-accesstosql"></a>Assistant Migration (AccessToSQL)
L’Assistant Migration vous guide tout au long de la migration d’une ou plusieurs bases de données d’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. À l’aide de l’Assistant, vous allez créer un projet, ajouter des bases de données au projet, sélectionner les objets à migrer, puis vous connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. Vous allez également convertir, charger et migrer les schémas d’accès et les données. Si vous le souhaitez, vous pouvez lier des tables d’accès à des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tables ou SQL Azure.  
  
La plupart des pages de l’Assistant Migration contiennent les mêmes options que les boîtes de dialogue SSMA existantes. Par conséquent, les pages de l’Assistant sont décrites ici, puis des liens sont fournis afin que vous puissiez en savoir plus sur les différentes options. Si une page contient des options uniques, elles sont documentées ici.  
  
## <a name="starting-the-migration-wizard"></a>Démarrage de l’Assistant Migration  
Par défaut, l’Assistant Migration s’affiche lorsque vous démarrez SSMA. Vous pouvez également démarrer l’Assistant dans le menu **fichier** en sélectionnant **Assistant Migration**.  
  
## <a name="welcome-page"></a>Page d’accueil  
La page d’accueil présente l’Assistant Migration et fournit l’option suivante pour démarrer l’Assistant.  
  
**Lancez cet Assistant au démarrage.**  
Par défaut, SSMA démarre l’Assistant Migration lorsque vous démarrez SSMA. Pour empêcher le démarrage automatique de l’Assistant, désactivez cette case à cocher.  
  
## <a name="create-new-project-page"></a>Page créer un nouveau projet  
La page créer un nouveau projet vous permet d’entrer le nom du fichier projet, l’emplacement et le type de projet de migration (la version de la SQL Server cible utilisée pour la migration). Pour plus d’informations, consultez [New Project (SSMA)](./new-project-ssma-accesstosql.md) .  
  
## <a name="add-access-databases-page"></a>Page Ajouter des bases de données Access  
La page Ajouter des bases de données Access est l’emplacement où vous ajoutez une ou plusieurs bases de données Access au projet. Vous pouvez ajouter des bases de données individuelles en cliquant sur **Ajouter des bases** de données, puis en sélectionnant les bases de données dans la fenêtre **ouvrir** . Vous pouvez rechercher des bases de données à l’aide du bouton **Rechercher des bases de données** . Pour plus d'informations, voir les rubriques suivantes :  
  
-   [Ajout et suppression de fichiers de base de données Access](adding-and-removing-access-database-files-accesstosql.md)  
  
-   [Assistant Rechercher des bases de données (Sélectionner des emplacements)](./find-databases-wizard-select-locations-accesstosql.md)  
  
-   [Assistant Rechercher des bases de données (Sélectionner des fichiers)](./find-databases-wizard-select-files-accesstosql.md)  
  
-   [Assistant Rechercher des bases de données (Vérifier la sélection)](./find-databases-wizard-verify-selection-accesstosql.md)  
  
## <a name="select-objects-to-migrate-page"></a>Page Sélectionner les objets à migrer  
Dans la page Sélectionner les objets à migrer, vous sélectionnez les objets à convertir. Vous pouvez sélectionner tous les objets, groupes d’objets ou objets individuels.  
  
**Pour sélectionner des objets**  
  
1.  Développez **accès-métabase**, puis développez **bases de données**.  
  
2.  Effectuez une ou plusieurs des actions suivantes :  
  
    -   Pour convertir toutes les bases de données, activez la case à cocher en regard de **bases de données**.  
  
    -   Pour convertir ou omettre des bases de données, activez ou désactivez la case à cocher en regard du nom de la base de données.  
  
    -   Pour convertir ou omettre des requêtes, développez la base de données, puis activez ou désactivez la case à cocher **requêtes** .  
  
    -   Pour convertir ou omettre des tables individuelles, développez la base de données, développez **tables**, puis activez ou désactivez la case à cocher en regard de la table.  
  
Si vous avez de nombreux objets, vous souhaiterez peut-être utiliser les options **avancées de sélection d’objet** dans le volet droit pour filtrer les objets de base de données Access. Par exemple, si vous sélectionnez **tables** dans le volet gauche, vous pouvez filtrer la liste des tables en entrant des chaînes dans la zone de **filtre** . Vous pouvez ensuite sélectionner ou effacer les tables filtrées pour la migration à l’aide des boutons situés en haut du volet.  
  
Pour plus d’informations sur le filtrage, consultez la section Options de la [Sélection avancée d’objets (SSMA Common)](../sybase/advanced-object-selection-sybasetosql.md).  
  
## <a name="connect-to-sql-server-page"></a>Page se connecter à SQL Server  
Sur la page se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous spécifiez les propriétés de connexion, puis vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [se connecter à SQL Server](connect-to-sql-server-accesstosql.md).
  
> [!IMPORTANT]  
> Dès que la connexion est établie, la page lier les **tables** s’affiche, dans laquelle vous pouvez lier les tables. Cliquez sur **suivant** pour démarrer la migration.  
  
## <a name="connect-to-sql-azure-page"></a>Page se connecter à SQL Azure  
Sur la page se connecter à SQL Azure, vous spécifiez les propriétés de connexion, puis vous vous connectez à SQL Azure. Pour créer une nouvelle base de données Azure, vous pouvez utiliser l’option **créer une base de données Azure** qui apparaît sur le bouton **Parcourir** . Pour plus d’informations, consultez [se connecter à SQL Azure](connect-to-azure-sql-db-accesstosql.md)  
  
> [!IMPORTANT]  
> Dès que la connexion est établie, la page lier les **tables** s’affiche, dans laquelle vous pouvez lier les tables. Cliquez sur le bouton **suivant** dans la page Liens pour démarrer la migration.  
  
## <a name="link-tables-page"></a>Page lier les tables  
La page lier les tables vous permet de lier vos tables Access d’origine aux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tables migrées ou SQL Azure. La liaison de tables modifie votre base de données Access afin que vos requêtes, formulaires, rapports et pages d’accès aux données utilisent les données de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure SQL Database ou au lieu des données de votre base de données Access.  
  
**Lier les tables**  
Activez la case à cocher **lier les tables** pour lier les tables d’accès aux tables migrées [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. Pour démarrer la migration, vous devez cliquer sur le bouton **suivant** .  
  
## <a name="migration-status-page"></a>Page État de la migration  
La page État de la migration affiche la progression de la conversion des schémas d’accès vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure schémas, le chargement des schémas convertis dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, puis la migration des données.  
  
Pour plus d’informations sur cette page, consultez [convertir, charger et migrer](./convert-load-and-migrate-accesstosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Prise en main avec Assistant Migration SQL Server pour Access &#40;AccessToSQL&#41;](../../ssma/access/getting-started-with-sql-server-migration-assistant-for-access-accesstosql.md)  
[Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[Référence de l’interface utilisateur (accès)](./user-interface-reference-accesstosql.md)  
