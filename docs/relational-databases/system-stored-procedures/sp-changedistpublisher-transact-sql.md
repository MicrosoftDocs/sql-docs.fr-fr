---
description: sp_changedistpublisher (Transact-SQL)
title: sp_changedistpublisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changedistpublisher_TSQL
- sp_changedistpublisher
helpviewer_keywords:
- sp_changedistpublisher
ms.assetid: 7ef5c89d-faaa-4f8e-aef7-00649ebc8bc9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d48757f7404ba4f915e7b92609c9077ab702adb4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159038"
---
# <a name="sp_changedistpublisher-transact-sql"></a>sp_changedistpublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifie les propriétés du serveur de publication de distribution. Cette procédure stockée est exécutée sur n’importe quelle base de données du serveur de distribution.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changedistpublisher [ @publisher = ] 'publisher'  
    [ , [ @property = ] 'property' ]  
    [ , [ @value = ] 'value' ]  
    [ , [ @storage_connection_string = ] 'storage_connection_string']
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, sans valeur par défaut.  
  
`[ @property = ] 'property'` Propriété à modifier pour le serveur de publication donné. *Property* est de **type sysname** et peut prendre l’une des valeurs suivantes.  
  
`[ @value = ] 'value'` Valeur de la propriété donnée. la *valeur* est de type **nvarchar (255)**, avec NULL comme valeur par défaut.  
  
`[ @storage_connection_string = ] 'storage_connection_string'` Est requis pour SQL Managed Instance, doit correspondre à la clé d’accès du volume de stockage Azure SQL Database. 


 > [!INCLUDE[Azure SQL Database link](../../includes/azure-sql-db-repl-for-more-information.md)]
 
 Le tableau ci-dessous décrit les propriétés des serveurs de publication et les valeurs de ces propriétés.  
  
|Propriété|Valeurs|Description|  
|--------------|------------|-----------------|  
|**active**|**true**|Active le serveur de publication.|  
||**false**|Désactive le serveur de publication.|  
|**bd_distribution**||Nom de la base de données de distribution.|  
|**login**||Nom de connexion.|  
|**mot de passe**||Mot de passe fort pour le nom de connexion fourni.|  
|**security_mode**|**1**|Utiliser l'authentification Windows pour la connexion au serveur de publication. *Cela ne peut pas être modifié pour un non-* [!INCLUDE[msCoName](../../includes/msconame-md.md)] serveur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *publication.*|  
||**0**|Utiliser l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la connexion au serveur de publication. *Cela ne peut pas être modifié pour un non-* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] serveur de *publication.*|  
|**working_directory**||Répertoire de travail utilisé pour stocker les fichiers de données et de schéma de la publication.|  
|NULL (par défaut)||Toutes les options de *propriété* disponibles sont imprimées.| 
|**storage_connection_string**| Clé d’accès | La clé d’accès pour le répertoire de travail lorsque la base de données est Azure SQL Managed Instance. 
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changedistpublisher** est utilisé dans tous les types de réplications.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_changedistpublisher**.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’un serveur de distribution et d’un serveur de publication](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)   
 [sp_helpdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
