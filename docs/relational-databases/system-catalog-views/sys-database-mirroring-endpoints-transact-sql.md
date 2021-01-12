---
description: sys.database_mirroring_endpoints (Transact-SQL)
title: sys.database_mirroring_endpoints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.database_mirroring_endpoints_TSQL
- database_mirroring_endpoints
- sys.database_mirroring_endpoints
- database_mirroring_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database mirroring [SQL Server], endpoint
- HADR [SQL Server], endpoint
- database mirroring [SQL Server], catalog views
- sys.database_mirroring_endpoints catalog view
ms.assetid: f2285199-97ad-473c-a52d-270044dd862b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4e70bd703e258d8605563119e41617985bb0b961
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093199"
---
# <a name="sysdatabase_mirroring_endpoints-transact-sql"></a>sys.database_mirroring_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour le point de terminaison de mise en miroir de bases de données d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Le point de terminaison de mise en miroir de bases de données prend en charge les deux sessions entre les partenaires de mise en miroir de bases de données et les témoins et les sessions entre le réplica principal d’un Always On groupe de disponibilité et ses réplicas secondaires.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|-|Hérite des colonnes des **points de terminaison sys.** (pour plus d’informations, consultez [sys. Endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md)).|  
|**role**|**tinyint**|Rôle de mise en miroir. Peut prendre l'une des valeurs suivantes :<br /><br /> **0** = aucun<br /><br /> **1** = partenaire<br /><br /> **2** = témoin<br /><br /> **3** = tous<br /><br /> Remarque : cette valeur s’applique uniquement à la mise en miroir de bases de données.|  
|**role_desc**|**nvarchar(60)**|Description du rôle de mise en miroir. Peut prendre l'une des valeurs suivantes :<br /><br /> **NONE**<br /><br /> **ASSOCIER**<br /><br /> **FOI**<br /><br /> **ALL**<br /><br /> Remarque : cette valeur s’applique uniquement à la mise en miroir de bases de données.|  
|**is_encryption_enabled**|**bit**|**1** signifie que le chiffrement est activé.<br /><br /> **0** signifie que le chiffrement est désactivé.|  
|**connection_auth**|**tinyint**|Type d'authentification de connexion requis pour les connexions à ce point de terminaison. Peut prendre l'une des valeurs suivantes :<br /><br /> **1** -NTLM<br /><br /> **2** -Kerberos<br /><br /> **3** -négocier<br /><br /> **4** -certificat<br /><br /> **5** -NTLM, certificat<br /><br /> **6** -Kerberos, certificat<br /><br /> **7** -négocier, certificat<br /><br /> **8** -certificat, NTLM<br /><br /> **9** -certificat, Kerberos<br /><br /> **10** -certificat, négocier|  
|**connection_auth_desc**|**Nvarchar (60)**|Description du type d'authentification requis pour les connexions à ce point de terminaison. Peut prendre l'une des valeurs suivantes :<br /><br /> NTLM<br /><br /> KERBEROS<br /><br /> NEGOTIATE<br /><br /> CERTIFICATE<br /><br /> NTLM, CERTIFICATE<br /><br /> KERBEROS, CERTIFICATE<br /><br /> NEGOTIATE, CERTIFICATE<br /><br /> CERTIFICATE, NTLM<br /><br /> CERTIFICATE, KERBEROS<br /><br /> CERTIFICATE, NEGOTIATE|  
|**certificate_id**|**int**|ID du certificat utilisé pour l'authentification, le cas échéant.<br /><br /> 0 = L'authentification Windows et utilisée.|  
|**encryption_algorithm**|**tinyint**|Algorithme de chiffrement. Peut prendre l'une des valeurs suivantes :<br /><br /> **0** -aucun<br /><br /> **1** -RC4<br /><br /> **2** -AES<br /><br /> **3** -aucun, RC4<br /><br /> **4** -aucun, AES<br /><br /> **5** -RC4, AES<br /><br /> **6** -AES, RC4<br /><br /> **7** -aucun, RC4, AES<br /><br /> **8** -aucun, AES, RC4|  
|**encryption_algorithm_desc**|**nvarchar(60)**|Description de l'algorithme de chiffrement. Peut prendre l'une des valeurs suivantes :<br /><br /> NONE<br /><br /> RC4<br /><br /> AES<br /><br /> NONE, RC4<br /><br /> NONE, AES<br /><br /> RC4, AES<br /><br /> AES, RC4<br /><br /> NONE, RC4, AES<br /><br /> NONE, AES, RC4|  
  
## <a name="remarks"></a>Notes  
  
> [!NOTE]  
>  L'algorithme RC4 est uniquement pris en charge pour des raisons de compatibilité descendante. Le nouveau matériel ne peut être chiffré à l'aide de RC4 ou de RC4_128 que lorsque la base de données se trouve dans le niveau de compatibilité 90 ou 100. (Non recommandé.) Utilisez à la place un algorithme plus récent, tel qu'un des algorithmes AES. Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et les versions ultérieures, le matériel chiffré à l’aide de RC4 ou de RC4_128 peut être déchiffré dans n’importe quel niveau de compatibilité.  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Spécifiez l’URL du point de terminaison lors de l’ajout ou de la modification d’un réplica de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)   
 [sys.availability_replicas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [sys.database_mirroring &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md)   
 [sys.database_mirroring_witnesses &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)   
 [Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Questions fréquentes sur l'interrogation des catalogues système de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
