---
description: Types d’événements
title: Types d’événements | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- EventComplete event [ADO]
- events [ADO], types
- Will events [ADO]
- complete events [ADO]
- WillEvent event [ADO]
ms.assetid: f3327ea0-635a-43d4-bd78-c1674f62f1a2
author: rothja
ms.author: jroth
ms.openlocfilehash: 9da4e5cefa1c33a2a1b4b5002dc9030550265ca3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032341"
---
# <a name="types-of-events"></a>Types d’événements
Il existe deux types d’événements de base. Les événements « do Events », qui sont appelés avant le démarrage d’une opération, incluent généralement «» dans leur nom, par exemple **WillChangeRecordset** ou **WillConnect**. Les événements qui sont appelés après la fin d’un événement incluent généralement « Complete » dans leur nom, par exemple, **RecordChangeComplete** ou **ConnectComplete**. Il existe des exceptions, telles que **InfoMessage** , mais elles se produisent une fois l’opération associée terminée.  
  
## <a name="will-events"></a>Événements  
 Les gestionnaires d’événements appelés avant le démarrage de l’opération vous offrent la possibilité d’examiner ou de modifier les paramètres d’opération, puis d’annuler l’opération ou de l’autoriser à se terminer. Ces routines de gestionnaire d’événements ont généralement des noms de <strong>type *événement*</strong>.  
  
## <a name="complete-events"></a>Événements complets  
 Les gestionnaires d’événements appelés après la fin d’une opération peuvent informer votre application qu’une opération a été effectuée. Un gestionnaire d’événements de ce type est également notifié lorsqu’un gestionnaire d’événements va annuler une opération en attente. Ces routines de gestionnaire d’événements ont généralement des noms de l' <strong> *événement* Form Complete</strong>.  
  
 Les événements d’exécution et de fin sont généralement utilisés par paires.  
  
## <a name="other-events"></a>Autres événements  
 Les autres gestionnaires d’événements, autrement dit, les événements dont le nom n’est pas au format <strong>*événement*</strong> ou <strong> *événement* terminé</strong> , sont appelés uniquement après la fin d’une opération. Ces événements sont **Disconnect**, **EndOfRecordset** et **InfoMessage**.  
  
## <a name="see-also"></a>Voir aussi  
 [Résumé du gestionnaire d’événements ADO](../../../ado/guide/data/ado-event-handler-summary.md)   
 [Instanciation des événements ADO par langage](../../../ado/guide/data/ado-event-instantiation-by-language.md)   
 [Paramètres d’événement](../../../ado/guide/data/event-parameters.md)   
 [Fonctionnement conjoint des gestionnaires d’événements](../../../ado/guide/data/how-event-handlers-work-together.md)
