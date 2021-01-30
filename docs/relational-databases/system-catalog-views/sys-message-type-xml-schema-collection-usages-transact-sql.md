---
description: sys.message_type_xml_schema_collection_usages (Transact-SQL)
title: sys.message_type_xml_schema_collection_usages (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- message_type_xml_schema_collection_usages_TSQL
- sys.message_type_xml_schema_collection_usages_TSQL
- sys.message_type_xml_schema_collection_usages
- message_type_xml_schema_collection_usages
dev_langs:
- TSQL
helpviewer_keywords:
- sys.message_type_xml_schema_collection_usages catalog view
ms.assetid: 544f61a1-c7b7-44b4-bf8d-980ba87d0665
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 425d48cf74ead30b156636f75eb7c301a18f259d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191319"
---
# <a name="sysmessage_type_xml_schema_collection_usages-transact-sql"></a>sys.message_type_xml_schema_collection_usages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cet affichage catalogue retourne une ligne pour chaque type de message de service qui est validé par une collection de schémas XML.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**message_type_id**|**int**|ID du type de message de service. Cette colonne n'accepte pas la valeur NULL.|  
|**xml_collection_id**|**int**|ID de la collection contenant l'espace de noms du schéma XML de validation. Cette colonne n'accepte pas la valeur NULL.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
  
