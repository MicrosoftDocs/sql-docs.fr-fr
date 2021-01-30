---
description: Événements BeginTransComplete, CommitTransComplete et RollbackTransComplete (ADO)
title: BeginTrans, CommitTrans, RollbackTrans, événements (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CommitTransComplete
- Connection::BeginTransComplete
- Connection::RollbackTransComplete
- Connection::CommitTransComplete
- RollbackTransComplete
- BeginTransComplete
helpviewer_keywords:
- CommitTransComplete event [ADO]
- RollbackTransComplete event [ADO]
- BeginTransComplete event [ADO]
ms.assetid: ec4e4b38-e9c6-4757-b2ef-4e468ae5f1d8
author: rothja
ms.author: jroth
ms.openlocfilehash: 013df2c56a973d8df6b51434f016ee7642fc4f71
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164789"
---
# <a name="begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado"></a>Événements BeginTransComplete, CommitTransComplete et RollbackTransComplete (ADO)
Ces événements sont appelés après la fin de l’exécution de l’opération associée sur l’objet de [connexion](./connection-object-ado.md) .  
  
-   **BeginTransComplete** est appelé après l’opération [BeginTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) .  
  
-   **CommitTransComplete** est appelé après l’opération [CommitTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) .  
  
-   **RollbackTransComplete** est appelé après l’opération [RollbackTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BeginTransComplete TransactionLevel, pError, adStatus, pConnection  
CommitTransComplete pError, adStatus, pConnection  
RollbackTransComplete pError, adStatus, pConnection  
```  
  
#### <a name="parameters"></a>Paramètres  
 *TransactionLevel*  
 Valeur de **type long** qui contient le nouveau niveau de transaction du **BeginTrans** à l’origine de cet événement.  
  
 *pError*  
 Objet d' [erreur](./error-object.md) . Il décrit l’erreur qui s’est produite si la valeur de EventStatusEnum est **adStatusErrorsOccurred**; dans le cas contraire, il n’est pas défini.  
  
 *adStatus*  
 Valeur d’état [EventStatusEnum](./eventstatusenum.md) . Lorsque l’un de ces événements est appelé, ce paramètre a la valeur **adStatusOK** si l’opération à l’origine de l’événement a réussi, ou à **adStatusErrorsOccurred** si l’opération a échoué.  
  
 Ces événements peuvent empêcher les notifications suivantes en affectant à ce paramètre la valeur **adStatusUnwantedEvent** avant le retour de l’événement.  
  
 *pConnection*  
 Objet de **connexion** pour lequel cet événement s’est produit.  
  
## <a name="remarks"></a>Notes  
 Dans Visual C++, plusieurs **connexions** peuvent partager la même méthode de gestion des événements. La méthode utilise l’objet de **connexion** retourné pour déterminer l’objet qui a provoqué l’événement.  
  
 Si la propriété [attributes](./attributes-property-ado.md) a la valeur **adXactCommitRetaining** ou **adXactAbortRetaining**, une nouvelle transaction démarre après la validation ou la restauration d’une transaction. Utilisez l’événement **BeginTransComplete** pour ignorer tous les événements de début de transaction sauf le premier.  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de modèle d’événements ADO (VC + +)](./ado-events-model-example-vc.md)   
 [BeginTrans, CommitTrans et RollbackTrans, exemples de méthodes (VB)](./begintrans-committrans-and-rollbacktrans-methods-example-vb.md)   
 [Résumé du gestionnaire d’événements ADO](../../guide/data/ado-event-handler-summary.md)   
 [BeginTrans, CommitTrans et RollbackTrans, méthodes (ADO)](./begintrans-committrans-and-rollbacktrans-methods-ado.md)