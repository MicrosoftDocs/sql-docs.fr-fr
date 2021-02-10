---
description: Fonctionnement conjoint des gestionnaires d’événements
title: Fonctionnement de l’ensemble des gestionnaires d’événements | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- events [ADO], about event handlers
- unpaired event handlers [ADO]
- paired event handlers [ADO]
- single event handlers [ADO]
- event handlers [ADO]
- multiple object event handlers [ADO]
ms.assetid: a86c8a02-dd69-420d-8a47-0188b339858d
author: rothja
ms.author: jroth
ms.openlocfilehash: dd6950ec37ddf974470e29ae554603723b299ef5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037349"
---
# <a name="how-event-handlers-work-together"></a>Fonctionnement conjoint des gestionnaires d’événements
À moins que vous ne programmiez dans Visual Basic, tous les gestionnaires d’événements pour les événements **Connection** et **Recordset** doivent être implémentés, que vous soyez en fait en train de traiter tous les événements. La quantité de travail d’implémentation que vous devez effectuer dépend de votre langage de programmation. Pour plus d’informations, consultez [instanciation des événements ADO par langage](./ado-event-instantiation-by-language.md).  
  
## <a name="paired-event-handlers"></a>Gestionnaires d’événements associés  
 Chaque gestionnaire d’événements est associé à un gestionnaire d’événements **complet** . Par exemple, lorsque votre application modifie la valeur d’un champ, le gestionnaire d’événements **WillChangeField** est appelé. Si la modification est acceptable, votre application laisse le paramètre **adStatus** inchangé et l’opération est effectuée. Une fois l’opération terminée, un événement **FieldChangeComplete** informe votre application que l’opération est terminée. S’il s’est terminé avec succès, **adStatus** contient **adStatusOK**; dans le cas contraire, **adStatus** contient **adStatusErrorsOccurred** et vous devez vérifier l’objet **Error** pour déterminer la cause de l’erreur.  
  
 Lorsque **WillChangeField** est appelé, vous pouvez déterminer que la modification ne doit pas être effectuée. Dans ce cas, définissez **adStatus** sur **adStatusCancel.** L’opération est annulée et l’événement **FieldChangeComplete** reçoit une valeur **adStatus** de **adStatusErrorsOccurred**. L’objet **Error** contient **adErrOperationCancelled** afin que votre gestionnaire **FieldChangeComplete** sache que l’opération a été annulée. Toutefois, vous devez vérifier la valeur du paramètre **adStatus** avant de la modifier, car la définition de **adStatus** sur **adStatusCancel** n’a aucun effet si le paramètre a la valeur **adStatusCantDeny** à l’entrée de la procédure.  
  
 Parfois, une opération peut déclencher plusieurs événements. Par exemple, l’objet **Recordset** possède des événements associés pour les modifications de **champ** et les modifications d' **enregistrements** . Lorsque votre application modifie la valeur d’un **champ**, le gestionnaire d’événements **WillChangeField** est appelé. S’il détermine que l’opération peut continuer, le gestionnaire d’événements **WillChangeRecord** est également déclenché. Si ce gestionnaire autorise également l’événement à continuer, la modification est apportée et les gestionnaires d’événements **FieldChangeComplete** et **RecordChangeComplete** sont appelés. L’ordre dans lequel les gestionnaires d’événements pour une opération particulière sont appelés n’est pas défini. vous devez donc éviter d’écrire du code qui dépend des gestionnaires d’appel dans une séquence particulière.  
  
 Dans les instances où plusieurs événements sont déclenchés, l’un des événements peut annuler l’opération en attente. Par exemple, lorsque votre application modifie la valeur d’un **champ**, les gestionnaires d’événements **WillChangeField** et **WillChangeRecord** sont normalement appelés. Toutefois, si l’opération est annulée dans le premier gestionnaire d’événements, son gestionnaire **complet** associé est immédiatement appelé avec **adStatusOperationCancelled**. Le deuxième gestionnaire n’est jamais appelé. Toutefois, si le premier gestionnaire d’événements autorise l’événement à continuer, l’autre gestionnaire d’événements est appelé. S’il annule ensuite l’opération, les deux événements **complets** sont appelés comme dans les exemples précédents.  
  
## <a name="unpaired-event-handlers"></a>Gestionnaires d’événements non couplés  
 Tant que l’état passé à l’événement n’est pas **adStatusCantDeny**, vous pouvez désactiver les notifications d’événements pour tout événement en retournant **adStatusUnwantedEvent** dans le paramètre *Status* . Par exemple, lorsque votre gestionnaire d’événements **complet** est appelé la première fois, vous pouvez retourner **adStatusUnwantedEvent**. Par la **suite, vous recevrez uniquement les** événements. Toutefois, certains événements peuvent être déclenchés pour plusieurs raisons. Dans ce cas, l’événement aura un paramètre *reason* . Lorsque vous retournez **adStatusUnwantedEvent**, vous ne recevrez plus de notifications pour cet événement uniquement lorsqu’elles se produisent pour cette raison particulière. En d’autres termes, vous recevrez potentiellement une notification pour chaque raison possible de déclenchement de l’événement.  
  
 Les gestionnaires d’événements Single **ne** peuvent être utiles lorsque vous souhaitez examiner les paramètres qui seront utilisés dans une opération. Vous pouvez modifier ces paramètres d’opération ou annuler l’opération.  
  
 Vous pouvez également conserver la notification d’événements **complète** activée. Lorsque votre premier gestionnaire d’événements sera appelé, retournez **adStatusUnwantedEvent**. Par la suite, vous recevrez uniquement les événements **complets** .  
  
 Les gestionnaires d’événements **complets** peuvent être utiles pour la gestion des opérations asynchrones. Chaque opération asynchrone a un événement **complet** approprié.  
  
 Par exemple, le remplissage d’un objet [Recordset](../../reference/ado-api/recordset-object-ado.md) volumineux peut prendre beaucoup de temps. Si votre application est écrite correctement, vous pouvez démarrer une `Recordset.Open(...,adAsyncExecute)` opération et continuer avec d’autres traitements. Vous serez alors averti lorsque le **Recordset** sera rempli par un événement **ExecuteComplete** .  
  
## <a name="single-event-handlers-and-multiple-objects"></a>Gestionnaires d’événements uniques et plusieurs objets  
 La flexibilité d’un langage de programmation comme Microsoft Visual C++® vous permet d’avoir un gestionnaire d’événements qui traite les événements de plusieurs objets. Par exemple, vous pouvez avoir un gestionnaire d’événements **Disconnect** pour traiter les événements de plusieurs objets de **connexion** . Si l’une des connexions est terminée, le gestionnaire d’événements de **déconnexion** est appelé. Vous pouvez indiquer la connexion à l’origine de l’événement, car le paramètre de l’objet de gestionnaire d’événements est défini sur l’objet de **connexion** correspondant.  
  
> [!NOTE]
>  Cette technique ne peut pas être utilisée dans Visual Basic, car ce langage ne peut corréler qu’un seul objet à un gestionnaire d’événements.  
  
## <a name="see-also"></a>Voir aussi  
 [Résumé du gestionnaire d’événements ADO](./ado-event-handler-summary.md)   
 [Instanciation des événements ADO par langage](./ado-event-instantiation-by-language.md)   
 [Paramètres d’événement](./event-parameters.md)   
 [Types d’événements](./types-of-events.md)