---
description: sys.security_policies (Transact-SQL)
title: sys.security_policies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SYS.SECURITY_POLICIES_TSQL
- SECURITY_POLICIES_TSQL
- SYS.SECURITY_POLICIES
- SECURITY_POLICIES
dev_langs:
- TSQL
helpviewer_keywords:
- sys.security_policies catalog view
- security_policies catalog view
ms.assetid: 35362f5b-e601-4049-9e1d-c5307e823831
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c33e75e803b7b5d9972d373f4cc5738601197860
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182501"
---
# <a name="syssecurity_policies-transact-sql"></a>sys.security_policies (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Retourne une ligne pour chaque stratégie de sécurité de la base de données.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|name|**sysname**|Nom de la stratégie de sécurité, unique dans la base de données.|  
|object_id|**int**|ID de la stratégie de sécurité.|  
|principal_id|**int**|ID du propriétaire de la stratégie de sécurité, tel qu'enregistré dans la base de données. NULL si le propriétaire est déterminé par le schéma.|  
|schema_id|**int**|ID du schéma où réside l'objet.|  
|parent_object_id|**int**|Identificateur de l'objet auquel appartient la stratégie. Doit être égal à 0.|  
|type|**vachar (2)**|Doit être **SP**.|  
|type_desc|**nvarchar(60)**|**SECURITY_POLICY**.|  
|create_date|**datetime**|Date UTC de création de la stratégie de sécurité.|  
|modify_date|**datetime**|Date UTC de dernière modification de la stratégie de sécurité.|  
|is_ms_shipped|**bit**|Toujours false.|  
|is_enabled|**bit**|État de spécification de stratégie de sécurité :<br /><br /> 0 = désactivé<br /><br /> 1 = activé|  
|is_not_for_replication|**bit**|La stratégie a été créée avec l'option NOT FOR REPLICATION.|  
|uses_database_collation|**bit**|Utilise le même classement que la base de données.|  
|is_schemabinding_enabled|**bit**|État de SCHEMABINDING pour la stratégie de sécurité :<br /><br /> 0 ou NULL = activé<br /><br /> 1 = désactivé|  
  
## <a name="permissions"></a>Autorisations  
 Les principaux avec l’autorisation **modifier une stratégie de sécurité** ont accès à tous les objets de cet affichage catalogue, ainsi qu’à toute personne avec la **définition de vue** sur l’objet.  
  
## <a name="see-also"></a>Voir aussi  
 [Sécurité au niveau des lignes](../../relational-databases/security/row-level-security.md)   
 [sys.security_predicates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)   
 [CREATE SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/create-security-policy-transact-sql.md)   
 [Affichages catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
