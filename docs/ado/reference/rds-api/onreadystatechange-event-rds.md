---
description: onReadyStateChange, événement (RDS)
title: onReadyStateChange, événement (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- onReadyStateChange event [ADO]
ms.assetid: bf2ae3ac-bfe4-4709-b50a-ea7c282c3164
author: rothja
ms.author: jroth
ms.openlocfilehash: b986f3afce40bf5b114a06cc152c1dd026857859
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049289"
---
# <a name="onreadystatechange-event-rds"></a>onReadyStateChange, événement (RDS)
L’événement **onreadystatechange** est appelé chaque fois que la valeur de la propriété [ReadyState](./readystate-property-rds.md) change.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
onReadyStateChange  
```  
  
#### <a name="parameters"></a>Paramètres  
 Aucun.  
  
## <a name="remarks"></a>Notes  
 La propriété **ReadyState** reflète la progression d’un [objet RDS. DataControl](./datacontrol-object-rds.md) , car il récupère de manière asynchrone des données dans son objet [Recordset](../ado-api/recordset-object-ado.md) . Utilisez l’événement **onreadystatechange** pour surveiller les modifications apportées à la propriété **ReadyState** à chaque fois qu’elles se produisent. C’est plus efficace que de vérifier régulièrement la valeur de la propriété.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de modèle d’événements ADO (VC + +)](../ado-api/ado-events-model-example-vc.md)   
 [Présentation rapide du gestionnaire d’événements ADO](../../guide/data/ado-event-handler-summary.md)