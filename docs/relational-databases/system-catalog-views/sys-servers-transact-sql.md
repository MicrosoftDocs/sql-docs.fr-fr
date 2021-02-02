---
description: sys.servers (Transact-SQL)
title: sys. Servers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/16/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- servers_TSQL
- sys.servers_TSQL
- servers
- sys.servers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.servers catalog view
ms.assetid: 4e774ed9-4e83-4726-9f1d-8efde8f9feff
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: a37a592866c8fcfdc12e5b84657bc7fa7166ee81
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237892"
---
# <a name="sysservers-transact-sql"></a>sys.servers (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Contient une ligne par serveur lié ou distant inscrit, et une ligne pour le serveur local qui a **server_id** = 0.  

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|ID local du serveur lié.|  
|**name**|**sysname**|Lorsque **server_id** = 0, la valeur retournée est le nom du serveur.<br /><br /> Lorsque **server_id** > 0, la valeur retournée est le nom local du serveur lié.|  
|**product**|**sysname**|Nom de produit du serveur lié. La valeur « SQL Server » indique une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**moteur**|**sysname**|Nom du fournisseur OLE DB permettant de se connecter au serveur lié.<br /><br />À compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] , la valeur « sqlncli » est mappée au [pilote Microsoft OLE DB pour SQL Server (MSOLEDBSQL)](../../connect/oledb/oledb-driver-for-sql-server.md) par défaut. Dans les versions antérieures, la valeur « SQLNCLI » est mappée au [fournisseur de OLE DB SQL Server Native Client (SQLNCLI11)](../../relational-databases/native-client/sql-server-native-client.md).|  
|**data_source**|**nvarchar(4000)**|Propriété de connexion à la source de données OLE DB.|  
|**location**|**nvarchar(4000)**|Propriété de connexion de l'emplacement OLE DB. NULL si aucun.|  
|**provider_string**|**nvarchar(4000)**|Propriété de connexion à la chaîne du fournisseur OLE DB.<br /><br /> A la valeur NULL, sauf si l’appelant dispose de l' `ALTER ANY LINKED SERVER` autorisation.|  
|**catalog**|**sysname**|Propriété de connexion du catalogue OLE DB. NULL si aucun.|  
|**connect_timeout**|**int**|Délai d'expiration de la connexion en secondes, 0 si aucun.|  
|**query_timeout**|**int**|Délai d'expiration de la requête en secondes, 0 si aucun.|  
|**is_linked**|**bit**|0 = est un serveur de style ancien ajouté à l’aide de **sp_addserver**, avec différents comportements de RPC et de transaction distribuée.<br /><br /> 1 = Serveur lié standard.|  
|**is_remote_login_enabled**|**bit**|L'option RPC est active et permet les connexions entrantes à distance pour ce serveur.|  
|**is_rpc_out_enabled**|**bit**|RPC sortant (depuis ce serveur) activé.|  
|**is_data_access_enabled**|**bit**|Les requêtes distribuées sont activées sur ce serveur.|  
|**is_collation_compatible**|**bit**|Le classement des données distantes est supposé être compatible avec les données locales en l'absence d'informations sur le classement.|  
|**uses_remote_collation**|**bit**|Si 1, utiliser le classement indiqué par le serveur distant. Sinon, utiliser le classement spécifié dans la colonne suivante.|  
|**collation_name**|**sysname**|Nom du classement à utiliser ou valeur NULL s'il faut simplement utiliser le classement local.|  
|**lazy_schema_validation**|**bit**|Si 1, la validation de schéma n'est pas activée au démarrage de la requête.|  
|**is_system**|**bit**|Ce serveur est uniquement accessible par le système interne.|  
|**is_publisher**|**bit**|Le serveur est un serveur de publication de réplication.|  
|**is_subscriber**|**bit**|Le serveur est un abonné de réplication.|  
|**is_distributor**|**bit**|Le serveur est un serveur de distribution de réplication.|  
|**is_nonsql_subscriber**|**bit**|Le serveur est un abonné de réplication non-SQL Server.|  
|**is_remote_proc_transaction_promotion_enabled**|**bit**|Si la valeur est 1, l'appel d'une procédure stockée distante démarre une transaction distribuée et enregistre la transaction dans MS DTC. Pour plus d’informations, consultez [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md).|  
|**modify_date**|**datetime**|Date de la dernière modification des informations de serveur.|  
|**is_rda_server**|**bit**|**S’applique à :** À partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] .<br /><br />Le serveur est activé pour l’archivage des données à distance (étendu). Pour plus d’informations, consultez [activer la Stretch Database sur le serveur](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md#EnableTSQLServer).|
  
## <a name="permissions"></a>Autorisations  
 La valeur de **provider_string** est toujours null, sauf si l’appelant dispose de l’autorisation ALTER ANY Linked Server.  
  
 Les autorisations ne sont pas requises pour afficher le serveur local (**server_id** = 0).  
  
 Lorsque vous créez un serveur lié ou distant, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée un mappage de connexion par défaut au rôle serveur **public** . Le mappage de connexion par défaut signifie que toutes les connexions peuvent afficher tous les serveurs liés et distants. Pour restreindre la visibilité de ces serveurs, supprimez le mappage de connexion par défaut en exécutant [sp_droplinkedsrvlogin](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md) et en spécifiant null pour le paramètre *LocalLogin* .  
  
 Si le mappage de connexion par défaut est supprimé, seuls les utilisateurs ayant explicitement été ajoutés en tant que connexion liée ou connexion distante peuvent voir les serveurs liés ou distants pour lesquels ils disposent d'un nom de connexion.  Les autorisations suivantes sont requises pour afficher tous les serveurs liés et distants après le mappage de connexion par défaut :  
  
- `ALTER ANY LINKED SERVER` ou `ALTER ANY LOGIN ON SERVER`  
- Appartenance au rôle de serveur fixe **setupadmin** ou **sysadmin**  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages catalogue des serveurs liés &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addremotelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addremotelogin-transact-sql.md)  
  
