---
description: CREATE INDEX, instruction - limitations
title: Limitations de l’instruction CREATe INDEX | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- CREATE INDEX statement limitations [ODBC]
- ODBC SQL grammar, CREATE INDEX statement limitations
ms.assetid: 832dcda1-e452-48e6-8adb-7fb33c4fb4ff
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 60952e7b9236c392e10bc155a19c5b147e011dbf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206172"
---
# <a name="create-index-statement-limitations"></a>CREATE INDEX, instruction - limitations
L’instruction CREATe INDEX n’est pas prise en charge pour Microsoft Excel ou les pilotes texte.  
  
 Un index peut être défini sur un maximum de 10 colonnes. Si plus de 10 colonnes sont incluses dans une instruction CREATe INDEX, l’index n’est pas reconnu et la table est traitée comme si aucun index n’avait été créé.  
  
 Le pilote dBASE ne peut pas créer d’index sur une colonne logique.  
  
 Lorsque le pilote dBASE est utilisé, vous pouvez améliorer le temps de réponse des fichiers volumineux en générant un index. MDX (ou. ndx) sur la colonne (champ) spécifiée dans les clauses WHERE d’une instruction SELECT. Les index. MDX existants seront appliqués automatiquement pour les opérateurs =, >, \<, > =, =< et between dans une clause WHERE, ainsi que dans les prédicats de jointure.  
  
 Lorsque le pilote dBASE est utilisé, l’index créé par une instruction CREATe UNIQUE INDEX est en fait non unique et des valeurs dupliquées peuvent être insérées dans la colonne indexée. Un seul enregistrement d’un ensemble avec des valeurs de clé identiques peut être ajouté à l’index.  
  
 Lorsque le pilote Paradox est utilisé, un index unique doit être défini sur un sous-ensemble contigu des colonnes d’une table, y compris la première colonne. Une table ne peut pas être mise à jour par le pilote Paradox si un index unique n’est pas défini sur la table ou lorsque le pilote Paradox est utilisé sans l’implémentation du Moteur de base de données Borland.
