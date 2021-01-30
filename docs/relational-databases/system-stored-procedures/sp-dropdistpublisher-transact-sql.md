---
description: sp_dropdistpublisher (Transact-SQL)
title: sp_dropdistpublisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropdistpublisher
- sp_dropdistpublisher_TSQL
helpviewer_keywords:
- sp_dropdistpublisher
ms.assetid: c0bdd3de-3be0-455c-898a-98d4660e7ce3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 09db4d47afee6795b403542c8442cc9d74724f4b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200693"
---
# <a name="sp_dropdistpublisher-transact-sql"></a>sp_dropdistpublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Supprime un serveur de publication de distribution. Cette procédure stockée est exécutée sur n’importe quelle base de données du serveur de distribution.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dropdistpublisher [ @publisher = ] 'publisher'  
    [ , [ @no_checks = ] no_checks ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Est le serveur de publication à supprimer. *Publisher* est de **type sysname**, sans valeur par défaut.  
  
`[ @no_checks = ] no_checks` Spécifie si **sp_dropdistpublisher** vérifie que le serveur de publication a désinstallé le serveur en tant que serveur de distribution. *no_checks* est de **bit**, avec **0** comme valeur par défaut.  
  
 Si la **valeur est 0**, la réplication vérifie que le serveur de publication distant a désinstallé le serveur local en tant que serveur de distribution. Si le serveur de publication est local, la réplication vérifie qu'il ne reste aucun objet de publication ou de distribution sur le serveur local.  
  
 Si la condition est égale à **1**, tous les objets de réplication associés au serveur de publication de distribution sont supprimés même si un serveur de publication distant est inaccessible. Après cela, le serveur de publication distant doit désinstaller la réplication à l’aide de [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md) avec **\@ ignore_distributor**  =  **1**.  
  
`[ @ignore_distributor = ] ignore_distributor` Spécifie si les objets de distribution sont conservés sur le serveur de distribution lorsque le serveur de publication est supprimé. *ignore_distributor* est de **bits** et peut prendre l’une des valeurs suivantes :  
  
 **1** = les objets de distribution appartenant au serveur de *publication* sont conservés sur le serveur de distribution.  
  
 **0** = les objets de distribution du serveur de *publication* sont nettoyés sur le serveur de distribution.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_dropdistpublisher** est utilisé dans tous les types de réplications.  
  
 Lors de la suppression d’un serveur de publication Oracle, si vous ne parvenez pas à supprimer le serveur de publication **sp_dropdistpublisher** retourne une erreur et les objets du serveur de distribution pour le serveur de publication sont supprimés.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_DropDistPub](../../relational-databases/replication/codesnippet/tsql/sp-dropdistpublisher-tra_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_dropdistpublisher**.  
  
## <a name="see-also"></a>Voir aussi  
 [Désactiver la publication et la distribution](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_changedistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_helpdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
