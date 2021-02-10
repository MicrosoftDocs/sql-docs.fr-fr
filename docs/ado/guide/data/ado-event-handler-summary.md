---
description: Événements ADO Connection et Recordset
title: Résumé du gestionnaire d’événements ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- events [ADO], about event handlers
- event handlers [ADO]
ms.assetid: b34f4472-5e04-4a2c-ab64-38d6eca31a69
author: rothja
ms.author: jroth
ms.openlocfilehash: a447c149769d892149096c980b35e77e49de05da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028361"
---
# <a name="ado-connection-and-recordset-events"></a>Événements ADO Connection et Recordset
Deux objets ADO peuvent déclencher des événements : l’objet [Connection](../../reference/ado-api/connection-object-ado.md) et l’objet [Recordset](../../reference/ado-api/recordset-object-ado.md) . La famille **ConnectionEvent** concerne les opérations sur l’objet de **connexion** , et la famille **RecordsetEvent** concerne les opérations sur l’objet **Recordset** .

-   **Événements de connexion**: les événements sont émis lorsqu’une transaction sur une connexion commence, est validée ou est annulée ; lors de l’exécution d’une [commande](../../reference/ado-api/command-object-ado.md) ; Lorsqu’un avertissement se produit pendant une opération d' **événement de connexion** ; ou lorsqu’une **connexion** démarre ou se termine.

-   **Événements Recordset**: les événements sont émis autour des opérations d’extraction asynchrones, ainsi que lorsque vous parcourez les lignes d’un objet **Recordset** , que vous modifiez un champ dans une ligne d’un jeu d' **enregistrements**, que vous modifiez une ligne dans un **jeu d'** enregistrements, que vous ouvrez un **jeu d’enregistrements** avec un curseur côté serveur , que vous fermez un **jeu d’enregistrements** ou que vous apportez

 Les tableaux suivants résument les événements et leurs descriptions.

|ConnectionEvent|Description|
|---------------------|-----------------|
|[BeginTransComplete, CommitTransComplete, RollbackTransComplete](../../reference/ado-api/begintranscomplete-committranscomplete-and-rollbacktranscomplete-events-ado.md)|**Gestion des transactions** -notification signalant que la transaction en cours sur la connexion a démarré, validée ou restaurée.|
|[WillConnect](../../reference/ado-api/willconnect-event-ado.md), [ConnectComplete, Disconnect](../../reference/ado-api/connectcomplete-and-disconnect-events-ado.md)|**Gestion des connexions** : notification signalant que la connexion active va démarrer, a démarré ou est terminée.|
|[WillExecute](../../reference/ado-api/willexecute-event-ado.md), [ExecuteComplete](../../reference/ado-api/executecomplete-event-ado.md)|**Gestion** de l’exécution de la commande-notification indiquant que l’exécution de la commande active sur la connexion va démarrer ou s’est terminée.|
|[InfoMessage](../../reference/ado-api/infomessage-event-ado.md)|**Information** : notification indiquant qu’il existe des informations supplémentaires sur l’opération en cours.|

|RecordsetEvent|Description|
|--------------------|-----------------|
|[FetchProgress](../../reference/ado-api/fetchprogress-event-ado.md), [FetchComplete](../../reference/ado-api/fetchcomplete-event-ado.md)|**État de récupération** -notification de la progression d’une opération de récupération de données ou de la fin de l’opération de récupération. Ces événements sont uniquement disponibles si le **jeu d’enregistrements** a été ouvert à l’aide d’un curseur côté client.|
|[WillChangeField, FieldChangeComplete](../../reference/ado-api/willchangefield-and-fieldchangecomplete-events-ado.md)|**Gestion des modifications de champ** : notification indiquant que la valeur du champ actuel va changer ou a été modifiée.|
|[WillMove, MoveComplete](../../reference/ado-api/willmove-and-movecomplete-events-ado.md), [EndOfRecordset](../../reference/ado-api/endofrecordset-event-ado.md)|**Gestion** de la navigation-notification indiquant que la position de ligne actuelle dans un **Recordset** change, a changé ou a atteint la fin de l’ensemble d' **enregistrements**.|
|[WillChangeRecord, RecordChangeComplete](../../reference/ado-api/willchangerecord-and-recordchangecomplete-events-ado.md)|**Gestion des modifications de lignes** : notification indiquant qu’un contenu de la ligne actuelle de l’ensemble d' **enregistrements** sera modifié ou a changé.|
|[WillChangeRecordset, RecordsetChangeComplete](../../reference/ado-api/willchangerecordset-and-recordsetchangecomplete-events-ado.md)|**Gestion des modifications** de l’ensemble d’enregistrements-notification indiquant qu’un **objet du jeu d’enregistrements** actuel est modifié ou a changé.|

## <a name="see-also"></a>Voir aussi
 [Instanciation des événements ADO par langage](./ado-event-instantiation-by-language.md) [](../../reference/ado-api/ado-events.md) [paramètres d’événement](./event-parameters.md) ADO [Comment les gestionnaires d’événements fonctionnent ensemble](./how-event-handlers-work-together.md) [types d’événements](./types-of-events.md)