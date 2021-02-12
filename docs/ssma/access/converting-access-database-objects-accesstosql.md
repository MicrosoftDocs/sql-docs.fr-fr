---
title: Conversion d’objets de base de données Access (AccessToSQL) | Microsoft Docs
description: Découvrez comment sélectionner des objets de base de données Access après vous être connecté à SQL Server/Azure SQL Database, puis convertir les schémas en schémas SQL Server/SQL Database.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases
- Access databases, converting schemas
- conversion
- conversion, converting schemas
- indexes, altering
- metadata
- metadata, altering
- metadata, converting
- migrating databases, one-click
- one-click migration
- schemas
- schemas, converting
- SQL
- SQL, converting
- syntax
- syntax, converting
- tables, altering
- translating Access to SQL Azure
- translating Access to SQL Server
ms.assetid: e0ef67bf-80a6-4e6c-a82d-5d46e0623c6c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 38385657de137e9e13bf03f7a4209be1303452d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076075"
---
# <a name="converting-access-database-objects-accesstosql"></a>Conversion d’objets de base de données Access (AccessToSQL)
Une fois que vous avez ajouté des bases de données Access et que vous vous êtes connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, SSMA affiche les métadonnées des objets Access et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure SQL Database. Vous pouvez maintenant sélectionner accéder aux objets de base de données, puis convertir les schémas dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure schémas.  
  
## <a name="the-conversion-process"></a>Processus de conversion  
La conversion d’objets de base de données prend les définitions d’objet des métadonnées d’accès, les convertit en syntaxe équivalente [!INCLUDE[tsql](../../includes/tsql-md.md)] , puis charge ces informations dans le projet. Vous pouvez ensuite afficher les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets ou SQL Azure et leurs propriétés à l’aide de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure Explorateur de métadonnées.  
  
> [!IMPORTANT]  
> La conversion d’objets ne crée pas les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. Il convertit uniquement les définitions d’objets et stocke les informations dans le projet SSMA.  
  
Pendant la conversion, SSMA imprime l’État dans le volet de sortie, ainsi que les messages d’erreur, d’avertissement et d’information dans le volet Liste d’erreurs. Utilisez ces informations pour déterminer si vous devez modifier vos bases de données Access ou votre processus de conversion pour obtenir les résultats de conversion souhaités. Vous pouvez également utiliser les informations de la rubrique [préparation des bases de données Access pour la migration](preparing-access-databases-for-migration-accesstosql.md) pour déterminer ce qui va et ne sera pas converti.  
  
## <a name="setting-conversion-options"></a>Définition des options de conversion  
Avant de convertir des objets, passez en revue les options de conversion de projet dans la boîte de dialogue **paramètres du projet** . À l’aide de cette boîte de dialogue, vous pouvez définir la manière dont SSMA convertit les colonnes de mémo indexées, les clés primaires, les contraintes de clé étrangère, les horodateurs et les tables sans index. Pour plus d’informations, consultez [paramètres du projet (conversion)](./project-settings-conversion-accesstosql.md) .  
  
## <a name="conversion-results"></a>Résultats de la conversion  
Le tableau suivant répertorie les objets Access qui sont convertis et les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets résultants ou SQL Azure :  
  
|Objet d’accès|Objet SQL Server résultant|  
|-----------------|-------------------------------|  
|table|table|  
|colonne|colonne|  
|index|index|  
|clé étrangère|clé étrangère|  
|query|vue<br /><br />La plupart des requêtes SELECT sont converties en vues. Les autres requêtes, telles que les requêtes de mise à jour, ne sont pas migrées.<br /><br />Les requêtes SELECT qui acceptent des paramètres ne sont pas converties, et ne sont pas des requêtes à onglets croisées.|  
|rapport|non converti|  
|form|non converti|  
|macro|non converti|  
|module|non converti|  
|valeur par défaut|valeur par défaut|  
|autoriser la propriété de colonne de longueur nulle|contrainte de validation|  
|règle de validation de colonne|contrainte de validation|  
|règle de validation de table|contrainte de validation|  
|clé primaire|clé primaire|  
  
## <a name="converting-access-objects"></a>Conversion d’objets Access  
Pour convertir des objets de base de données Access, vous devez d’abord sélectionner les objets que vous souhaitez convertir, puis demander à SSMA d’effectuer la conversion. Pour afficher les messages de sortie pendant la conversion, dans le menu **affichage** , sélectionnez **sortie**.  
  
**Pour sélectionner et convertir des objets de base de données Access en SQL Server ou SQL Azure syntaxe**  
  
1.  Dans l’Explorateur de métadonnées Access, développez **accès-métabase**, puis développez **bases de données**.  
  
2.  Effectuez une ou plusieurs des actions suivantes :  
  
    -   Pour convertir toutes les bases de données, activez la case à cocher en regard de **bases de données**.  
  
    -   Pour convertir ou omettre des bases de données, activez ou désactivez la case à cocher en regard du nom de la base de données.  
  
    -   Pour convertir ou omettre des requêtes, développez la base de données, puis activez ou désactivez la case à cocher **requêtes** .  
  
    -   Pour convertir ou omettre des tables individuelles, développez la base de données, développez **tables**, puis activez ou désactivez la case à cocher en regard de la table.  
  
3.  Effectuez l’une des opérations suivantes :  
  
    -   Pour convertir des schémas, cliquez avec le bouton droit sur **bases de données** , puis sélectionnez **convertir le schéma**.  
  
        Vous pouvez également convertir des objets individuels. Pour convertir un objet, quels que soient les objets sélectionnés, cliquez avec le bouton droit sur l’objet et sélectionnez **convertir le schéma**.  
  
        Lorsqu’un objet a été converti, il apparaît en gras dans l’Explorateur de métadonnées Access.  
  
    -   Pour convertir, charger et migrer des schémas et des données en une seule étape, cliquez avec le bouton droit sur bases de données, puis sélectionnez **convertir, charger et migrer**.  
  
4.  Examinez les messages dans le volet de **sortie** et les erreurs et les avertissements dans le volet **liste d’erreurs** .  
  
## <a name="altering-tables-and-indexes"></a>Modification des tables et des index  
Après avoir converti les métadonnées d’accès vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure métadonnées, et avant de charger les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, vous pouvez modifier [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure des tables et des index.  
  
**Pour modifier les propriétés d’une table ou d’un index**  
  
1.  Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure l’Explorateur de métadonnées, sélectionnez la table ou l’index que vous souhaitez modifier.  
  
2.  Sous l’onglet **table** , cliquez sur la propriété que vous souhaitez modifier, puis entrez ou sélectionnez le nouveau paramètre. Par exemple, vous pouvez remplacer nvarchar (15) par nvarchar (20) ou activer une case à cocher pour rendre une colonne de table Nullable.  
  
    Déplacez le curseur en dehors de la cellule de propriété modifiée. Pour ce faire, cliquez sur une autre ligne ou appuyez sur la touche Tab.  
  
3.  Cliquez sur **Appliquer**.  
  
Vous pouvez maintenant afficher les modifications dans le code sous l’onglet **SQL** .  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante du processus de migration consiste [à charger des objets de base de données convertis en SQL Server](loading-converted-database-objects-into-sql-server-accesstosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
