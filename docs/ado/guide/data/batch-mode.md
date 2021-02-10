---
description: Mode Lot
title: Mode batch | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data updates [ADO], batch mode
- batch mode [ADO]
- updating data [ADO], batch mode
ms.assetid: 0cb548e0-fcb4-4c49-98c8-be287911f826
author: rothja
ms.author: jroth
ms.openlocfilehash: ee9e67527b109b36c7ded266086f9efea06e4381
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028024"
---
# <a name="batch-mode"></a>Mode Lot
Le mode batch est activé lorsque la propriété **LockType** est définie sur **adLockBatchOptimistic** et que la mise à jour par lot est prise en charge par le fournisseur. Certains paramètres de type de verrou ne sont pas disponibles en fonction de l’emplacement du curseur. Par exemple, un type de verrou pessimiste n’est pas disponible lorsque **CursorLocation** a la valeur **adUseClient**. À l’inverse, un fournisseur ne peut pas prendre en charge un verrou optimiste par lot lorsque l’emplacement du curseur se trouve sur le serveur. Vous devez utiliser la mise à jour par lot avec un curseur de jeu de clés ou statique uniquement.  
  
 La méthode **UpdateBatch** est utilisée pour envoyer les modifications de l’ensemble **d’enregistrements** contenues dans la mémoire tampon de copie sur le serveur pour mettre à jour la source de données. Dans la section suivante, nous allons ouvrir un **Recordset** en mode batch, apporter des modifications à la mémoire tampon de copie, puis envoyer nos modifications à la source de données à l’aide d’un appel à **UpdateBatch**.  
  
 Cette section contient les rubriques suivantes :  
  
-   [Envoi des mises à jour : UpdateBatch, méthode](./sending-the-updates-updatebatch-method.md)  
  
-   [Filtrage des enregistrements mis à jour](./filtering-for-updated-records.md)  
  
-   [Traitement des mises à jour ayant échoué](./dealing-with-failed-updates.md)  
  
-   [Détection et résolution des conflits](./detecting-and-resolving-conflicts.md)  
  
-   [Déconnexion et reconnexion du recordset](./disconnecting-and-reconnecting-the-recordset.md)  
  
-   [Mise à jour des résultats d’une jointure : table unique](./updating-joined-results-unique-table.md)