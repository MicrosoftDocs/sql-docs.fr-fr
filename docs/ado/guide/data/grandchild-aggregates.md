---
description: Agrégats de petits-enfants
title: Agrégats petit-enfant | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- grandchild aggregates [ADO]
- data shaping [ADO], grandchild aggregates
ms.assetid: 4162d35f-2ce1-4218-80a5-b6933348837e
author: rothja
ms.author: jroth
ms.openlocfilehash: a5cae4f996112d660564cb8efd3ea9b7e53c4f9f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033149"
---
# <a name="grandchild-aggregates"></a>Agrégats de petits-enfants
La colonne de chapitre créée dans une clause d’une commande SHAPE peut recevoir un *nom d’alias de chapitre* (généralement avec le mot clé As). Vous pouvez identifier toute colonne dans n’importe quel chapitre du **Recordset** mis en forme avec un nom complet qui identifie l’enfant contenant la colonne. Par exemple, si le chapitre parent, chap1, contient un chapitre enfant, Chap2, qui a une colonne Amount, AMT, le nom qualifié est chap1. Chap2. AMT. Le nom qualifié peut ensuite être utilisé comme argument pour l’une des fonctions d’agrégation (SUM, AVG, MAX, MIN, COUNT, STDEV ou ANY).  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de mise en forme des données](./data-shaping-example.md)