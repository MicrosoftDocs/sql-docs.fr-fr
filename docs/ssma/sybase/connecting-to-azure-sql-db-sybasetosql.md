---
description: Connexion à Azure SQL Database (SybaseToSQL)
title: Connexion à Azure SQL Database (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/16/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 9e77e4b0-40c0-455c-8431-ca5d43849aa7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9407f646eeded6a24fcaf166492abe4792b9b609
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100057064"
---
# <a name="connecting-to-azure-sql-database-sybasetosql"></a>Connexion à Azure SQL Database (SybaseToSQL)

Pour migrer des bases de données Sybase vers [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , vous devez vous connecter à l’instance cible de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] . Quand vous vous connectez, SSMA obtient les métadonnées relatives à toutes les bases de données dans l’instance de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] et affiche les métadonnées de la base de données dans l' **Explorateur de métadonnées Azure SQL Database**. SSMA stocke les informations de l’instance de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] à laquelle vous êtes connecté, mais ne stocke pas les mots de passe.

Votre connexion [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] reste active jusqu’à ce que vous fermiez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] si vous souhaitez une connexion active au serveur. Vous pouvez travailler hors connexion jusqu’à ce que vous chargeant des objets de base de données dans [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] et migriez les données.

Les métadonnées relatives à l’instance de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] ne sont pas synchronisées automatiquement. Au lieu de cela, pour mettre à jour les métadonnées dans **Azure SQL Database Explorateur de métadonnées**, vous devez mettre à jour manuellement les [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] métadonnées. Pour plus d’informations, consultez la section « synchronisation des métadonnées de Azure SQL Database » plus loin dans cette rubrique.

## <a name="required-azure-sql-database-permissions"></a>Autorisations de Azure SQL Database requises

Le compte utilisé pour se connecter à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] requiert des autorisations différentes en fonction des actions effectuées par le compte :

- Pour convertir des objets ASE en [!INCLUDE[tsql](../../includes/tsql-md.md)] syntaxe, pour mettre à jour des métadonnées à partir de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] ou pour enregistrer la syntaxe convertie dans des scripts, le compte doit avoir l’autorisation de se connecter à l’instance de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

- Pour charger des objets de base de données dans [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , le compte doit être membre du rôle de base de données **db_ddladmin** .

- Pour migrer des données vers [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , le compte doit être membre du rôle de base de données **db_owner** .

- Pour exécuter le code généré par SSMA, le compte doit avoir les `EXECUTE` autorisations pour toutes les fonctions définies par l’utilisateur dans le schéma **ssma_syb** de la base de données cible. Ces fonctions fournissent des fonctionnalités équivalentes des fonctions système ASE et sont utilisées par les objets convertis.

## <a name="establishing-an-azure-sql-database-connection"></a>Établissement d’une connexion Azure SQL Database

Avant de convertir des objets de base de données Sybase en [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] syntaxe, vous devez établir une connexion à l’instance de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] dans laquelle vous souhaitez migrer la ou les bases de données Sybase.

Lorsque vous définissez les propriétés de connexion, vous spécifiez également la base de données dans laquelle les objets et les données seront migrés. Vous pouvez personnaliser ce mappage au niveau du schéma Sybase après vous être connecté à Azure SQL Database. Pour plus d’informations, consultez [mappage de schémas Sybase ASE à des schémas de SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql.md).

> [!IMPORTANT]
> Avant d’essayer de vous connecter à, assurez [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] -vous que votre adresse IP est autorisée via le [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] pare-feu.

Pour se connecter au [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] :

1. Dans le menu **fichier** , sélectionnez **se connecter à Azure SQL Database**(cette option est activée après la création d’un projet).
   Si vous vous êtes connecté précédemment à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , le nom de la commande sera **reconnecté à Azure SQL Database**.

2. Dans la boîte de dialogue connexion, entrez ou sélectionnez le nom du serveur [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

3. Entrez, sélectionnez ou **recherchez** le nom de la base de données.

4. Entrez ou sélectionnez **nom d’utilisateur**.

5. Entrez le **mot de passe**.

6. SSMA recommande la connexion chiffrée à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

7. Cliquez sur **Connecter**.

## <a name="synchronizing-azure-sql-database-metadata"></a>Synchronisation des métadonnées de Azure SQL Database

Les métadonnées relatives aux [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] bases de données ne sont pas automatiquement mises à jour. Les métadonnées dans Azure SQL Database Explorateur de métadonnées sont un instantané des métadonnées lorsque vous vous êtes connecté pour la première fois à Azure SQL Database, ou la dernière fois que vous avez mis à jour manuellement les métadonnées. Vous pouvez mettre à jour manuellement les métadonnées de toutes les bases de données ou d’une base de données ou d’un objet de base de données unique. Pour synchroniser les métadonnées :

1. Assurez-vous que vous êtes connecté à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

2. Dans **Azure SQL Database Explorateur de métadonnées**, activez la case à cocher en regard de la base de données ou du schéma de base de données que vous souhaitez mettre à jour.
   Par exemple, pour mettre à jour les métadonnées de toutes les bases de données, activez la case à cocher en regard de **bases de données**.

3. Cliquez avec le bouton droit sur **bases** de données, ou sur la base de données individuelle ou le schéma de base de données, puis sélectionnez **synchroniser avec la base de** données.

## <a name="next-step"></a>étape suivante

L’étape suivante de la migration dépend des besoins de votre projet :

- Pour personnaliser le mappage entre les schémas Sybase et les [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] bases de données et les schémas, consultez [mappage de schémas Sybase ASE à des schémas de SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql.md).
- Pour personnaliser les options de configuration des projets, consultez [définition des options de projet &#40;SybaseToSQL&#41;](../../ssma/sybase/setting-project-options-sybasetosql.md).
- Pour personnaliser le mappage des types de données sources et cibles, consultez [mappage des types de données Sybase ASE et SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-and-sql-server-data-types-sybasetosql.md).
- Si vous n’avez pas besoin d’effectuer ces tâches, vous pouvez convertir les définitions d’objet de base de données Sybase en [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] définitions d’objets. Pour plus d’informations, consultez [conversion d’objets de base de données Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md).

## <a name="see-also"></a>Voir aussi

[Migration de bases de données Sybase ASE vers SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)
