---
title: Connexion à Azure SQL Database (AccessToSQL) | Microsoft Docs
description: Découvrez comment vous connecter à une instance cible de Azure SQL Database pour migrer des bases de données Access. SSMA obtient des métadonnées sur les bases de données dans Azure SQL Database.
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- instance of SQL Azure
- metadata, refreshing
- refreshing metadata
- SQL Azure
- SQL Azure, connecting
- SQL Azure, connecting to
- SQL Azure, reconnecting
- SQL Azure, synchronizing metadata
ms.assetid: 1ba0d113-dc05-4431-8689-e14a8821bafd
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 4f31b2e46312e89830d977ca0ce61ad07572c875
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076270"
---
# <a name="connecting-to-azure-sql-database-accesstosql"></a>Connexion à Azure SQL Database (AccessToSQL)

Pour migrer des bases de données Access vers [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , vous devez vous connecter à l’instance cible de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] . Quand vous vous connectez, SSMA obtient les métadonnées relatives à toutes les bases de données dans l’instance de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] et affiche les métadonnées de la base de données dans l' **Explorateur de métadonnées Azure SQL Database**. SSMA stocke des informations sur l’instance de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] à laquelle vous êtes connecté, mais ne stocke pas les mots de passe.

Votre connexion [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] reste active jusqu’à ce que vous fermiez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] si vous souhaitez une connexion active au serveur. Vous pouvez travailler hors connexion jusqu’à ce que vous chargeant des objets de base de données dans [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] et migriez les données.

Les métadonnées relatives à l’instance de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] ne sont pas synchronisées automatiquement. Au lieu de cela, pour mettre à jour les métadonnées dans **Azure SQL Database Explorateur de métadonnées**, vous devez mettre à jour manuellement les [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] métadonnées. Pour plus d’informations, consultez la section « synchronisation des métadonnées de Azure SQL Database » plus loin dans cette rubrique.

## <a name="required-azure-sql-database-permissions"></a>Autorisations de Azure SQL Database requises

Le compte utilisé pour se connecter à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] requiert des autorisations différentes en fonction des actions effectuées par le compte :

- Pour convertir des objets Access en [!INCLUDE[tsql](../../includes/tsql-md.md)] syntaxe, pour mettre à jour des métadonnées à partir de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] ou pour enregistrer la syntaxe convertie dans des scripts, le compte doit avoir l’autorisation de se connecter à l’instance de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

- Pour charger des objets de base de données dans [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , le compte doit être membre du rôle de base de données **db_ddladmin** .

- Pour migrer des données vers [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , le compte doit être membre du rôle de base de données **db_owner** .

## <a name="establishing-an-azure-sql-database-connection"></a>Établissement d’une connexion Azure SQL Database

Avant de convertir des objets de base de données Access en [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] syntaxe, vous devez établir une connexion à l’instance de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] dans laquelle vous souhaitez migrer la ou les bases de données Access.

Lorsque vous définissez les propriétés de connexion, vous spécifiez également la base de données dans laquelle les objets et les données seront migrés. Vous pouvez personnaliser ce mappage au niveau du schéma d’accès après vous être connecté à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] . Pour plus d’informations, consultez [mappage de bases de données Access à des schémas de SQL Server](mapping-source-and-target-databases-accesstosql.md).
  
> [!IMPORTANT]
> Avant d’essayer de vous connecter à, assurez [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] -vous que votre adresse IP est autorisée via le [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] pare-feu.
  
Pour se connecter au [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] :

1. Dans le menu **fichier** , sélectionnez **se connecter à SQL Azure** (cette option est activée après la création d’un projet).
   Si vous vous êtes connecté précédemment à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , le nom de la commande sera **reconnecté à SQL Azure**.

2. Dans la boîte de dialogue connexion, entrez ou sélectionnez le nom du serveur [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

3. Entrez, sélectionnez ou **Parcourez** le nom de la base de données.

4. Entrez ou sélectionnez **nom d’utilisateur**.

5. Entrez le **mot de passe**.

6. SSMA recommande la connexion chiffrée à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

7. Cliquez sur **Connecter**.
  
S’il n’existe aucune base de données dans le [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , vous pouvez créer la première base de données à l’aide de l’option **créer une base de données Azure** qui apparaît sur le bouton **Parcourir** .

## <a name="synchronizing-azure-sql-database-metadata"></a>Synchronisation des métadonnées de Azure SQL Database

Les métadonnées relatives aux bases de données dans [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] ne sont pas automatiquement mises à jour. Les métadonnées dans **Azure SQL Database Explorateur de métadonnées** sont un instantané des métadonnées lors de la première connexion à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , ou la dernière fois que vous avez mis à jour manuellement les métadonnées. Vous pouvez mettre à jour manuellement les métadonnées de toutes les bases de données ou d’une base de données ou d’un objet de base de données unique. Pour synchroniser les métadonnées :

1. Assurez-vous que vous êtes connecté à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

2. Dans **Azure SQL Database Explorateur de métadonnées**, activez la case à cocher en regard de la base de données ou du schéma de base de données que vous souhaitez mettre à jour.
   Par exemple, pour mettre à jour les métadonnées de toutes les bases de données, activez la case à cocher en regard de **bases de données**.

3. Cliquez avec le bouton droit sur **bases** de données, ou sur la base de données individuelle ou le schéma de base de données, puis sélectionnez **synchroniser avec la base de** données.

## <a name="refreshing-azure-sql-database-metadata"></a>Actualisation des métadonnées de Azure SQL Database

Si [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] les schémas changent après la connexion, vous pouvez actualiser les métadonnées à partir du serveur.

Pour actualiser les [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] métadonnées :

- Dans **Azure SQL Database Explorateur de métadonnées**, cliquez avec le bouton droit sur **bases de données**, puis sélectionnez **Actualiser à partir de la base de données**.

## <a name="reconnecting-to-azure-sql-database"></a>Reconnexion à Azure SQL Database

Votre connexion [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] reste active jusqu’à ce que vous fermiez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] si vous souhaitez une connexion active au serveur. Vous pouvez travailler hors connexion jusqu’à ce que vous chargeant des objets de base de données dans [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] et migriez les données.

La procédure de reconnexion à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] est identique à celle de la procédure d’établissement d’une connexion.

## <a name="next-steps"></a>Étapes suivantes

L’étape suivante de la migration dépend des besoins de votre projet :

- Pour personnaliser le mappage entre les schémas d’accès et les Azure SQL Database, consultez [mappage de bases de données Access à des schémas de SQL Server](mapping-source-and-target-databases-accesstosql.md).
- Pour personnaliser les options de configuration des projets, consultez [définition des options du projet](setting-conversion-and-migration-options-accesstosql.md).
- Pour personnaliser le mappage des types de données sources et cibles, consultez [mappage des types de données source et cible](mapping-source-and-target-data-types-accesstosql.md).
- Si vous n’avez pas besoin d’effectuer ces tâches, vous pouvez convertir les définitions d’objet de base de données Access en SQL Azure définitions d’objets. Pour plus d’informations, consultez [conversion de bases de données Access](converting-access-database-objects-accesstosql.md).

## <a name="see-also"></a>Voir aussi

[Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)
