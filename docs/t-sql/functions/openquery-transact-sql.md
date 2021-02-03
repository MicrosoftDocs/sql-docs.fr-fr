---
description: OPENQUERY (Transact-SQL)
title: OPENQUERY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPENQUERY_TSQL
- OPENQUERY
dev_langs:
- TSQL
helpviewer_keywords:
- DELETE statement [SQL Server], OPENQUERY function
- OPENQUERY function
- FROM clause, OPENQUERY function
- UPDATE statement [SQL Server], OPENQUERY function
- pass-through queries [SQL Server]
- INSERT statement [SQL Server], OPENQUERY function
ms.assetid: b805e976-f025-4be1-bcb0-3a57b0c57717
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: d7af8ffa6f8fe26bf31c699dece7f24e7f6c32fe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180208"
---
# <a name="openquery-transact-sql"></a>OPENQUERY (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Exécute la requête directe spécifiée sur le serveur lié spécifié. Ce serveur est une source de données OLE DB. Il est possible de référencer OPENQUERY dans la clause FROM d’une requête SELECT comme s’il s’agissait du nom d’une table. Il est également possible de référencer OPENQUERY comme la table cible d'une instruction INSERT, UPDATE ou DELETE. Cela dépend des possibilités du fournisseur OLE DB. Bien que la requête puisse renvoyer plusieurs jeux de résultats, OPENQUERY ne renvoie que le premier.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
OPENQUERY ( linked_server ,'query' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *linked_server*  
 ID représentant le nom du serveur lié.  
  
 **'** *query* **'**  
 Chaîne de requête exécutée dans le serveur lié. La longueur maximale de la chaîne est limitée à 8 Ko.  
  
## <a name="remarks"></a>Remarques  
 OPENQUERY n'accepte pas de variables pour ses arguments.  
  
 Vous ne pouvez pas utiliser OPENQUERY pour exécuter des procédures stockées étendues sur un serveur lié. Par contre, une procédure stockée étendue peut être exécutée sur un serveur lié en utilisant un nom en quatre parties. Par exemple :  
  
```sql  
EXEC SeattleSales.master.dbo.xp_msver  
```  
  
 Tout appel à OPENDATASOURCE, OPENQUERY ou OPENROWSET dans la clause FROM est évalué séparément et indépendamment de tout appel à ces fonctions utilisé comme cible de la mise à jour, même si des arguments identiques sont fournis aux deux appels. En particulier, les conditions de filtre ou de jointure appliquées sur le résultat de l'un de ces appels n'ont aucun effet sur les résultats de l'autre.  
  
## <a name="permissions"></a>Autorisations  
 Tous les utilisateurs peuvent exécuter OPENQUERY. Les autorisations utilisées pour la connexion au serveur distant sont obtenues à partir des paramètres définis pour le serveur lié.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-executing-an-update-pass-through-query"></a>R. Exécution d'une requête UPDATE directe  
 L'exemple suivant exécute une requête `UPDATE` directe sur le serveur lié créé dans l'exemple A.  
  
```sql  
UPDATE OPENQUERY (OracleSvr, 'SELECT name FROM joe.titles WHERE id = 101')   
SET name = 'ADifferentName';  
```  
  
### <a name="b-executing-an-insert-pass-through-query"></a>B. Exécution d'une requête INSERT directe  
 L'exemple suivant exécute une requête `INSERT` directe sur le serveur lié créé dans l'exemple A.  
  
```sql  
INSERT OPENQUERY (OracleSvr, 'SELECT name FROM joe.titles')  
VALUES ('NewTitle');  
```  
  
### <a name="c-executing-a-delete-pass-through-query"></a>C. Exécution d'une requête DELETE directe  
 L'exemple suivant exécute une requête `DELETE` directe pour supprimer la ligne insérée dans l'exemple C.  
  
```sql  
DELETE OPENQUERY (OracleSvr, 'SELECT name FROM joe.titles WHERE name = ''NewTitle''');  
```  
  
### <a name="d-executing-a-select-pass-through-query"></a>D. Exécution d'une requête SELECT directe  
 L’exemple suivant exécute une requête directe `SELECT` pour sélectionner la ligne insérée dans l’exemple C.  
  
```sql  
SELECT * FROM OPENQUERY (OracleSvr, 'SELECT name FROM joe.titles WHERE name = ''NewTitle''');  
```  
    
## <a name="see-also"></a>Voir aussi  
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [OPENDATASOURCE &#40;Transact-SQL&#41;](../../t-sql/functions/opendatasource-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  
