---
description: Interblocages avec le niveau d’isolation REPEATABLE READ
title: Interblocages avec le niveau d’isolation lecture renouvelable | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- deadlocks in RDS [ADO]
- read repeatable in RDS [ADO]
ms.assetid: 29f3683f-12f3-4304-8a54-fe133c25a423
author: rothja
ms.author: jroth
ms.openlocfilehash: 398fd636c7e7ebfce448d5b7ebfae7a62d7c1a2b
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88452211"
---
# <a name="deadlocks-with-read-repeatable-isolation-level"></a>Interblocages avec le niveau d’isolation REPEATABLE READ
Si un objet métier personnalisé utilise un niveau d’isolation lecture renouvelable pour accéder à un SQL Server, et que l’objet métier est appelé simultanément par deux clients qui envoient une requête et mettent à jour dans la même transaction, un blocage est possible. Le service de données à distance est conçu pour permettre à l’un des processus de libérer le délai pour libérer le blocage, mais la mise à jour échoue pour ce client.  
  
 Utilisez la propriété dynamique délai d’expiration de la **commande** [Cursor Service](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) pour modifier la longueur du délai d’attente.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](https://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts de base de RDS](../../../ado/guide/remote-data-service/rds-fundamentals.md)



