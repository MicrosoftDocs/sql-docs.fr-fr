---
description: sys.service_broker_endpoints (Transact-SQL)
title: sys.service_broker_endpoints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.service_broker_endpoints_TSQL
- service_broker_endpoints
- service_broker_endpoints_TSQL
- sys.service_broker_endpoints
dev_langs:
- TSQL
helpviewer_keywords:
- sys.service_broker_endpoints catalog view
ms.assetid: 6979ec9b-0043-411e-aafb-0226fa26c5ba
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d8e8a6590dedf9c43da14147c503d1e62423e595
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096687"
---
# <a name="sysservice_broker_endpoints-transact-sql"></a>sys.service_broker_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cet affichage catalogue contient une ligne pour le point de terminaison Service Broker. Pour chaque ligne de cette vue, il existe une ligne correspondante avec le même **endpoint_id** dans la vue **sys.tcp_endpoints** qui contient les métadonnées de configuration TCP. TCP est le seul protocole autorisé pour Service Broker.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|**--**|Hérite des colonnes des [points de terminaison sys. endpoint &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md).|  
|**is_message_forwarding_enabled**|**bit**|Indique si le point de terminaison prend en charge la transmission de message. La valeur initiale est **0** (désactivé). Cette colonne n'accepte pas la valeur NULL.|  
|**message_forwarding_size**|**int**|Le nombre maximal de mégaoctets de l’espace de **tempdb** autorisé à être utilisé pour les messages transférés. La valeur initiale est **10**. Cette colonne n'accepte pas la valeur NULL.|  
|**connection_auth**|**tinyint**|Type d'authentification de connexion requis pour les connexions à ce point de terminaison. Peut prendre l'une des valeurs suivantes :<br /><br /> **1** -NTLM<br /><br /> **2** -Kerberos<br /><br /> **3** -négocier<br /><br /> **4** -certificat<br /><br /> **5** -NTLM, certificat<br /><br /> **6** -Kerberos, certificat<br /><br /> **7** -négocier, certificat<br /><br /> **8** -certificat, NTLM<br /><br /> **9** -certificat, Kerberos<br /><br /> **10** -certificat, négocier<br /><br /> Cette colonne n'accepte pas la valeur NULL.|  
|**connection_auth_desc**|**nvarchar(60)**|Description du type d'authentification de connexion requis pour les connexions à ce point de terminaison :<br /><br /> NTLM<br /><br /> KERBEROS<br /><br /> NEGOTIATE<br /><br /> CERTIFICATE<br /><br /> NTLM, CERTIFICATE<br /><br /> KERBEROS, CERTIFICATE<br /><br /> NEGOTIATE, CERTIFICATE<br /><br /> CERTIFICATE, NTLM<br /><br /> CERTIFICATE, KERBEROS<br /><br /> CERTIFICATE, NEGOTIATE<br /><br /> Accepte la valeur NULL.|  
|**certificate_id**|**int**|ID du certificat utilisé pour l'authentification, le cas échéant.<br /><br /> 0 = L'authentification Windows et utilisée.|  
|**encryption_algorithm**|**tinyint**|Algorithme de chiffrement. Voici les valeurs possibles avec leurs descriptions et les options DDL correspondantes.<br /><br /> **0** : aucun. Option DDL correspondante : désactivée.<br /><br /> **1** : RC4. Option DDL correspondante : {required &#124; algorithme RC4}.<br /><br /> **2** : AES. Option DDL correspondante : algorithme AES obligatoire.<br /><br /> **3** : aucun, RC4. Option DDL correspondante : {algorithme pris en charge &#124; algorithme RC4} pris en charge.<br /><br /> **4** : aucun, AES. Option DDL correspondante : algorithme AES pris en charge.<br /><br /> **5** : RC4, AES. Option DDL correspondante : algorithme obligatoire RC4 AES.<br /><br /> **6** : AES, RC4. Option DDL correspondante : algorithme obligatoire AES RC4.<br /><br /> **7** : aucun, RC4, AES. Option DDL correspondante : algorithme RC4 AES pris en charge.<br /><br /> **8** : aucun, AES, RC4. Option DDL correspondante : algorithme AES RC4 pris en charge.<br /><br /> Cette colonne n'accepte pas la valeur NULL.|  
|**encryption_algorithm_desc**|**nvarchar(60)**|Description de l’algorithme de chiffrement. Les valeurs possibles et les options DDL correspondantes sont répertoriées ci-dessous :<br /><br /> AUCUN : désactivé<br /><br /> RC4 : {required &#124; algorithme RC4}<br /><br /> AES : algorithme obligatoire AES<br /><br /> AUCUN, RC4 : {pris en charge &#124; algorithme RC4}<br /><br /> AUCUN, AES : algorithme AES pris en charge<br /><br /> RC4, AES : algorithme obligatoire RC4 AES<br /><br /> AES, RC4 : algorithme obligatoire AES RC4<br /><br /> AUCUN, RC4, AES : algorithme RC4 AES pris en charge<br /><br /> NONE, AES, RC4 : algorithme AES RC4 pris en charge<br /><br /> Accepte la valeur NULL.|  
  
## <a name="remarks"></a>Notes  
  
> [!NOTE]  
>  L'algorithme RC4 est uniquement pris en charge pour des raisons de compatibilité descendante. Le nouveau matériel ne peut être chiffré à l'aide de RC4 ou de RC4_128 que lorsque la base de données se trouve dans le niveau de compatibilité 90 ou 100. (Non recommandé.) Utilisez à la place un algorithme plus récent, tel qu'un des algorithmes AES. Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures, le matériel chiffré à l’aide de RC4 ou de RC4_128 peut être déchiffré dans n’importe quel niveau de compatibilité.  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)  
  
  
