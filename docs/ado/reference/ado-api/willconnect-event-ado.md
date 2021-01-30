---
description: WillConnect, événement (ADO)
title: WillConnect, événement (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- WillConnect
- Connection::WillConnect
helpviewer_keywords:
- WillConnect event [ADO]
ms.assetid: da561d58-eb58-446c-a4fd-1838c76073c0
author: rothja
ms.author: jroth
ms.openlocfilehash: a57b99597bea639eddb5e73b2d6e4c7137333bc2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166268"
---
# <a name="willconnect-event-ado"></a>WillConnect, événement (ADO)
L’événement **WillConnect** est appelé avant le démarrage d’une connexion.  
  
 **S’applique à :** [objet Connection (ADO)](./connection-object-ado.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
WillConnect ConnectionString, UserID, Password, Options, adStatus, pConnection  
```  
  
#### <a name="parameters"></a>Paramètres  
 *ConnectionString*  
 **Chaîne** qui contient les informations de connexion pour la connexion en attente.  
  
 *l'UserId*  
 **Chaîne** qui contient un nom d’utilisateur pour la connexion en attente.  
  
 *Mot de passe*  
 **Chaîne** qui contient un mot de passe pour la connexion en attente.  
  
 *Options*  
 Valeur de **type long** qui indique comment le fournisseur doit évaluer *ConnectionString*. La seule option est **adAsyncOpen**.  
  
 *adStatus*  
 Valeur d’état [EventStatusEnum](./eventstatusenum.md) .  
  
 Lorsque cet événement est appelé, ce paramètre a la valeur **adStatusOK** par défaut. Elle a la valeur **adStatusCantDeny** si l’événement ne peut pas demander l’annulation de l’opération en attente.  
  
 Avant le retour de cet événement, définissez ce paramètre sur **adStatusUnwantedEvent** pour empêcher les notifications suivantes. Affectez à ce paramètre la valeur **adStatusCancel** pour demander l’opération de connexion qui a provoqué l’annulation de cette notification.  
  
 *pConnection*  
 Objet de [connexion](./connection-object-ado.md) pour lequel cette notification d’événement s’applique. Les modifications apportées aux paramètres de la **connexion** par le gestionnaire d’événements **WillConnect** n’auront aucun effet sur la **connexion**.  
  
## <a name="remarks"></a>Notes  
 Lorsque l’option **WillConnect** est appelée, les paramètres *ConnectionString*, *userid*, *Password* et *options* sont définis sur les valeurs établies par l’opération qui a provoqué cet événement (la connexion en attente) et peuvent être modifiées avant le retour de l’événement. Le **WillConnect** peut retourner une demande indiquant que la connexion en attente doit être annulée.  
  
 Lorsque cet événement est annulé, **ConnectComplete** est appelé avec le paramètre *adStatus* défini sur **adStatusErrorsOccurred**.  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de modèle d’événements ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Présentation rapide du gestionnaire d’événements ADO](../../guide/data/ado-event-handler-summary.md)