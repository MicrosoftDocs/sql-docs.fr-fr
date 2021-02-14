---
description: Enable Stretch Database for a table
title: Enable Stretch Database for a table
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, enabling table
- enabling table for Stretch Database
ms.assetid: de4ac0c5-46ef-4593-a11e-9dd9bcd3ccdc
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 29d4e1d6e2de7c575b9549f13dc325add4f5ca88
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081092"
---
# <a name="enable-stretch-database-for-a-table"></a>Enable Stretch Database for a table
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Pour configurer une table pour Stretch Database, sélectionnez **Stretch | Activer** pour une table dans SQL Server Management Studio afin d’ouvrir l’Assistant **Activer la table pour Stretch** . Vous pouvez également utiliser Transact-SQL pour activer Stretch Database sur une table existante, ou pour créer une table avec Stretch Database.  
  
-   Si vous stockez des données à froid dans une table distincte, vous pouvez migrer la table entière.  
  
-   Si votre table contient des données actuelles et anciennes, vous pouvez spécifier une fonction de filtre pour sélectionner les lignes à migrer.    
 
 **Conditions préalables**. Si vous sélectionnez **Stretch | Activer** pour une table et que vous n’avez pas encore activé Stretch Database pour la base de données, l’Assistant configure d’abord la base de données pour Stretch Database. Suivez les étapes de la [Mise en route en exécutant l’Assistant Activer la base de données pour Stretch](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md) au lieu des étapes décrites dans cet article.  
  
 **Autorisations**. L’activation de Stretch Database sur une table ou une base de données nécessite les autorisations db_owner. L’activation de Stretch Database sur une table nécessite également l’autorisation ALTER sur la table.  

 > [!NOTE]
 > Ultérieurement, n’oubliez pas que la désactivation de Stretch Database pour une table ou une base de données ne supprime pas l’objet distant. Si vous souhaitez supprimer la table distante ou la base de données distante, vous devez la supprimer à l'aide du portail de gestion Azure. Les objets distants continuent d’entraîner des coûts Azure tant qu’ils n’ont pas été supprimés manuellement.
 
##  <a name="use-the-wizard-to-enable-stretch-database-on-a-table"></a><a name="EnableWizardTable"></a> Utilisation de l’assistant pour activer Stretch Database sur une table  
 **Lancer l'Assistant**  
 1.  Dans l’Explorateur d’objets de SQL Server Management Studio, sélectionnez la table pour laquelle vous souhaitez activer Stretch.  
  
2.  Cliquez avec le bouton droit et sélectionnez **Stretch**, puis sélectionnez **Activer** pour lancer l’Assistant.  
  
 **Introduction**  
 Passez en revue l'objectif de l'assistant et les conditions préalables.  
  
 **Sélectionner des tables de base de données**  
 Vérifiez que la table que vous souhaitez activer est affichée et sélectionnée.  
  
 Vous pouvez migrer une table entière ou spécifier une fonction de filtre simple dans l’Assistant. Si vous souhaitez utiliser un autre type de fonction de filtre pour sélectionner les lignes à migrer, effectuez l’une des opérations suivantes.  
  
-   Quittez l’Assistant et exécutez l’instruction ALTER TABLE pour activer Stretch pour la table et pour spécifier une fonction de filtre.  
  
-   Exécutez l’instruction ALTER TABLE pour spécifier une fonction de filtre après avoir quitté l’Assistant. Pour connaître les étapes nécessaires, consultez [Ajouter une fonction de filtre après avoir exécuté l’Assistant](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md#addafterwiz).  
  
 La syntaxe ALTER TABLE est décrite plus loin dans cet article.  
  
 **Résumé**  
 Passez en revue les valeurs que vous avez entrées et les options que vous avez sélectionnées dans l’Assistant. Puis sélectionnez **Terminer** pour activer Stretch.  
  
 **Résultats**  
 Examinez les résultats.  
  
##  <a name="use-transact-sql-to-enable-stretch-database-on-a-table"></a><a name="EnableTSQLTable"></a> Utilisation de Transact-SQL pour activer Stretch Database sur une table  
 Vous pouvez activer Stretch Database pour une table existante ou créer une table avec Stretch Database activée à l'aide de Transact-SQL.  
  
### <a name="options"></a>Options  
 Utilisez les options suivantes lorsque vous exécutez CREATE TABLE ou ALTER TABLE pour activer Stretch Database sur une table.  
  
-   Vous pouvez également utiliser la clause `FILTER_PREDICATE = <function>` pour spécifier une fonction afin de sélectionner les lignes à migrer si la table contient des données chaudes et froides. Le prédicat doit appeler une fonction table inline. Pour plus d’informations, consultez [Sélectionner les lignes à migrer à l’aide d’une fonction de filtre](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md). Si vous ne spécifiez pas de fonction de filtre, la table entière est migrée.  
  
    > [!IMPORTANT]  
    > Si vous fournissez une fonction de filtre qui fonctionne mal, la migration des données fonctionne mal également. Stretch Database applique la fonction de filtre à la table à l’aide de l’opérateur CROSS APPLY.  
  
-   Spécifiez `MIGRATION_STATE = OUTBOUND` pour démarrer immédiatement la migration des données, ou  `MIGRATION_STATE = PAUSED` pour reporter le lancement de la migration des données.  
  
### <a name="enable-stretch-database-for-an-existing-table"></a>Activer Stretch Database pour une table existante  
 Pour configurer une table existante pour Stretch Database, exécutez la commande ALTER TABLE.  
  
 Voici un exemple qui migre la table entière et lance immédiatement la migration des données.  
  
```sql  
USE <Stretch-enabled database name>;
GO
ALTER TABLE <table name>  
    SET ( REMOTE_DATA_ARCHIVE = ON ( MIGRATION_STATE = OUTBOUND ) ) ;  
GO
```  
  
 Voici un exemple qui migre uniquement les lignes identifiées par la fonction table inline `dbo.fn_stretchpredicate` et reporte la migration des données. Pour plus d’informations sur la fonction de filtre, consultez [Sélectionner les lignes à migrer à l’aide d’une fonction de filtre](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).  
  
```sql  
USE <Stretch-enabled database name>;
GO
ALTER TABLE <table name>  
    SET ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(),  
        MIGRATION_STATE = PAUSED ) ) ;  
 GO
```  
  
 Pour plus d’informations, consultez [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
### <a name="create-a-new-table-with-stretch-database-enabled"></a>Créer une table avec Stretch Database activée  
 Pour créer une table avec Stretch Database activée, exécutez la commande CREATE TABLE.  
  
 Voici un exemple qui migre la table entière et lance immédiatement la migration des données.  
  
```sql  
USE <Stretch-enabled database name>;
GO
CREATE TABLE <table name>
    ( ... )  
    WITH ( REMOTE_DATA_ARCHIVE = ON ( MIGRATION_STATE = OUTBOUND ) ) ;  
GO
```  
  
 Voici un exemple qui migre uniquement les lignes identifiées par la fonction table inline `dbo.fn_stretchpredicate` et reporte la migration des données. Pour plus d’informations sur la fonction de filtre, consultez [Sélectionner les lignes à migrer à l’aide d’une fonction de filtre](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).  
  
```sql  
USE <Stretch-enabled database name>;
GO
CREATE TABLE <table name> 
    ( ... )  
    WITH ( REMOTE_DATA_ARCHIVE = ON (  
        FILTER_PREDICATE = dbo.fn_stretchpredicate(),  
        MIGRATION_STATE = PAUSED ) ) ;  
GO  
```  
  
 Pour plus d’informations, consultez [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
  
  
