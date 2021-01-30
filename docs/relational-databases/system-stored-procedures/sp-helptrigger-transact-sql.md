---
description: sp_helptrigger (Transact-SQL)
title: sp_helptrigger (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helptrigger
- sp_helptrigger_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helptrigger
ms.assetid: e486d39b-771d-488d-a786-7136433a2203
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8de5ac41fcd22dc721df5c5fc9c56455910157f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204420"
---
# <a name="sp_helptrigger-transact-sql"></a>sp_helptrigger (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne le ou les types de déclencheurs DML définis sur la table spécifiée de la base de données actuelle. sp_helptrigger ne peut pas être utilisé avec des déclencheurs DDL. Interrogez plutôt l’affichage catalogue des [procédures stockées système](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helptrigger [ @tabname = ] 'table'   
     [ , [ @triggertype = ] 'type' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @tabname = ] 'table'` Nom de la table dans la base de données active pour laquelle les informations de déclencheur doivent être retournées. *table* est de type **nvarchar (776)**, sans valeur par défaut.  
  
`[ @triggertype = ] 'type'` Type du déclencheur DML pour lequel des informations doivent être retournées. le *type* est **char (6)**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**DELETE**|Renvoie des informations sur le déclencheur DELETE.|  
|**INSERT**|Renvoie des informations sur le déclencheur INSERT.|  
|**UPDATE**|Renvoie des informations sur le déclencheur UPDATE.|  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 La table ci-dessous décrit les informations figurant dans le jeu de résultats.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**trigger_name**|**sysname**|Nom du déclencheur.|  
|**trigger_owner**|**sysname**|Nom du propriétaire de la table pour laquelle le déclencheur est défini.|  
|**isupdate**|**int**|1 = déclencheur UPDATE<br /><br /> 0 = n'est pas un déclencheur UPDATE|  
|**isdelete**|**int**|1 = déclencheur DELETE<br /><br /> 0 = n'est pas un déclencheur DELETE|  
|**isinsert**|**int**|1 = déclencheur INSERT<br /><br /> 0 = n'est pas un déclencheur INSERT|  
|**isafter**|**int**|1 = déclencheur AFTER<br /><br /> 0 = n'est pas un déclencheur AFTER|  
|**isinsteadof**|**int**|1 = déclencheur INSTEAD OF<br /><br /> 0 = n'est pas un déclencheur INSTEAD OF|  
|**trigger_schema**|**sysname**|Nom du schéma auquel le déclencheur appartient.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation de configuration de la [visibilité des métadonnées](../../relational-databases/security/metadata-visibility-configuration.md) sur la table.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant exécute `sp_helptrigger` pour produire des informations sur le ou les déclencheurs de la table `Person.Person`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_helptrigger 'Person.Person';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
