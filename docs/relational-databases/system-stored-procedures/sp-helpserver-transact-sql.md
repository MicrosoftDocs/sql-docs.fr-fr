---
description: sp_helpserver (Transact-SQL)
title: sp_helpserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpserver
- sp_helpserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpserver
ms.assetid: e8f42de7-c738-41c3-8bf5-dbd559dc7184
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 83d212a6cfb817f3aa28c87ab0999d0488cec5e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211914"
---
# <a name="sp_helpserver-transact-sql"></a>sp_helpserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fournit des informations sur un serveur distant, sur un serveur de réplication particulier ou sur tous les serveurs des deux types. Fournit le nom du serveur, le nom réseau du serveur, l'état de réplication du serveur, le numéro d'identification du serveur et le nom du classement. Fournit également les valeurs des délais d'expiration pour les connexions ou les requêtes des serveurs liés.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpserver [ [ @server = ] 'server' ]   
  [ , [ @optname = ] 'option' ]   
  [ , [ @show_topology = ] 'show_topology' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @server = ] 'server'` Est le serveur sur lequel les informations sont signalées. Si le *serveur* n’est pas spécifié, génère des rapports sur tous les serveurs de **master.sys. serveurs**. *Server* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @optname = ] 'option'` Est l’option qui décrit le serveur. l' *option* est de type **varchar (** 35 **)**, avec NULL comme valeur par défaut et doit prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**compatibilité du classement**|Affecte l'exécution des requêtes distribuées sur les serveurs liés. Si cette option a la valeur true,|  
|**accès aux données**|Active ou désactive un serveur lié pour l'accès des requêtes distribuées.|  
|**dist**|Serveur de distribution.|  
|**dpub**|Serveur de publication distant de ce serveur de distribution.|  
|**lazy schema validation**|La vérification du schéma des tables distantes est ignorée au début de la requête.|  
|**pub**|Publication.|  
|**Call**|Active l'appel de procédure à distance (RPC) à partir du serveur spécifié.|  
|**sortie RPC**|Active l'appel de procédure à distance (RPC) à destination du serveur spécifié.|  
|**sub**|Côté.|  
|**système**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**utiliser le classement distant**|Le classement d'une colonne distante est utilisé à la place de celui du serveur local.|  
  
`[ @show_topology = ] 'show_topology'` Relation entre le serveur spécifié et d’autres serveurs. *show_topology* est de type **varchar (** 1 **)**, avec NULL comme valeur par défaut. Si *show_topology* n’est pas égal à **t** ou a la valeur null, **sp_helpserver** retourne les colonnes listées dans la section ensembles de résultats. Si *show_topology* est égal à **t**, en plus des colonnes listées dans les jeux de résultats, **sp_helpserver** retourne également les informations **topx** et **Topy** .  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou 1 (échec).  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom du serveur.|  
|**network_name**|**sysname**|Nom réseau du serveur.|  
|**statut**|**varchar (** 70 **)**|État du serveur.|  
|**id**|**char (** 4 **)**|Numéro d'identification du serveur.|  
|**collation_name**|**sysname**|Classement du serveur.|  
|**connect_timeout**|**int**|Valeur du délai d'expiration de la connexion au serveur lié.|  
|**query_timeout**|**int**|Valeur du délai d'expiration des requêtes sur le serveur lié.|  
  
## <a name="remarks"></a>Notes  
 Un même serveur peut avoir plusieurs états.  
  
## <a name="permissions"></a>Autorisations  
 Aucune autorisation n’est vérifiée.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-displaying-information-about-all-servers"></a>R. Affichage d’informations sur tous les serveurs  
 L'exemple suivant affiche des informations sur tous les serveurs en utilisant la procédure `sp_helpserver` sans paramètre.  
  
```  
USE master;  
GO  
EXEC sp_helpserver;  
```  
  
### <a name="b-displaying-information-about-a-specific-server"></a>B. Affichage d'informations sur un serveur spécifique  
 Cet exemple affiche toutes les informations sur le serveur `SEATTLE2`.  
  
```  
USE master;  
GO  
EXEC sp_helpserver 'SEATTLE2';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_addserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_addsubscriber &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscriber-transact-sql.md)   
 [sp_changesubscriber &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubscriber-transact-sql.md)   
 [sp_dropserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropserver-transact-sql.md)   
 [sp_dropsubscriber &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscriber-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [sp_helpremotelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpremotelogin-transact-sql.md)   
 [sp_helpsubscriberinfo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md)   
 [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
