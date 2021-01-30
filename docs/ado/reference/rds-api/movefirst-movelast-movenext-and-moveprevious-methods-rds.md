---
description: MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes (RDS)
title: Méthodes MoveFirst, MoveLast, MoveNext et MovePrevious (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- MoveLast method [RDS]
- MovePrevious method [RDS]
- MoveFirst method [RDS]
- MoveNext method [RDS]
ms.assetid: 45c80bb5-136f-4204-9df2-78740fa55574
author: rothja
ms.author: jroth
ms.openlocfilehash: 80a0a2b5fae2339718089903060c512595957be3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168915"
---
# <a name="movefirst-movelast-movenext-and-moveprevious-methods-rds"></a>MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes (RDS)
Passe au premier enregistrement, dernier, suivant ou précédent dans un objet [Recordset](../ado-api/recordset-object-ado.md) spécifié.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DataControl.Recordset.{MoveFirst | MoveLast | MoveNext | MovePrevious}  
```  
  
#### <a name="parameters"></a>Paramètres  
 *DataControl*  
 Variable objet qui représente un objet [RDS. DataControl](./datacontrol-object-rds.md) .  
  
## <a name="remarks"></a>Notes  
 Vous pouvez utiliser les méthodes **Move** avec le **RDS. DataControl** pour parcourir les enregistrements de données dans les contrôles liés aux données sur une page Web. Supposons, par exemple, que vous affichez un **jeu d’enregistrements** dans une grille en le liant à un **objet RDS. DataControl** . Vous pouvez ensuite inclure les boutons premier, dernier, suivant et précédent sur lesquels les utilisateurs peuvent cliquer pour passer au premier enregistrement, dernier, suivant ou précédent dans le **jeu d’enregistrements** affiché. Pour ce faire, appelez les méthodes **MoveFirst**, **MoveLast**, **MoveNext** et **MovePrevious** de l' **objet RDS. DataControl** dans les procédures OnClick pour les boutons First, Last, Next et Previous, respectivement. L' [exemple de carnet d’adresses](../../guide/remote-data-service/address-book-navigation-buttons.md) montre comment procéder.  
  
## <a name="applies-to"></a>S'applique à  
 [DataControl, objet (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Move, méthode (ADO)](../ado-api/move-method-ado.md)   
 [MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes (ADO)](../ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)   
 [MoveRecord, méthode (ADO)](../ado-api/moverecord-method-ado.md)