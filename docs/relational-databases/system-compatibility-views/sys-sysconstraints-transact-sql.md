---
description: sys.sysconstraints (Transact-SQL)
title: Contraintes de sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysconstraints
- sys.sysconstraints
- sysconstraints_TSQL
- sys.sysconstraints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysconstraints compatibility view
- sysconstraints system table
ms.assetid: f2b2e2ad-ba24-48a1-913c-8ee4e0895dc4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3014430aad20c74d22a72e1756503baadbe7cb68
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099129"
---
# <a name="syssysconstraints-transact-sql"></a>sys.sysconstraints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient le mappage des contraintes vers les objets détenant ces contraintes dans la base de données.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**constid**|**int**|Numéro de contrainte.|  
|**id**|**int**|Identificateur de la table qui détient la contrainte.|  
|**colid**|**smallint**|ID de la colonne sur laquelle la contrainte est définie.<br /><br /> 0 = Contrainte de niveau table|  
|**spare1**|**tinyint**|Réservé|  
|**statut**|**int**|Pseudo-masque de bits indiquant l'état. Il peut prendre les valeurs suivantes :<br /><br /> 1 = Contrainte PRIMARY KEY<br /><br /> 2 = Contrainte UNIQUE KEY<br /><br /> 3 = Contrainte FOREIGN KEY<br /><br /> 4 = Contrainte CHECK<br /><br /> 5 = Contrainte DEFAULT<br /><br /> 16 = Contrainte de niveau colonne<br /><br /> 32 = Contrainte de niveau table|  
|**actions**|**int**|Réservé|  
|**error**|**int**|Réservé|  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage de tables système à des vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
