---
description: InfoMessage, événement (ADO)
title: InfoMessage, événement (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Connection::InfoMessage
- InfoMessage
helpviewer_keywords:
- InfoMessage event [ADO]
ms.assetid: 468c87dd-e3bc-4084-9941-94d10743d4e9
author: rothja
ms.author: jroth
ms.openlocfilehash: 8695dc364a649dff204fcd689ab4722f19e125b9
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88990800"
---
# <a name="infomessage-event-ado"></a>InfoMessage, événement (ADO)
L’événement **InfoMessage** est appelé chaque fois qu’un avertissement se produit pendant une opération **ConnectionEvent** .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
InfoMessage pError, adStatus, pConnection  
```  
  
#### <a name="parameters"></a>Paramètres  
 *pError*  
 Objet d' [erreur](./error-object.md) . Ce paramètre contient toutes les erreurs retournées. Si plusieurs erreurs sont retournées, énumérez la collection **Errors** pour les Rechercher.  
  
 *adStatus*  
 Valeur d’état [EventStatusEnum](./eventstatusenum.md) . Si un avertissement se produit, *adStatus* est défini sur **adStatusOK** et *perror* contient l’avertissement.  
  
 Avant le retour de cet événement, définissez ce paramètre sur **adStatusUnwantedEvent** pour empêcher les notifications suivantes.  
  
 *pConnection*  
 Objet de [connexion](./connection-object-ado.md) . Connexion pour laquelle l’avertissement s’est produit. Par exemple, des avertissements peuvent se produire lors de l’ouverture d’un objet de **connexion** ou de l’exécution d’une [commande](./command-object-ado.md) sur une **connexion**.  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de modèle d’événements ADO (VC + +)](./ado-events-model-example-vc.md)   
 [Résumé du gestionnaire d’événements ADO](../../guide/data/ado-event-handler-summary.md)   
 [Connection, objet (ADO)](./connection-object-ado.md)