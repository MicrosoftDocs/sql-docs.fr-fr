---
description: Connexion à Azure SQL Database (MySQLToSQL)
title: Connexion à Azure SQL Database (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connecting to Azure SQL Database, SQL Azure permissions
- Connecting to Azure SQL Database, synchronization
ms.assetid: d0b6f16a-1880-459d-a0c7-28b7ef15c56a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: ad03f4f1dda77ddae21138af8581dfba4c8f8bf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068933"
---
# <a name="connecting-to-azure-sql-database-mysqltosql"></a>Connexion à Azure SQL Database (MySQLToSQL)

Pour migrer des bases de données MySQL vers [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , vous devez vous connecter à l’instance cible de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] . Quand vous vous connectez, SSMA obtient les métadonnées relatives à toutes les bases de données dans l’instance de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] et affiche les métadonnées de la base de données dans l' **Explorateur de métadonnées Azure SQL Database**. SSMA stocke les informations de l’instance de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] à laquelle vous êtes connecté, mais ne stocke pas les mots de passe.

Votre connexion [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] reste active jusqu’à ce que vous fermiez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] si vous souhaitez une connexion active au serveur. Vous pouvez travailler hors connexion jusqu’à ce que vous chargeant des objets de base de données dans [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] et migriez les données.

Les métadonnées relatives à l’instance de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] ne sont pas synchronisées automatiquement. Au lieu de cela, pour mettre à jour les métadonnées dans **Azure SQL Database Explorateur de métadonnées**, vous devez mettre à jour manuellement les [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] métadonnées. Pour plus d’informations, consultez la section « synchronisation des métadonnées de Azure SQL Database » plus loin dans cette rubrique.

## <a name="required-azure-sql-database-permissions"></a>Autorisations de Azure SQL Database requises

Le compte utilisé pour se connecter à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] requiert des autorisations différentes en fonction des actions effectuées par le compte :

- Pour convertir des objets MySQL en [!INCLUDE[tsql](../../includes/tsql-md.md)] syntaxe, pour mettre à jour des métadonnées à partir de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] ou pour enregistrer la syntaxe convertie dans des scripts, le compte doit avoir l’autorisation de se connecter à l’instance de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

- Pour charger des objets de base de données dans [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , le compte doit être membre du rôle de base de données **db_ddladmin** .

- Pour migrer des données vers [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , le compte doit être membre du rôle de base de données **db_owner** .

## <a name="establishing-an-azure-sql-database-connection"></a>Établissement d’une connexion Azure SQL Database

Avant de convertir des objets de base de données MySQL en [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] syntaxe, vous devez établir une connexion à l’instance de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] dans laquelle vous souhaitez migrer la ou les bases de données MySQL.

Lorsque vous définissez les propriétés de connexion, vous spécifiez également la base de données dans laquelle les objets et les données seront migrés. Vous pouvez personnaliser ce mappage au niveau du schéma MySQL après vous être connecté à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] . Pour plus d’informations, consultez [mappage de bases de données MySQL à des schémas de SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md).

> [!IMPORTANT]
> Avant d’essayer de vous connecter à, assurez [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] -vous que votre adresse IP est autorisée via le [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] pare-feu.

Pour se connecter au [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] :

1. Dans le menu **fichier** , sélectionnez **se connecter à Azure SQL Database** (cette option est activée après la création d’un projet).
   Si vous vous êtes connecté précédemment à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , le nom de la commande sera **reconnecté à Azure SQL Database**.

2. Dans la boîte de dialogue connexion, entrez ou sélectionnez le nom du serveur [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

3. Entrez, sélectionnez ou **recherchez** le nom de la base de données.

4. Entrez ou sélectionnez **nom d’utilisateur**.

5. Entrez le **mot de passe**.

6. SSMA recommande la connexion chiffrée à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

7. Cliquez sur **Connecter**.
  
## <a name="synchronizing-azure-sql-database-metadata"></a>Synchronisation des métadonnées de Azure SQL Database

Les métadonnées relatives aux bases de données dans [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] ne sont pas automatiquement mises à jour. Les métadonnées dans **Azure SQL Database Explorateur de métadonnées** sont un instantané des métadonnées lors de la première connexion à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , ou la dernière fois que vous avez mis à jour manuellement les métadonnées. Vous pouvez mettre à jour manuellement les métadonnées de toutes les bases de données ou d’une base de données ou d’un objet de base de données unique. Pour synchroniser les métadonnées :

1. Assurez-vous que vous êtes connecté à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

2. Dans **Azure SQL Database Explorateur de métadonnées**, activez la case à cocher en regard de la base de données ou du schéma de base de données que vous souhaitez mettre à jour.
   Par exemple, pour mettre à jour les métadonnées de toutes les bases de données, activez la case à cocher en regard de **bases de données**.

3. Cliquez avec le bouton droit sur **bases** de données, ou sur la base de données individuelle ou le schéma de base de données, puis sélectionnez **synchroniser avec la base de** données.

## <a name="next-step"></a>étape suivante

L’étape suivante de la migration dépend des besoins de votre projet :

- Pour personnaliser le mappage entre les schémas MySQL et [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , consultez [mappage de bases de données MySQL à des schémas de SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md).
- Pour personnaliser les options de configuration des projets, consultez [définition des options de projet &#40;MySQLToSQL&#41;](../../ssma/mysql/setting-project-options-mysqltosql.md).
- Pour personnaliser le mappage des types de données sources et cibles, consultez [mappage de MySQL et des types de données SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md).
- Si vous n’avez pas besoin d’effectuer ces tâches, vous pouvez convertir les définitions d’objet de base de données MySQL en [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] définitions d’objets. Pour plus d’informations, consultez [conversion de bases de données MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md).

## <a name="see-also"></a>Voir aussi

[Migration de bases de données MySQL vers SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)
