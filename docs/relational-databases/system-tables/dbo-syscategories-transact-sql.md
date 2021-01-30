---
description: dbo.syscategories (Transact-SQL)
title: Catégories de dbo.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.syscategories_TSQL
- syscategories
- syscategories_TSQL
- dbo.syscategories
dev_langs:
- TSQL
helpviewer_keywords:
- syscategories system table
ms.assetid: eb2cb75c-dc58-4a5b-b329-664e9fe20ce0
author: cawrites
ms.author: chadam
ms.openlocfilehash: c1d7bf545605d8ebbf34f45e1b8d3b9a45e89c2f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211290"
---
# <a name="dbosyscategories-transact-sql"></a>dbo.syscategories (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient les catégories utilisées par [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour organiser les travaux, les alertes et les opérateurs. Cette table est stockée dans la base de données **msdb** .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**category_id**|**int**|Identifiant de la catégorie|  
|**category_class**|**int**|Type d'élément dans la catégorie :<br /><br /> **1** = travail<br /><br /> **2** = alerte<br /><br /> **3** = (opérateur)|  
|**category_type**|**tinyint**|Type de catégorie :<br /><br /> **1** = local<br /><br /> **2** = multiserveur<br /><br /> **3** = aucun|  
|**name**|**sysname**|Nom de la catégorie|  
  
  
