---
description: SET DELETED, commande
title: DÉFINIR la commande DELETEd | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET DELETED command [ODBC]
ms.assetid: 6b5e0086-156d-471d-8e7f-6c5fa9686cd5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6447e2d0c77e1a161df58ca070fae8c624c196a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208639"
---
# <a name="set-deleted-command"></a>SET DELETED, commande
Spécifie si les enregistrements marqués pour la suppression sont traités et s’ils peuvent être utilisés dans d’autres commandes.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET DELETED ON | OFF  
```  
  
## <a name="arguments"></a>Arguments  
 ACTIVÉ  
 (Valeur par défaut pour le pilote ; la valeur par défaut de Visual FoxPro est OFF.) Spécifie que les commandes qui opèrent sur des enregistrements (y compris les enregistrements dans les tables associées) à l’aide d’une étendue ignorent les enregistrements marqués pour suppression.  
  
 OFF  
 Spécifie que les enregistrements marqués pour suppression sont accessibles par les commandes qui opèrent sur les enregistrements (y compris les enregistrements dans les tables associées) à l’aide d’une étendue.  
  
## <a name="remarks"></a>Notes  
 Les requêtes qui utilisent DELETEd () pour tester l’état des enregistrements peuvent être optimisées à l’aide de la technologie Visual FoxPro Rushmore si la table est indexée sur DELETEd ().  
  
> [!IMPORTANT]  
>  L’option SET DELETEd est ignorée si l’étendue par défaut de la commande est l’enregistrement en cours ou si vous incluez une étendue d’un seul enregistrement. INDEX ignore toujours SET DELETEd et indexe tous les enregistrements de la table.  
  
## <a name="see-also"></a>Voir aussi  
 [DELETE, commande SQL](../../odbc/microsoft/delete-sql-command.md)
