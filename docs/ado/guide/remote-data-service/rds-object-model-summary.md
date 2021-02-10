---
description: Résumé du modèle objet RDS
title: Résumé du modèle objet RDS | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS objects [ADO], object model summary
- RDS object model [ADO]
ms.assetid: 909f9af7-31db-4eec-ad52-650ce74dac2f
author: rothja
ms.author: jroth
ms.openlocfilehash: 67f6cdaeea36e5ed13e28a7674a0bb4b3d6ef3f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036459"
---
# <a name="rds-object-model-summary"></a>Résumé du modèle objet RDS
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
|Object|Description|  
|------------|-----------------|  
|[RDS.DataSpace](../../reference/rds-api/dataspace-object-rds.md)|Cet objet contient une méthode pour obtenir un serveur proxy. Le proxy peut être la valeur par défaut ou un programme serveur personnalisé (objet métier). Le programme serveur peut être appelé sur Internet, sur un intranet, sur un réseau local ou sur une bibliothèque de liens dynamiques locale.<br /><br /> L’objet **DataSpace** est sécurisé pour l’écriture de scripts.|  
|[RDSServer.DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md)|Cet objet représente le programme serveur par défaut. Il exécute le comportement par défaut de la récupération et de la mise à jour des données RDS.<br /><br /> L’objet **DataFactory** n’est pas sûr pour l’écriture de scripts.|  
|[RDS.DataControl](../../reference/rds-api/datacontrol-object-rds.md)|Cet objet peut appeler automatiquement le **RDS. Objets DataSpace** et **RDSServer. DataFactory** .<br /><br /> Utilisez cet objet pour appeler le comportement par défaut de la récupération ou de la mise à jour des données RDS.<br /><br /> Cet objet fournit également les moyens pour les contrôles visuels d’accéder à l’objet **Recordset** retourné.<br /><br /> L’objet **DataControl** est sécurisé pour l’écriture de scripts.|  
  
## <a name="see-also"></a>Voir aussi  
 [Notions de base sur RDS](./rds-fundamentals.md)   
 [Scénario RDS](./rds-scenario.md)   
 [Didacticiel RDS](./rds-tutorial.md)   
 [Utilisation et sécurité de RDS](./rds-usage-and-security.md)