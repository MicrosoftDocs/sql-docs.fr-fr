---
description: Fonctionnalités déconseillées dans la réplication SQL Server
title: Fonctionnalités dépréciées dans la réplication SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 01/22/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- deprecated features [SQL Server replication]
ms.assetid: 46bd3edd-d6de-40a6-a015-21cce8321feb
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 10306a2d4cc8f447706581365d31617ac2a29220
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88498772"
---
# <a name="deprecated-features-in-sql-server-replication"></a>Fonctionnalités déconseillées dans la réplication SQL Server
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Cette rubrique décrit les fonctionnalités de réplication déconseillées qui sont toujours disponibles dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Il est prévu que ces fonctionnalités soient supprimées dans une prochaine version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les fonctions déconseillées ne doivent pas être utilisées dans de nouvelles applications.  
  
## <a name="items-deprecated-in-sssql15"></a>Éléments déconseillés dans [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]  
  
|Fonctionnalité|Description|  
|-------------|-----------------|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|La réplication est prise en charge si chaque point de terminaison [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se trouve dans deux versions principales de la version actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par conséquent, [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] ne prend pas en charge la réplication vers ou depuis [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].|  
|[!INCLUDE[ssEW](../../includes/ssew-md.md)]|La réplication est prise en charge si chaque point de terminaison [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se trouve dans deux versions principales de la version actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par conséquent, [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] ne prend pas en charge la réplication vers ou depuis [!INCLUDE[ssEW](../../includes/ssew-md.md)].|  
  
## <a name="see-also"></a>Voir aussi  
 [Compatibilité descendante de la réplication](../../relational-databases/replication/replication-backward-compatibility.md)  
  
  
