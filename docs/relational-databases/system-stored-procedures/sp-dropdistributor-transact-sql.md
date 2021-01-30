---
description: sp_dropdistributor (Transact-SQL)
title: sp_dropdistributor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropdistributor
- sp_dropdistributor_TSQL
helpviewer_keywords:
- sp_dropdistributor
ms.assetid: 0644032f-5ff0-4718-8dde-321bc9967a03
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f7a08c3f8cd035db38533f9b7fd56aaa34c39399
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205149"
---
# <a name="sp_dropdistributor-transact-sql"></a>sp_dropdistributor (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Désinstalle le serveur de distribution. Cette procédure stockée est exécutée sur n'importe quelle base de données du serveur de distribution, à l'exception de la base de données de distribution.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dropdistributor [ [ @no_checks= ] no_checks ]   
    [ , [ @ignore_distributor= ] ignore_distributor ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @no_checks = ] no_checks` Indique s’il faut vérifier les objets dépendants avant de supprimer le serveur de distribution. *no_checks* est de **bit**, avec 0 comme valeur par défaut.  
  
 Si la **valeur est 0**, **sp_dropdistributor** s’assure que tous les objets de publication et de distribution en plus du serveur de distribution ont été supprimés.  
  
 Si la valeur est **1**, **sp_dropdistributor** supprime tous les objets de publication et de distribution avant de désinstaller le serveur de distribution.  
  
`[ @ignore_distributor = ] ignore_distributor` Indique si cette procédure stockée est exécutée sans se connecter au serveur de distribution. *ignore_distributor* est de **bit**, avec **0** comme valeur par défaut.  
  
 Si la **valeur est 0**, **sp_dropdistributor** se connecte au serveur de distribution et supprime tous les objets de réplication. Si **sp_dropdistributor** ne parvient pas à se connecter au serveur de distribution, la procédure stockée échoue.  
  
 Si la valeur est **1**, aucune connexion n’est établie avec le serveur de distribution et les objets de réplication ne sont pas supprimés. Cette option est utilisée lors de la désinstallation du serveur de distribution ou lorsque celui-ci est mis hors ligne de façon permanente. Les objets de ce serveur de publication sur le serveur de distribution seront supprimés uniquement lors d'une réinstallation ultérieure du serveur de distribution.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_dropdistributor** est utilisé dans tous les types de réplications.  
  
 Si d’autres objets de publication ou de distribution existent sur le serveur, **sp_dropdistributor** échoue, sauf si **\@ no_checks** a la valeur **1**.  
  
 Cette procédure stockée doit être exécutée après la suppression de la base de données de distribution en exécutant **sp_dropdistributiondb**.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_DropDistPub](../../relational-databases/replication/codesnippet/tsql/sp-dropdistributor-trans_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_dropdistributor**.  
  
## <a name="see-also"></a>Voir aussi  
 [Désactiver la publication et la distribution](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [sp_adddistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md)   
 [sp_changedistributor_property &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistributor-property-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
