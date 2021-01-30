---
description: SET NULL, commande
title: SET NULL, commande | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- set nullSET NULL
ms.assetid: 410c5a6e-e957-4ecc-9e2d-e591cbc0bc4f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a190c913b2f8cc9d7a111a0c95d0408de2c1c6c2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208609"
---
# <a name="set-null-command"></a>SET NULL, commande
Détermine la façon dont les valeurs NULL sont prises en charge par les commandes ALTER TABLE-SQL, CREATE TABLE-SQL et INSERT-SQL.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET NULL ON | OFF  
```  
  
## <a name="arguments"></a>Arguments  
 ACTIVÉ  
 (Valeur par défaut pour le pilote ; la valeur par défaut de Visual FoxPro est OFF.) Spécifie que toutes les colonnes d’une table créée avec ALTER TABLE et CREATE TABLE autorisent les valeurs NULL. Vous pouvez substituer la prise en charge des valeurs NULL pour les colonnes de la table en incluant la clause NOT NULL dans les définitions des colonnes.  
  
 Spécifie également que INSERT-SQL insère des valeurs NULL dans toutes les colonnes qui ne sont pas incluses dans la clause INSERT-SQL VALUE. INSERT-SQL insère uniquement des valeurs NULL dans les colonnes qui autorisent les valeurs NULL.  
  
 OFF  
 Spécifie que toutes les colonnes d’une table créée avec ALTER TABLE et CREATE TABLE n’autorisent pas les valeurs NULL. Vous pouvez spécifier la prise en charge des valeurs NULL pour les colonnes de l’instruction ALTER TABLE et CREATE TABLE en incluant la clause NULL dans les définitions des colonnes.  
  
 Spécifie également que INSERT-SQL insère des valeurs vides dans toutes les colonnes qui ne sont pas incluses dans la clause INSERT-SQL VALUE.  
  
## <a name="remarks"></a>Notes  
 SET NULL affecte uniquement la prise en charge des valeurs NULL par ALTER TABLE, CREATE TABLE et INSERT-SQL. Les autres commandes ne sont pas affectées par la valeur NULL.  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE-commande SQL](../../odbc/microsoft/alter-table-sql-command.md)   
 [CREATE TABLE-commande SQL](../../odbc/microsoft/create-table-sql-command.md)   
 [INSERT, commande SQL](../../odbc/microsoft/insert-sql-command.md)
