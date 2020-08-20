---
description: Fermeture du curseur
title: Fermeture du curseur | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- cursors [ODBC], closing
- closing cursors [ODBC]
ms.assetid: 4f19bf5e-6d8c-40ae-a975-cfd62a0790ec
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cd9465f0fc076a4f41dd8bf4cb3463ce5a47de84
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88461570"
---
# <a name="closing-the-cursor"></a>Fermeture du curseur
Lorsqu’une application a fini d’utiliser un curseur, elle appelle **SQLCloseCursor** pour fermer le curseur. Par exemple :  
  
```  
SQLCloseCursor(hstmt);  
```  
  
 Tant que l’application n’a pas fermé le curseur, l’instruction sur laquelle le curseur est ouvert ne peut pas être utilisée pour la plupart des autres opérations, telles que l’exécution d’une autre instruction SQL. Pour obtenir la liste complète des fonctions qui peuvent être appelées lorsqu’un curseur est ouvert, consultez [annexe B : tables de transition d’État ODBC](../../../odbc/reference/appendixes/appendix-b-odbc-state-transition-tables.md).  
  
> [!NOTE]  
>  Pour fermer un curseur, une application doit appeler **SQLCloseCursor**, et non **SQLCancel**.  
  
 Les curseurs restent ouverts jusqu’à ce qu’ils soient explicitement fermés, sauf lors de la validation ou de la restauration d’une transaction. dans ce cas, certaines sources de données ferment le curseur. En particulier, jusqu’à la fin du jeu de résultats, lorsque **SQLFetch** retourne SQL_NO_DATA, ne ferme pas un curseur. Même les curseurs sur des jeux de résultats vides (jeux de résultats créés lorsqu’une instruction s’est exécutée correctement mais qui n’a retourné aucune ligne) doit être explicitement fermé.
