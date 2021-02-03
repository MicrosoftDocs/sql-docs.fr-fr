---
description: MSSQLSERVER_2020
title: MSSQLSERVER_2020 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2020 (Database Engine error)
ms.assetid: 4a8bf90f-a083-4c53-84f0-d23c711c8081
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 3898acf2b29719b2fa17566c579c1f350d6524cd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196463"
---
# <a name="mssqlserver_2020"></a>MSSQLSERVER_2020
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|2020|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique||  
|Texte du message|Les dépendances signalées pour l'entité "%.*ls" n'incluent pas de références aux colonnes. Cela est dû au fait que l'entité fait référence à un objet inexistant ou que l'une ou plusieurs instructions de l'entité comportent une erreur.  Avant de réexécuter la requête, assurez-vous que l'entité ne comporte aucune erreur et que tous les objets référencés par l'entité existent.|  
  
## <a name="explanation"></a>Explication  
La fonction système **sys.dm_sql_referenced_entities** signale toutes les dépendances au niveau des colonnes pour les références liées au schéma. Par exemple, la fonction signale toutes les dépendances au niveau des colonnes pour une vue indexée car une vue indexée requiert une liaison de schéma. Toutefois, lorsque l'entité référencée n'est pas liée au schéma, les dépendances de colonne sont signalées uniquement lorsque toutes les instructions dans lesquelles les colonnes sont référencées peuvent être liées. Les instructions ne peuvent être correctement liées que si tous les objets existent au moment de l'analyse des instructions. Si la liaison échoue dans une instruction définie dans l’entité, les dépendances de colonnes ne sont pas signalées et la colonne **referenced_minor_id** retourne 0. Lorsque les dépendances de colonne ne peuvent pas être résolues, l'erreur 2020 est générée. Cette erreur n'empêche pas la requête de retourner des dépendances au niveau objet.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Corrigez toutes les erreurs identifiées dans le message avant l'erreur 2020. Par exemple, dans l'exemple de code suivant, la vue `Production.ApprovedDocuments` est définie dans les colonnes `Title`, `ChangeNumber` et `Status` de la table `Production.Document`. La fonction système **sys.dm_sql_referenced_entities** est interrogée pour les objets et les colonnes dont dépend la vue `ApprovedDocuments`. Étant donné que la vue n'est pas créée à l'aide de la clause WITH SCHEMA_BINDING, les colonnes référencées dans la vue peuvent être modifiées dans la table référencée. L'exemple modifie la colonne `ChangeNumber` de la table `Production.Document` en la renommant `TrackingNumber`. L'affichage catalogue est interrogé de nouveau pour la vue `ApprovedDocuments` ; toutefois, il ne peut pas être lié à toutes les colonnes définies dans la vue. Les erreurs 207 et 2020 sont retournées pour identifier le problème. Pour résoudre le problème, il est nécessaire de modifier la vue de façon à refléter le nouveau nom de la colonne.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE VIEW Production.ApprovedDocuments  
AS  
SELECT Title, ChangeNumber, Status  
FROM Production.Document  
WHERE Status = 2;  
GO  
SELECT referenced_schema_name AS schema_name  
,referenced_entity_name AS table_name  
,referenced_minor_name AS referenced_column  
FROM sys.dm_sql_referenced_entities ('Production.ApprovedDocuments', 'OBJECT');  
GO  
EXEC sp_rename 'Production.Document.ChangeNumber', 'TrackingNumber', 'COLUMN';  
GO  
SELECT referenced_schema_name AS schema_name  
,referenced_entity_name AS table_name  
,referenced_minor_name AS referenced_column  
FROM sys.dm_sql_referenced_entities ('Production.ApprovedDocuments', 'OBJECT');  
GO
```
  
La requête retourne les messages d'erreur suivants.  
  
```
Msg 207, Level 16, State 1, Procedure ApprovedDocuments, Line 3  
Invalid column name 'ChangeNumber'.  
Msg 2020, Level 16, State 1, Line 1  
The dependencies reported for entity  
"Production.ApprovedDocuments" do not include references to  
columns. This is either because the entity references an  
object that does not exist or because of an error in one or  
more statements in the entity. Before rerunning the query,  
ensure that there are no errors in the entity and that all  
objects referenced by the entity exist.
```
  
L'exemple suivant corrige le nom de colonne dans la vue.  
  
```sql
USE AdventureWorks2012;  
GO  
ALTER VIEW Production.ApprovedDocuments  
AS  
SELECT Title,TrackingNumber, Status  
FROM Production.Document  
WHERE Status = 2;  
GO
```
  
## <a name="see-also"></a>Voir aussi  
[sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)  
  
