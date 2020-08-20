---
description: sp_dropmergepullsubscription (Transact-SQL)
title: sp_dropmergepullsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_dropmergepullsubscription
- sp_dropmergepullsubscription_TSQL
helpviewer_keywords:
- sp_dropmergepullsubscription
ms.assetid: 9301dd80-72f7-4adb-9b13-87e7f9114248
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 1dab181e38d9c072f5e25dc8db490a7538bbe615
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88474252"
---
# <a name="sp_dropmergepullsubscription-transact-sql"></a>sp_dropmergepullsubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime un abonnement de fusion par extraction de données (pull). Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dropmergepullsubscription [ @publication= ] 'publication'   
        , [ @publisher= ] 'publisher'   
        , [ @publisher_db= ] 'publisher_db'   
    [ , [ @reserved= ] 'reserved' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, avec NULL comme valeur par défaut. Ce paramètre est obligatoire. Spécifiez la valeur **All** pour supprimer les abonnements à toutes les publications  
  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher*est de **type sysname**, avec NULL comme valeur par défaut. Ce paramètre est obligatoire.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données du serveur de publication. *publisher_db*est de **type sysname**, avec NULL comme valeur par défaut. Ce paramètre est obligatoire.  
  
`[ @reserved = ] 'reserved'` Est réservé pour une utilisation ultérieure. la valeur *réservée* est de **bit**, avec **0**comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_dropmergepullsubscription** est utilisé dans la réplication de fusion.  
  
 **sp_dropmergepullsubscription** supprime le agent de fusion pour cet abonnement par extraction de fusion, bien que le agent de fusion ne soit pas créé dans **sp_addmergepullsubscription**.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_dropmergepullsubscription](../../relational-databases/replication/codesnippet/tsql/sp-dropmergepullsubscrip_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou de l’utilisateur qui a créé l’abonnement par extraction de fusion peuvent exécuter **sp_dropmergepullsubscription**. Le rôle de base de données fixe **db_owner** peut uniquement exécuter **sp_dropmergepullsubscription** si l’utilisateur qui a créé l’abonnement par extraction de fusion appartient à ce rôle.  
  
## <a name="see-also"></a>Voir aussi  
 [Supprimer un abonnement par extraction](../../relational-databases/replication/delete-a-pull-subscription.md)   
 [sp_addmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_changemergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md)   
 [sp_dropmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md)   
 [sp_helpmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md)  
  
  
