---
description: Traitement des mises à jour ayant échoué
title: Traitement des mises à jour ayant échoué | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- updates [ADO], dealing with failed updates
ms.assetid: 299c37bd-19ff-4261-8571-b9665687e075
author: rothja
ms.author: jroth
ms.openlocfilehash: a5ce736c8dd24f2398d7c8c374be260080c32e11
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037639"
---
# <a name="dealing-with-failed-updates"></a>Traitement des mises à jour ayant échoué
Lorsqu’une mise à jour se termine avec des erreurs, la façon dont vous résolvez les erreurs dépend de la nature et de la gravité des erreurs et de la logique de votre application. Toutefois, si la base de données est partagée avec d’autres utilisateurs, une erreur classique est que quelqu’un d’autre modifie le champ avant. Ce type d’erreur est appelé un conflit. ADO détecte cette situation et signale une erreur.  
  
## <a name="remarks"></a>Notes  
 Si des erreurs de mise à jour se produisent, elles sont interceptées dans une routine de gestion des erreurs. Filtrez le Recordset avec la constante adFilterConflictingRecords afin que seules les lignes en conflit soient visibles. Dans cet exemple, la stratégie de résolution des erreurs consiste simplement à imprimer le prénom et le nom de famille de l’auteur (au_fname et au_lname).  
  
 Le code permettant d’alerter l’utilisateur en cas de conflit de mise à jour ressemble à ceci :  
  
```  
objRs.Filter = adFilterConflictingRecords  
objRs.MoveFirst  
Do While Not objRst.EOF  
   Debug.Print "Conflict: Name =  "; objRs!au_fname; " "; objRs!au_lname  
   objRs.MoveNext  
Loop  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Mode Batch](./batch-mode.md)