---
description: sp_enumeratependingschemachanges (Transact-SQL)
title: sp_enumeratependingschemachanges (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_enumeratependingschemachanges
- sp_enumeratependingschemachanges_TSQL
helpviewer_keywords:
- sp_enumeratependingschemachanges
ms.assetid: df169b21-d10a-41df-b3a1-654cfb58bc21
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7a75e22f94688d4d8819b22b1b478bd3a3a66ce1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193567"
---
# <a name="sp_enumeratependingschemachanges-transact-sql"></a>sp_enumeratependingschemachanges (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie une liste de toutes les modifications de schéma en attente. Cette procédure stockée peut être utilisée avec [sp_markpendingschemachange](../../relational-databases/system-stored-procedures/sp-markpendingschemachange-transact-sql.md), qui permet à un administrateur d’ignorer les modifications de schéma en attente sélectionnées afin qu’elles ne soient pas répliquées. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_enumeratependingschemachanges [ @publication = ] 'publication'   
    [ , [ @starting_schemaversion = ] starting_schemaversion ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @starting_schemaversion = ] starting_schemaversion` Modification de schéma la plus faible à inclure dans le jeu de résultats.  
  
## <a name="result-set"></a>Jeu de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**article_name**|**sysname**|Nom de l’article auquel la modification de schéma s’applique, ou à l’échelle de la **publication** pour les modifications de schéma qui s’appliquent à la publication entière.|  
|**SchemaVersion**|**int**|Numéro de la modification de schéma en attente.|  
|**schematype**|**sysname**|Valeur de texte représentant le type de modification de schéma.|  
|**schematext**|**nvarchar(max)**|[!INCLUDE[tsql](../../includes/tsql-md.md)] qui décrit la modification de schéma.|  
|**schemastatus**|**nvarchar(10**|Indique si une modification de schéma est en attente pour l'article. Peut avoir l'une des valeurs suivantes :<br /><br /> **actif** = la modification du schéma est en attente<br /><br /> **inactif** = la modification du schéma est inactive<br /><br /> **Skip** = la modification de schéma n’est pas répliquée|  
|**schemaguid**|**uniqueidentifier**|Identifie la modification de schéma.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_enumeratependingschemachanges** est utilisé dans la réplication de fusion.  
  
 **sp_enumeratependingschemachanges**, utilisé avec [sp_markpendingschemachange](../../relational-databases/system-stored-procedures/sp-markpendingschemachange-transact-sql.md), est destiné à la prise en charge de la réplication de fusion et doit être utilisé uniquement lorsque d’autres actions correctives, telles que la réinitialisation, n’ont pas réussi à corriger la situation.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_enumeratependingschemachanges**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sysmergeschemachange &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysmergeschemachange-transact-sql.md)  
  
  
