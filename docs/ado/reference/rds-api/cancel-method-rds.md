---
description: Cancel, méthode (RDS)
title: Cancel, méthode (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Cancel method [RDS]
ms.assetid: 560b5b3d-fba9-4275-8920-9c3e186134f7
author: rothja
ms.author: jroth
ms.openlocfilehash: e87e8d173e1ab9aa8978ed9ab98c6a2f37a93e79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169067"
---
# <a name="cancel-method-rds"></a>Cancel, méthode (RDS)
Annule l’exécution d’un appel de méthode asynchrone en attente.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
RDS.DataControl.Cancel  
```  
  
## <a name="remarks"></a>Notes  
 Quand vous appelez **Cancel**, [ReadyState](./readystate-property-rds.md) est automatiquement défini sur **adcReadyStateLoaded** et le [Recordset](../ado-api/recordset-object-ado.md) est vide.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Cancel, exemple de méthode (VBScript)](./cancel-method-example-vbscript.md)   
 [Cancel, méthode (ADO)](../ado-api/cancel-method-ado.md)   
 [Méthode CancelBatch (ADO)](../ado-api/cancelbatch-method-ado.md)   
 [CancelUpdate, méthode (ADO)](../ado-api/cancelupdate-method-ado.md)   
 [CancelUpdate, méthode (RDS)](./cancelupdate-method-rds.md)   
 [ExecuteOptions, propriété (RDS)](./executeoptions-property-rds.md)