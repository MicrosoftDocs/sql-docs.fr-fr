---
description: sp_deletetracertokenhistory (Transact-SQL)
title: sp_deletetracertokenhistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_deletetracertokenhistory
- sp_deletetracertokenhistory_TSQL
helpviewer_keywords:
- sp_deletetracertokenhistory
ms.assetid: 9ae1be14-0d2f-40b1-9d6e-22d79726abf4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5bc5a6d5191f89d8b31b48939c83674bf0302e15
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178160"
---
# <a name="sp_deletetracertokenhistory-transact-sql"></a>sp_deletetracertokenhistory (Transact-SQL)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Supprime les enregistrements de jeton de suivi du [MStracer_tokens &#40;Transact-sql&#41;](../../relational-databases/system-tables/mstracer-tokens-transact-sql.md) et MStracer_history &#40;tables système&#41;[Transact-SQL ](../../relational-databases/system-tables/mstracer-history-transact-sql.md) . Cette procédure stockée est exécutée sur la base de données de publication du serveur de publication ou sur la base de données de distribution du serveur de distribution.

![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```
sp_deletetracertokenhistory [ @publication = ] 'publication'
    [ , [ @tracer_id = ] tracer_id ]
    [ , [ @cutoff_date = ] cutoff_date ]
    [ , [ @publisher = ] 'publisher' ]
    [ , [ @publisher_db = ] 'publisher_db' ]
```

## <a name="arguments"></a>Arguments

`@publication= 'publication'`  
Nom de la publication dans laquelle le jeton de suivi a été inséré. Le type de données est **sysname**. Ce paramètre est obligatoire.

`[ @tracer_id= ] tracer_id`  
ID du jeton de suivi à supprimer. Le type de données est **int**. La valeur par défaut est *null*. Si la *valeur est null*, tous les jetons de suivi appartenant à la publication sont supprimés.

`[ @cutoff_date= ] cutoff_date`  
Les jetons de suivi insérés dans la publication avant cette date sont supprimés. Le type de données est **DateTime**. La valeur par défaut est *null*.

`[ @publisher= ] 'publisher'`  
Nom du serveur de publication. Le type de données est **sysname**. La valeur par défaut est *null*.

> [!NOTE]
> Ce paramètre doit être spécifié uniquement pour les serveurs de publication non- [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou lors de l’exécution de la procédure stockée à partir du serveur de distribution.

`[ @publisher_db= ] 'publisher_db'`  
Nom de la base de données de publication. Le type de données est **sysname**. La valeur par défaut est NULL. Ce paramètre est ignoré si la procédure stockée est exécutée sur le serveur de publication.

> [!NOTE]
> Ce paramètre doit être spécifié lors de l’exécution de la procédure stockée à partir du serveur de distribution.

## <a name="return-code-values"></a>Codet de retour

**0** (succès) ou **1** (échec)

## <a name="remarks"></a>Notes

**sp_deletetracertokenhistory** est utilisé dans la réplication transactionnelle.  

Une erreur se produit si vous spécifiez les paramètres *tracer_id* et *cutoff_date*.

Si vous n’exécutez pas **sp_deletetracertokenhistory** pour supprimer les métadonnées de jeton de suivi, elles seront supprimées lors du nettoyage régulier de l’historique.

Les ID de jeton de suivi peuvent être déterminés en exécutant [sp_helptracertokens &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md) ou en interrogeant le [MStracer_tokens &#40;table système&#41;Transact-SQL ](../../relational-databases/system-tables/mstracer-tokens-transact-sql.md) .

## <a name="permissions"></a>Autorisations

Seul le personnel suivant est habilité à exécuter **sp_deletetracertokenhistory**:

- Membres des rôles **replmonitor** , dans la base de données de distribution
- Membres du rôle serveur fixe **sysadmin** .
- Membres du rôle de base de données fixe **db_owner** , dans la base de données de publication.
- **Db_owner** de la base de données fixe.

## <a name="see-also"></a>Voir aussi

[Mesurer la latence et valider les connexions pour la réplication transactionnelle](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)

[sp_helptracertokenhistory &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptracertokenhistory-transact-sql.md)
