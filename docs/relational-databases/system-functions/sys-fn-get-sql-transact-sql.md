---
description: sys.fn_get_sql (Transact-SQL)
title: sys.fn_get_sql (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_get_sql
- sys.fn_get_sql_TSQL
- fn_get_sql_TSQL
- sys.fn_get_sql
dev_langs:
- TSQL
helpviewer_keywords:
- fn_get_sql function
- text [SQL Server], SQL handles
- sys.fn_get_sql function
- valid SQL handles [SQL Server]
- SQL handles
ms.assetid: d5fe49b5-0813-48f2-9efb-9187716b2fd4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4b3e28e4c66d45f28c6239431e8e6d5440d5d4a0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093819"
---
# <a name="sysfn_get_sql-transact-sql"></a>sys.fn_get_sql (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie le texte de l'instruction SQL pour le handle SQL spécifié.  
  
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une prochaine version de Microsoft SQL Server. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Utilisez sys.dm_exec_sql_text à la place. Pour plus d’informations, consultez [sys.dm_exec_sql_text &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
 
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.fn_get_sql ( SqlHandle )  
```  
  
## <a name="arguments"></a>Arguments  
 *SqlHandle*  
 Valeur du handle. *Sqlhandle* est de type **varbinary (64)** et n’a pas de valeur par défaut.  
  
## <a name="tables-returned"></a>Tables retournées  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|dbid|**smallint**|ID de la base de données. Pour les instructions SQL ad hoc et préparées, l'ID de la base de données où les instructions ont été compilées.|  
|objectid|**int**|Identificateur de l'objet de base de données. NULL pour les instructions SQL ad hoc.|  
|nombre|**smallint**|Indique le numéro du groupe, si les procédures sont groupées.<br /><br /> 0 = les entrées ne sont pas des procédures.<br /><br /> NULL = instructions SQL ad hoc.|  
|encrypted|**bit**|Indique si l'objet est chiffré.<br /><br /> 0 = Non chiffrée<br /><br /> 1 = Chiffrée|  
|texte|**text**|Texte de l'instruction SQL. NULL pour les objets chiffrés.|  
  
## <a name="remarks"></a>Notes  
 Vous pouvez obtenir un handle SQL valide à partir de la colonne sql_handle de la sys.dm_exec_requests &#40;la vue de gestion dynamique [&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md) .  
  
 Si vous transmettez un handle qui n’existe plus dans le cache, fn_get_sq **l** retourne un jeu de résultats vide. Si vous passez un handle qui n'est pas valide, le lot s'arrête et un message d'erreur s'affiche.  
  
 Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ne peut pas mettre en cache certaines [!INCLUDE[tsql](../../includes/tsql-md.md)] instructions, telles que les instructions de copie en bloc et les instructions avec des littéraux de chaîne dont la taille est supérieure à 8 Ko. Les handles de ces instructions ne peuvent pas être récupérés avec fn_get_sql.  
  
 La colonne **Text** du jeu de résultats est filtrée pour le texte qui peut contenir des mots de passe. Pour plus d’informations sur les procédures stockées liées à la sécurité qui ne sont pas surveillées, consultez [Filtrer une trace](../../relational-databases/sql-trace/filter-a-trace.md).  
  
 La fonction fn_get_sql retourne des informations similaires à la commande [DBCC INPUTBUFFER](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md) . Les exemples suivants montrent quand vous pouvez utiliser la fonction fn_get_sql parce que DBCC INPUTBUFFER ne peut pas l'être :  
  
-   lorsque des événements contiennent plus de 255 caractères ;  
  
-   lorsque vous devez renvoyer le niveau d'imbrication actuel le plus élevé d'une procédure stockée. Imaginons par exemple deux procédures stockées appelées sp_1 et sp_2. Si sp_1 appelle sp_2 et que vous obtenez le handle de la vue de gestion dynamique sys.dm_exec_requests alors que sp_2 est en cours d'exécution, la fonction fn_get_sql renvoie des informations sur sp_2. En outre, la fonction fn_get_sql renvoie le texte complet de la procédure stockée au niveau d'imbrication actuel le plus élevé.  
  
## <a name="permissions"></a>Autorisations  
 L’utilisateur doit disposer de l’autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="examples"></a>Exemples  
 Les administrateurs de base de données peuvent utiliser la fonction fn_get_sql, comme indiqué dans l'exemple suivant pour diagnostiquer les problèmes de traitement. Lorsque l'administrateur identifie un ID de session problématique, il peut récupérer le handle SQL de cette session, appeler fn_get_sql avec le handle, puis utiliser les décalages de début et de fin pour déterminer le texte SQL de l'ID de session problématique.  
  
```  
DECLARE @Handle varbinary(64);  
SELECT @Handle = sql_handle   
FROM sys.dm_exec_requests   
WHERE session_id = 52 and request_id = 0;  
SELECT * FROM sys.fn_get_sql(@Handle);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DBCC INPUTBUFFER &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md)   
 [ Processus desys.sys&#40;&#41;Transact-SQL ](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
  
