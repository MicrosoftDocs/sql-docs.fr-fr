---
title: Transactions XTP de SQL Server | Microsoft Docs
description: Découvrez l’objet de performance des transactions XTP de SQL Server, qui comprend des compteurs liés aux transactions impliquant l’OLTP en mémoire dans SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: 443d67e4-1c7f-41d7-b18d-2d657f58c22a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 84d00aa0fc9f9677af96604788dd374e8d08e92d
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505453"
---
# <a name="sql-server-xtp-transactions"></a>Transactions XTP de SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  L’objet de performance des transactions XTP de SQL Server comprend des compteurs liés aux transactions impliquant l’OLTP en mémoire dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Ce tableau décrit les compteurs **Transactions XTP de SQL Server** .  
  
|Compteur|Description|  
|-------------|-----------------|  
|**Abandons en cascade par seconde**|Nombre de transactions restaurées en raison d'une restauration de dépendance de validation (en moyenne), par seconde.|  
|**Dépendances de validation prises par seconde**|Nombre de dépendances de validation prises par des transactions (en moyenne), par seconde.|  
|**Transactions en lecture seule préparées par seconde**|Nombre de transactions en lecture seule qui ont été préparées pour le traitement de validation, par seconde.|  
|**Actualisations de point de sauvegarde par seconde**|Nombre de fois qu'un point de sauvegarde a été « actualisé » (en moyenne), par seconde. Une actualisation de point de sauvegarde se produit lorsqu'un point de sauvegarde existant est réinitialisé au point actuel dans la durée de vie de la transaction.|  
|**Restaurations de point de sauvegarde par seconde**|Nombre de fois qu'une transaction a été restaurée à un point de sauvegarde (en moyenne), par seconde.|  
|**Points de sauvegarde créés par seconde**|Nombre de points de sauvegarde créés (en moyenne), par seconde.|  
|**Échec de validation des transactions par seconde**|Nombre de transactions ayant échoué lors du traitement de validation (en moyenne), par seconde.|  
|**Transactions abandonnées par l'utilisateur par seconde**|Nombre de transactions abandonnées par l'utilisateur (en moyenne), par seconde.|  
|**Transactions abandonnées par seconde**|Nombre de transactions abandonnées par l'utilisateur et par le système (en moyenne), par seconde.|  
|**Transactions créées par seconde**|Nombre de transactions créées dans le système (en moyenne), par seconde.<br /><br /> Les transactions XTP sont comptées différemment que les transactions sur disque (comme obtenu par réflexion dans Databases:Transactions/sec). Par exemple, les transactions created/sec comptent les transactions read/only, contrairement à Databases:Transactions/sec.|  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server XTP &#40;OLTP en mémoire&#41;, compteurs de performances](../../relational-databases/performance-monitor/sql-server-xtp-in-memory-oltp-performance-counters.md)  
  
  
