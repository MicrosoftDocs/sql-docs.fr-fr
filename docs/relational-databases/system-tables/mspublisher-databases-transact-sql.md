---
description: MSpublisher_databases (Transact-SQL)
title: MSpublisher_databases (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSpublisher_databases
- MSpublisher_databases_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpublisher_databases system table
ms.assetid: 59b0166e-a64c-46b8-befc-c222fa1ccce2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: caba0fa6bf5bee0d00b182a82fc2add0c5061720
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89550972"
---
# <a name="mspublisher_databases-transact-sql"></a>MSpublisher_databases (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSpublisher_databases** contient une ligne pour chaque paire serveur de publication/base de données de publication desservie par le serveur de distribution local. Cette table est stockée dans la base de données de distribution.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|ID du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**id**|**int**|ID de la ligne.|  
|**publisher_engine_edition**|**int**|Édition du serveur de publication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], pouvant prendre l'une des valeurs suivantes :<br /><br /> **10** = édition personnelle<br /><br /> **11** = Desktop Engine (MSDE)<br /><br /> **20** = standard<br /><br /> **21** = groupe de travail<br /><br /> **30** = entreprise (évaluation)<br /><br /> **31** = développeur<br /><br /> **40** = Express (Express ne peut pas être un serveur de publication. Cette valeur est présente par souci d'exhaustivité.)|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
