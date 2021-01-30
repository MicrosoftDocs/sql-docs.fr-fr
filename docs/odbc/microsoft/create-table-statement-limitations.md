---
description: CREATE TABLE, instruction - limitations
title: CREATE TABLE limitations des instructions | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- CREATE TABLE statement limitations [ODBC]
- ODBC SQL grammar, CREATE TABLE statement limitations
ms.assetid: c5067855-20c9-456f-8d63-f375b4297f2e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 94b72930fc33c3abbf3920d1290811d0379413c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183194"
---
# <a name="create-table-statement-limitations"></a>CREATE TABLE, instruction - limitations
Lorsque Microsoft Access, Microsoft Excel ou Paradoxdriver est utilisé, et que la longueur d’une colonne de texte ou binaire n’est pas spécifiée (ou est spécifiée comme 0), la longueur de la colonne est définie sur 255.  
  
 Lorsque le pilote dBASE est utilisé et que la longueur d’une colonne de texte ou binaire n’est pas spécifiée (ou est spécifiée comme 0), la longueur de la colonne est définie sur 254.  
  
 Un maximum de 255 colonnes est pris en charge.  
  
 Lorsque le pilote Microsoft Excel est utilisé sur une source de données MicrosoftExcel 5,0, 7,0 ou 97, une feuille de calcul ne peut pas être créée avec le même nom qu’une feuille de calcul précédemment supprimée. Lorsque le pilote Microsoft Excel est utilisé pour accéder à une feuille de calcul version 5,0, 7,0 ou 97, une instruction DROP TABLE efface la feuille de calcul, mais ne supprime pas le nom de la feuille de calcul.  
  
 Lorsque le pilote Paradox est utilisé, les colonnes ne peuvent pas être ajoutées une fois qu’un index a été défini sur une table. Si la première colonne de la liste d’arguments d’une instruction CREATE TABLE crée un index, une deuxième colonne ne peut pas être incluse dans la liste d’arguments.
