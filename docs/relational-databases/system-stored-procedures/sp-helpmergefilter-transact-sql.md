---
description: sp_helpmergefilter (Transact-SQL)
title: sp_helpmergefilter (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergefilter
- sp_helpmergefilter_TSQL
helpviewer_keywords:
- sp_helpmergefilter
ms.assetid: f133a094-0009-4771-b93b-e86a5c01e40b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 862f23713da4bcd611310149b509e34658766006
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179234"
---
# <a name="sp_helpmergefilter-transact-sql"></a>sp_helpmergefilter (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie des informations sur les filtres de fusion. Cette procédure stockée est exécutée sur n'importe quelle base de données du serveur de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpmergefilter [ @publication= ] 'publication'   
    [ , [ @article= ] 'article']  
    [ , [ @filtername= ] 'filtername']  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @article = ] 'article'` Nom de l’article. *article* est de **type sysname**, avec la valeur par défaut **%** , qui renvoie les noms de tous les articles.  
  
`[ @filtername = ] 'filtername'` Nom du filtre sur lequel les informations doivent être retournées. *FilterName* est de **type sysname**, avec la valeur par défaut **%** , qui retourne des informations sur tous les filtres définis sur l’article ou la publication.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**join_filtreid**|**int**|Identificateur du filtre de jointure.|  
|**FilterName**|**sysname**|Nom du filtre.|  
|**join article name**|**sysname**|Nom de l'article de jointure.|  
|**join_filterclause**|**nvarchar (2000)**|Clause FILTER qualifiant la jointure.|  
|**join_unique_key**|**int**|Indique si la jointure porte sur une clé unique.|  
|**base table owner**|**sysname**|Nom du propriétaire de la table de base.|  
|**base table name**|**sysname**|Nom de la table de base.|  
|**join table owner**|**sysname**|Nom du propriétaire de la table jointe à la table de base.|  
|**join table name**|**sysname**|Nom de la table jointe à la table de base.|  
|**article name**|**sysname**|Nom de l'article de la table jointe à la table de base.|  
|**filter_type**|**tinyint**|Type de filtre de fusion parmi les types suivants :<br /><br /> **1** = filtre de jointure uniquement<br /><br /> **2** = relation d’enregistrement logique<br /><br /> **3** = les deux|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpmergefilter** est utilisé dans la réplication de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** et du rôle de base de données fixe **db_owner** peuvent exécuter **sp_helpmergefilter**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md)   
 [sp_changemergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergefilter-transact-sql.md)   
 [sp_dropmergefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergefilter-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
