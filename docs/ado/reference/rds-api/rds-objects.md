---
description: Objets RDS
title: Objets RDS | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
helpviewer_keywords:
- objects [ADO], RDS
- RDS objects [ADO]
ms.assetid: f2ac8b3b-f968-41c4-a504-7aee3538b7c7
author: rothja
ms.author: jroth
ms.openlocfilehash: 1f53cd2ef72b6ff540a8663f6400c8dcdf2fb822
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052980"
---
# <a name="rds-objects"></a>Objets RDS
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
|Object|Description|  
|-|-|  
|[DataControl (RDS)](./datacontrol-object-rds.md)|Lie un **jeu d’enregistrements** de requêtes de données à un ou plusieurs contrôles (par exemple, une zone de texte, un contrôle de grille ou une zone de liste déroulante) pour afficher les données du **Recordset** sur une page Web.<br /><br /> L’objet **DataControl** est sécurisé pour l’écriture de scripts.|  
|[DataFactory (RDSServer)](./datafactory-object-rdsserver.md)|Implémente des méthodes qui fournissent un accès en lecture/écriture aux données des sources de données spécifiées pour les applications côté client.<br /><br /> L’objet **DataFactory** n’est pas sûr pour l’écriture de scripts.|  
|[DataSpace (RDS)](./dataspace-object-rds.md)|Crée des proxies côté client pour des objets métier personnalisés situés sur la couche intermédiaire.<br /><br /> L’objet **DataSpace** est sécurisé pour l’écriture de scripts.|  
|[IRDSService, interface (RDS)](./irdsservice-interface-rds.md)|Expose la méthode [InvokeService (RDS)](./invokeservice-rds.md) , qui est utilisée pour retourner un pointeur vers l’interface demandée sur une version plus puissante de l’objet.|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence sur l’API RDS](./rds-api-reference.md)