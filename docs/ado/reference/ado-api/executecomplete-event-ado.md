---
description: ExecuteComplete, événement (ADO)
title: ExecuteComplete, événement (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection::ExecuteComplete
- ExecuteComplete
helpviewer_keywords:
- ExecuteComplete event [ADO]
ms.assetid: 62470d42-e511-494c-bec4-ad4591734b7b
author: rothja
ms.author: jroth
ms.openlocfilehash: c64856ccbd3cdada037ffd77a9ac3b0e74add570
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171127"
---
# <a name="executecomplete-event-ado"></a>ExecuteComplete, événement (ADO)
L’événement **ExecuteComplete** est appelé à la fin de l’exécution d’une commande.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
ExecuteComplete RecordsAffected, pError, adStatus, pCommand, pRecordset, pConnection  
```  
  
#### <a name="parameters"></a>Paramètres  
 *RecordsAffected*  
 Valeur de **type long** indiquant le nombre d’enregistrements affectés par la commande.  
  
 *pError*  
 Objet d' [erreur](../../../ado/reference/ado-api/error-object.md) . Il décrit l’erreur qui s’est produite si la valeur de **adStatus** est **adStatusErrorsOccurred**; dans le cas contraire, il n’est pas défini.  
  
 *adStatus*  
 Valeur d’état [EventStatusEnum](../../../ado/reference/ado-api/eventstatusenum.md) . Lorsque cet événement est appelé, ce paramètre a la valeur **adStatusOK** si l’opération à l’origine de l’événement s’est déroulée correctement ou **adStatusErrorsOccurred** si l’opération a échoué.  
  
 Avant le retour de cet événement, définissez ce paramètre sur **adStatusUnwantedEvent** pour empêcher les notifications suivantes.  
  
 *pCommand*  
 Objet de [commande](../../../ado/reference/ado-api/command-object-ado.md) qui a été exécuté. Contient un objet **Command** même lors de l’appel de **Connection.Exejolie** ou **Recordset. Open** sans créer explicitement une **commande**, auquel cas l’objet **Command** est créé en interne par ADO.  
  
 *pRecordset*  
 Objet [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) qui est le résultat de la commande exécutée. Ce **jeu d’enregistrements** peut être vide. Vous ne devez jamais détruire cet objet Recordset dans ce gestionnaire d’événements. Cela entraînera une violation d’accès lorsque ADO tente d’accéder à un objet qui n’existe plus.  
  
 *pConnection*  
 Objet de [connexion](../../../ado/reference/ado-api/connection-object-ado.md) . Connexion sur laquelle l’opération a été exécutée.  
  
## <a name="remarks"></a>Notes  
 Un événement **ExecuteComplete** peut se produire en raison de la **connexion.** [Exécutez](../../../ado/reference/ado-api/execute-method-ado-connection.md)la **commande.** [Exécutez](../../../ado/reference/ado-api/execute-method-ado-command.md), **Recordset.** [Ouvrez](../../../ado/reference/ado-api/open-method-ado-recordset.md), **Recordset.** [Requery](../../../ado/reference/ado-api/requery-method.md)ou **Recordset.** Méthodes [NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md) .  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de modèle d’événements ADO (VC + +)](../../../ado/reference/ado-api/ado-events-model-example-vc.md)   
 [Présentation rapide du gestionnaire d’événements ADO](../../../ado/guide/data/ado-event-handler-summary.md)
