---
description: ReadyState, propriété (RDS)
title: ReadyState, propriété (RDS) | Microsoft Docs
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- ReadyState property [ADO]
ms.assetid: 5be75bc7-1171-4440-a37e-c8cc6b5cd865
author: rothja
ms.author: jroth
ms.openlocfilehash: 3bd2f05a90acbbade46e6897cabdee49246a68c7
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88438751"
---
# <a name="readystate-property-rds"></a>ReadyState, propriété (RDS)
Indique la progression d’un objet [DataControl](../../../ado/reference/rds-api/datacontrol-object-rds.md) lorsqu’il récupère des données dans son objet [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) .  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](https://go.microsoft.com/fwlink/?LinkId=199565).  
  
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
 Utilisez l’événement [onreadystatechange](../../../ado/reference/rds-api/onreadystatechange-event-rds.md) pour surveiller les modifications apportées à la propriété **ReadyState** pendant une opération de requête asynchrone. C’est plus efficace que de vérifier régulièrement la valeur de la propriété.  
  
 Si une erreur se produit pendant une opération asynchrone, la propriété **ReadyState** devient **adcReadyStateComplete**, la propriété [State](../../../ado/reference/ado-api/state-property-ado.md) passe de **AdStateExecuting** à **adStateClosed**, et la propriété [value](../../../ado/reference/ado-api/value-property-ado.md) de l’objet **Recordset** *ne conserve rien*.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](../../../ado/reference/rds-api/datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ReadyState, exemple de propriété (VBScript)](../../../ado/reference/rds-api/readystate-property-example-vbscript.md)   
 [Cancel, méthode (RDS)](../../../ado/reference/rds-api/cancel-method-rds.md)   
 [ExecuteOptions, propriété (RDS)](../../../ado/reference/rds-api/executeoptions-property-rds.md)


