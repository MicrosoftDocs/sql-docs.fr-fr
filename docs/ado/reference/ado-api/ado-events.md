---
description: Événements ADO
title: Événements ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- events [ADO]
- ADO, events
ms.assetid: 0ded5ad9-8f83-4224-95af-38512783b972
author: rothja
ms.author: jroth
ms.openlocfilehash: 41431d92cd8c9dfea44ea88feadcce9df7edc3bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031271"
---
# <a name="ado-events"></a>Événements ADO

|Événement|Description|  
|-|-|  
|[BeginTransComplete](./begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|Appelé après l’opération **BeginTrans** .|  
|[CommitTransComplete](./begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|Appelé après l’opération **CommitTrans** .|  
|[ConnectComplete](./connectcomplete-and-disconnect-events-ado.md)|Appelée après le démarrage d’une connexion.|  
|[Déconnexion](./connectcomplete-and-disconnect-events-ado.md)|Appelé après la fin d’une connexion.|  
|[EndOfRecordset](./endofrecordset-event-ado.md)|Appelé en cas de tentative de déplacement vers une ligne située au-delà de la fin du **Recordset**.|  
|[ExecuteComplete](./executecomplete-event-ado.md)|Appelé après la fin de l’exécution d’une commande.|  
|[FetchComplete](./fetchcomplete-event-ado.md)|Appelée une fois que tous les enregistrements d’une longue opération asynchrone ont été récupérés dans le **Recordset**.|  
|[FetchProgress](./fetchprogress-event-ado.md)|Appelée régulièrement au cours d’une opération asynchrone de longue durée pour signaler le nombre de lignes qui ont été récupérées actuellement dans le **Recordset**.|  
|[FieldChangeComplete](./willchangefield-and-fieldchangecomplete-events-ado.md)|Appelé après la modification de la valeur d’un ou plusieurs objets **Field** .|  
|[InfoMessage](./infomessage-event-ado.md)|Appelé chaque fois qu’un avertissement se produit pendant une opération **ConnectionEvent** .|  
|[MoveComplete](./willmove-and-movecomplete-events-ado.md)|Appelé après la modification de la position actuelle dans l’ensemble **d’enregistrements** .|  
|[RecordChangeComplete](./willchangerecord-and-recordchangecomplete-events-ado.md)|Appelé après la modification d’un ou de plusieurs enregistrements.|  
|[RecordsetChangeComplete](./willchangerecordset-and-recordsetchangecomplete-events-ado.md)|Appelé après la modification du **Recordset** .|  
|[RollbackTransComplete](./begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|Appelé après l’opération **RollbackTrans** .|  
|[WillChangeField](./willchangefield-and-fieldchangecomplete-events-ado.md)|Appelé avant qu’une opération en attente modifie la valeur d’un ou plusieurs objets **Field** dans le **Recordset**.|  
|[WillChangeRecord](./willchangerecord-and-recordchangecomplete-events-ado.md)|Appelé avant qu’un ou plusieurs enregistrements (lignes) du **Recordset** soient modifiés.|  
|[WillChangeRecordset](./willchangerecordset-and-recordsetchangecomplete-events-ado.md)|Appelé avant qu’une opération en attente modifie le **Recordset**.|  
|[WillConnect](./willconnect-event-ado.md)|Appelé avant le démarrage d’une connexion.|  
|[WillExecute](./willexecute-event-ado.md)|Appelé juste avant qu’une commande en attente ne s’exécute sur cette connexion et donne à l’utilisateur la possibilité d’examiner et de modifier les paramètres d’exécution en attente.|  
|[WillMove](./willmove-and-movecomplete-events-ado.md)|L’événement **WillMove** est appelé *avant qu'* une opération en attente modifie la position actuelle dans l’ensemble **d’enregistrements**.|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ADO](./ado-api-reference.md)   
 [Collections ADO](./ado-collections.md)   
 [Propriétés dynamiques ADO](./ado-dynamic-properties.md)   
 [Constantes énumérées ADO](./ado-enumerated-constants.md)   
 [Annexe B : erreurs ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [Gestion des événements ADO](../../guide/data/handling-ado-events.md)   
 [Méthodes ADO](./ado-methods.md)   
 [Modèle objet ADO](./ado-object-model.md)   
 [Objets et interfaces ADO](./ado-objects-and-interfaces.md)   
 [Propriétés ADO](./ado-properties.md)