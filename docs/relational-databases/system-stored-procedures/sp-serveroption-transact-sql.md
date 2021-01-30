---
description: sp_serveroption (Transact-SQL)
title: sp_serveroption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_serveroption_TSQL
- sp_serveroption
dev_langs:
- TSQL
helpviewer_keywords:
- 7343 (Database Engine error)
- sp_serveroption
ms.assetid: 47d04a2b-dbf0-4f15-bd9b-81a2efc48131
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ec6dfda1d4e12b190999b82e16c27b5127d5af6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190778"
---
# <a name="sp_serveroption-transact-sql"></a>sp_serveroption (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit les options de serveur pour les serveurs distants et les serveurs liés.  
  
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_serveroption [@server = ] 'server'   
      ,[@optname = ] 'option_name'       
      ,[@optvalue = ] 'option_value' ;  
```  
  
## <a name="arguments"></a>Arguments  
`[ @server = ] 'server'` Nom du serveur pour lequel l’option doit être définie. *server* est de type **sysname** et n'a pas de valeur par défaut.  
  
`[ @optname = ] 'option_name'` Option à définir pour le serveur spécifié. *option_name* est de type **varchar (** 35 **)**, sans valeur par défaut. *option_name* peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**compatibilité du classement**|Concerne l'exécution des requêtes distribuées sur les serveurs liés. Si cette option a la valeur **true**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suppose que tous les caractères du serveur lié sont compatibles avec le serveur local, en ce qui concerne le jeu de caractères et la séquence de classement (ou ordre de tri). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut alors envoyer au fournisseur des comparaisons sur les colonnes de caractères. Si cette option n'est pas activée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compare toujours les colonnes de caractères en local.<br /><br /> Cette option ne doit être définie que si la source de données correspondant au serveur lié possède le même jeu de caractères et respecte le même ordre de tri que le serveur local.|  
|**nom du classement**|Spécifie le nom du classement utilisé par la source de données distante si **Use Remote collation** a la **valeur true** et que la source de données n’est pas une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] source de données. Le nom doit être l'un des classements pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Utilisez cette option lors d'un accès à une source de données OLE DB autre que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mais dont le classement correspond à l'un des classements de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> Le serveur lié doit prendre en charge un classement unique utilisable pour toutes les colonnes du serveur. Ne définissez pas cette option si le serveur lié prend en charge plusieurs classements dans une source de données unique ou si le classement du serveur lié ne correspond peut-être pas à l'un des classements de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**délai de connexion**|Délai d’attente en secondes pour la connexion à un serveur lié.<br /><br /> Si la valeur est **0**, utilisez la valeur par défaut **sp_configure** .|  
|**accès aux données**|Active ou désactive un serveur lié pour l'accès des requêtes distribuées. Peut être utilisé uniquement pour les entrées **sys. Server** ajoutées par le biais de **sp_addlinkedserver**.|  
|**dist**|Serveur de distribution.|  
|**lazy schema validation**|Détermine si le schéma des tables distantes doit être vérifié.<br /><br /> Si la **valeur est true**, la vérification du schéma des tables distantes est ignorée au début de la requête.|  
|**pub**|Publication.|  
|**délai de requête**|Valeur du délai d'expiration des requêtes par rapport à un serveur lié.<br /><br /> Si la valeur est **0**, utilisez la valeur par défaut **sp_configure** .|  
|**Call**|Active RPC à partir du serveur donné.|  
|**sortie RPC**|Active RPC vers le serveur donné.|  
|**sub**|Côté.|  
|**système**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**utiliser le classement distant**|Détermine si le classement d'une colonne distante ou d'un serveur local doit être utilisé.<br /><br /> Si la **valeur est true**, le classement des colonnes distantes est utilisé pour les sources de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] données, et le classement spécifié dans **collation Name** est utilisé pour les sources de données non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> Si la **valeur est false**, les requêtes distribuées utilisent toujours le classement par défaut du serveur local, tandis que le **nom de classement** et le classement des colonnes distantes sont ignorés. La valeur par défaut est **false**. (La valeur **false** est compatible avec la sémantique de classement utilisée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7,0.)|  
|**remote proc transaction promotion**|Cette option permet de protéger les actions d'une procédure de serveur à serveur par le biais d'une transaction MS DTC ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator). Lorsque cette option a la valeur TRUE (ou ON), l'appel d'une procédure stockée distante démarre une transaction distribuée et inscrit la transaction avec MS DTC. L'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appelant la procédure stockée distante constitue l'élément créateur de la transaction et qui contrôle l'exécution jusqu'à son terme. Si une instruction COMMIT TRANSACTION ou ROLLBACK TRANSACTION est ensuite émise pour la connexion, le serveur de contrôle demande à MS DTC de gérer l'achèvement de la transaction distribuée sur tous les ordinateurs concernés.<br /><br /> Une fois qu'une transaction distribuée [!INCLUDE[tsql](../../includes/tsql-md.md)] a démarré, des appels de procédures stockées distantes peuvent être effectués envers d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ont été définies en tant que serveurs liés. Les serveurs liés sont tous inscrits dans la transaction distribuée [!INCLUDE[tsql](../../includes/tsql-md.md)] et MS DTC garantit que la transaction est exécutée jusqu'à son terme sur chaque serveur lié.<br /><br /> Si cette option a la valeur FALSE (ou OFF), une transaction locale ne sera pas promue en transaction distribuée lors de l'exécution d'un appel de procédure distante sur un serveur lié.<br /><br /> Si, avant d'exécuter un appel de procédure de serveur à serveur, la transaction est déjà une transaction distribuée, cette option n'a pas effet. L'appel de procédure sur le serveur lié s'exécutera sous la même transaction distribuée.<br /><br /> Si, avant d'exécuter un appel de procédure de serveur à serveur, aucune transaction n'est active dans la connexion, cette option n'a pas effet. La procédure s'exécute ensuite sur le serveur lié sans transactions actives.<br /><br /> La valeur par défaut de cette option est TRUE (ou ON).|  
  
`[ @optvalue = ] 'option_value'` Spécifie si le *option_name* doit être activé (**true** ou **on**) ou désactivé (**false** ou **off**). *option_value* est de type **varchar (** 10 **)**, sans valeur par défaut.  
  
 *option_value* peut être un entier non négatif pour les options **délai de connexion** et **délai d’expiration** de la requête. Pour l’option **nom du classement** , *option_value* peut être un nom de classement ou une valeur null.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Si l’option **collation compatible** est définie sur true, le **nom de classement** sera automatiquement défini sur null. Si le **nom du classement** est défini sur une valeur non null, le **classement compatible** est défini automatiquement sur false.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite une autorisation ALTER ANY LINKED SERVER sur le serveur.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant configure un serveur lié correspondant à une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], `SEATTLE3`, , de façon à ce qu'il soit compatible avec le classement de l'instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
USE master;  
EXEC sp_serveroption 'SEATTLE3', 'collation compatible', 'true';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de requêtes distribuées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_dropdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
