---
description: sp_setsubscriptionxactseqno (Transact-SQL)
title: sp_setsubscriptionxactseqno (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_setsubscriptionxactseqno
- sp_setsubscriptionxactseqno_TSQL
helpviewer_keywords:
- sp_setsubscriptionxactseqno
ms.assetid: cdb4e0ba-5370-4905-b03f-0b0c6f080ca6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3ec2589b3cacb2e4426793b6adb7814c03e19fce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176411"
---
# <a name="sp_setsubscriptionxactseqno-transact-sql"></a>sp_setsubscriptionxactseqno (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Utilisé lors de la résolution des problèmes pour spécifier la dernière transaction livrée en utilisant le numéro séquentiel dans le journal (LSN), ce qui permet au Agent de distribution de commencer à remettre la transaction suivante. Lors du redémarrage, le Agent de distribution retourne des transactions supérieures à ce filigrane (LSN) à partir du cache de la base de données de distribution (msrepl_commands). Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné. Non pris en charge pour les Abonnés non-SQL Server.  
  
> [!CAUTION]  
>  Si vous n'utilisez pas correctement cette procédure stockée ou si vous spécifiez une valeur de numéro d'enregistrement incorrecte, l'Agent de distribution annule les modifications appliquées au niveau de l'Abonné ou ignore toutes les modifications restantes.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_setsubscriptionxactseqno [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'  
        , [ @xact_seqno = ] xact_seqno   
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, sans valeur par défaut.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données de publication. *publisher_db* est de **type sysname**, sans valeur par défaut. Pour un serveur de publication non SQL Server, *publisher_db* est le nom de la base de données de distribution.  
  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut. Lorsque le Agent de distribution est partagé par plusieurs publications, vous devez spécifier la valeur ALL pour la *publication*.  
  
`[ @xact_seqno = ] xact_seqno` LSN de la transaction suivante sur le serveur de distribution à appliquer à l’abonné. *xact_seqno* est de type **varbinary (16)**, sans valeur par défaut.  
  
## <a name="result-set"></a>Jeu de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**ORIGINAL XACT_SEQNO**|**varbinary(16)**|Numéro séquentiel d'enregistrement d'origine de la transaction suivante à appliquer à l'Abonné.|  
|**UPDATED XACT_SEQNO**|**varbinary(16)**|Numéro séquentiel d'enregistrement mis à jour de la transaction suivante à appliquer à l'Abonné.|  
|**SUBSCRIPTION STREAM COUNT**|**int**|Nombre de flux d'abonnements utilisés au cours de la dernière synchronisation.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_setsubscriptionxactseqno** est utilisé dans la réplication transactionnelle.  
  
 **sp_setsubscriptionxactseqno** ne peut pas être utilisé dans une topologie de réplication transactionnelle d’égal à égal.  
  
 **sp_setsubscriptionxactseqno** peut être utilisé pour ignorer une transaction spécifique qui provoque une erreur lorsque s’applique à l’abonné. En cas de défaillance et après l’arrêt du Agent de distribution, appelez [sp_helpsubscriptionerrors &#40;&#41;Transact-SQL](../../relational-databases/system-stored-procedures/sp-helpsubscriptionerrors-transact-sql.md) sur le serveur de distribution pour récupérer la valeur xact_seqno de la transaction qui a échoué, puis appelez **sp_setsubscriptionxactseqno**, en transmettant cette valeur pour *xact_seqno*. Ainsi, seules les commandes consécutives à ce numéro séquentiel d'enregistrement sont traitées.  
  
 Affectez la valeur **0** à *xact_seqno* pour remettre à l’abonné toutes les commandes en attente dans la base de données de distribution.  
  
 **sp_setsubscriptionxactseqno** peut échouer si le agent de distribution utilise des flux à plusieurs abonnements.  
  
 Lorsque cette erreur se produit, vous devez exécuter l'Agent de distribution avec un seul flux d'abonnements. Pour plus d'informations, consultez [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md).  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_setsubscriptionxactseqno**.  
  
## <a name="see-more"></a>En savoir plus

[Blog : Comment ignorer une transaction](https://repltalk.com/2019/05/28/how-to-skip-a-transaction/)  
