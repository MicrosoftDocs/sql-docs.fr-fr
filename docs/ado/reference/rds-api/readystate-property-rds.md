---
description: ReadyState, propriété (RDS)
title: ReadyState, propriété (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- ReadyState property [ADO]
ms.assetid: 5be75bc7-1171-4440-a37e-c8cc6b5cd865
author: rothja
ms.author: jroth
ms.openlocfilehash: a70a2f7fc26e83b21cdb85edb3ddf4a664785d30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049209"
---
# <a name="readystate-property-rds"></a>ReadyState, propriété (RDS)
Indique la progression d’un objet [DataControl](./datacontrol-object-rds.md) lorsqu’il récupère des données dans son objet [Recordset](../ado-api/recordset-object-ado.md) .  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**adcReadyStateLoaded**|La requête active est toujours en cours d’exécution et aucune ligne n’a été extraite. Le **jeu d’enregistrements** de l’objet **DataControl** ne peut pas être utilisé.|  
|**adcReadyStateInteractive**|Un ensemble initial de lignes extraites par la requête actuelle a été stocké dans le **Recordset** de l’objet **DataControl** et peut être utilisé. Les lignes restantes sont toujours en cours d’extraction.|  
|**adcReadyStateComplete**|Toutes les lignes récupérées par la requête en cours ont été stockées dans le **Recordset** de l’objet **DataControl** et peuvent être utilisées.<br /><br /> Cet État existe également si une opération a été annulée en raison d’une erreur, ou si l’objet **Recordset** n’est pas initialisé.|  
  
> [!NOTE]
>  Chaque fichier exécutable côté client qui utilise ces constantes doit fournir des déclarations pour ceux-ci. Vous pouvez couper et coller les déclarations de constantes de votre choix à partir du fichier Adcvbs. Inc, situé dans le dossier d’installation par défaut de la bibliothèque RDS.  
  
## <a name="remarks"></a>Notes  
 Utilisez l’événement [onreadystatechange](./onreadystatechange-event-rds.md) pour surveiller les modifications apportées à la propriété **ReadyState** pendant une opération de requête asynchrone. C’est plus efficace que de vérifier régulièrement la valeur de la propriété.  
  
 Si une erreur se produit pendant une opération asynchrone, la propriété **ReadyState** devient **adcReadyStateComplete**, la propriété [State](../ado-api/state-property-ado.md) passe de **AdStateExecuting** à **adStateClosed**, et la propriété [value](../ado-api/value-property-ado.md) de l’objet **Recordset** *ne conserve rien*.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ReadyState, exemple de propriété (VBScript)](./readystate-property-example-vbscript.md)   
 [Cancel, méthode (RDS)](./cancel-method-rds.md)   
 [ExecuteOptions, propriété (RDS)](./executeoptions-property-rds.md)