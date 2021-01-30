---
description: Refresh, méthode (RDS)
title: Refresh, méthode (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
f1_keywords:
- Refresh
- RDS.DataControl::Refresh
- DataControl::Refresh
helpviewer_keywords:
- Refresh method [RDS]
ms.assetid: c90a8050-0ff4-4c83-9925-261f2f2ccfe9
author: rothja
ms.author: jroth
ms.openlocfilehash: f35574b0e9623560ddfafe123340e302a9fc5220
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168795"
---
# <a name="refresh-method-rds"></a>Refresh, méthode (RDS)
Interroge à jour la source de données spécifiée dans la propriété [Connect](./connect-property-rds.md) et met à jour les résultats de la requête.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DataControl.Refresh  
```  
  
#### <a name="parameters"></a>Paramètres  
 *DataControl*  
 Variable objet qui représente un objet [RDS. DataControl](./datacontrol-object-rds.md) .  
  
## <a name="remarks"></a>Notes  
 Vous devez définir les propriétés [Connect](./connect-property-rds.md), [Server](./server-property-rds.md)et [SQL](./sql-property.md) avant d’utiliser la méthode **Refresh** . Tous les contrôles liés aux données sur le formulaire associé à un **objet RDS. DataControl** reflète le nouvel ensemble d’enregistrements. Tout objet [Recordset](../ado-api/recordset-object-ado.md) préexistant est libéré, et toutes les modifications non enregistrées sont ignorées. La méthode **Refresh** fait automatiquement du premier enregistrement l’enregistrement actif.  
  
 Il est judicieux d’appeler régulièrement la méthode **Refresh** lorsque vous travaillez avec des données. Si vous récupérez des données, puis que vous les laissez sur un ordinateur client pendant un certain temps, elles risquent de devenir obsolètes. Il est possible que toutes les modifications que vous apportez échouent, car une autre personne a peut-être modifié l’enregistrement et a envoyé des modifications avant.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Refresh, exemple de méthode (VB)](../ado-api/refresh-method-example-vb.md)   
 [Refresh, exemple de méthode (VBScript)](./refresh-method-example-vbscript.md)   
 [Boutons de commande du carnet d’adresses](../../guide/remote-data-service/address-book-command-buttons.md)   
 [CancelUpdate, méthode (RDS)](./cancelupdate-method-rds.md)   
 [Refresh, méthode (ADO)](../ado-api/refresh-method-ado.md)   
 [SubmitChanges, méthode (RDS)](./submitchanges-method-rds.md)