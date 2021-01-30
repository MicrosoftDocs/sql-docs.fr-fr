---
description: INSERT, commande SQL
title: Commande INSERT-SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- INSERT [ODBC]
ms.assetid: 9b648198-349f-46f6-b869-13d129945971
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ca0342aa083fbcb34ce05b8925baa19da3018b17
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194581"
---
# <a name="insert---sql-command"></a>INSERT, commande SQL
Ajoute un enregistrement à la fin d’une table qui contient les valeurs de champ spécifiées.  
  
 Le pilote ODBC Visual FoxPro prend en charge la syntaxe du langage Visual FoxPro natif pour cette commande. Pour obtenir des informations spécifiques au pilote, consultez les notes.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
INSERT INTO dbf_name [(fname1 [, fname2, ...])]  
   VALUES (eExpression1 [, eExpression2, ...])  
```  
  
## <a name="arguments"></a>Arguments  
 INSÉRER dans *dbf_name*  
 Spécifie le nom de la table à laquelle le nouvel enregistrement est ajouté. *dbf_name* pouvez inclure un chemin d’accès et peut être une expression de nom.  
  
 Si la table que vous spécifiez n’est pas ouverte, elle est ouverte exclusivement dans une nouvelle zone de travail et le nouvel enregistrement est ajouté à la table. La nouvelle zone de travail n’est pas sélectionnée. la zone de travail active reste sélectionnée.  
  
 Si la table que vous spécifiez est ouverte, INSERT ajoute le nouvel enregistrement à la table. Si la table est ouverte dans une zone de travail autre que la zone de travail en cours, elle n’est pas sélectionnée après l’ajout de l’enregistrement ; la zone de travail active reste sélectionnée.  
  
 [( *fname1*[, *fname2*[,...]])]  
 Spécifie, dans le nouvel enregistrement, les noms des champs dans lesquels les valeurs sont insérées.  
  
 VALEURS ( *eExpression1*[, *eExpression2*[,...]])  
 Spécifie les valeurs de champ insérées dans le nouvel enregistrement. Si vous omettez les noms de champs, vous devez spécifier les valeurs de champ dans l’ordre défini par la structure de la table.  
  
## <a name="remarks"></a>Notes  
 Le nouvel enregistrement contient les données figurant dans la clause VALUEs.  
  
## <a name="driver-remarks"></a>Remarques sur le pilote  
 Lorsque votre application envoie l’instruction SQL ODBC INSERT à la source de données, le pilote ODBC Visual FoxPro convertit la commande en commande Visual FoxProINSERT sans traduction.  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE TABLE-commande SQL](../../odbc/microsoft/create-table-sql-command.md)   
 [SELECT, commande SQL](../../odbc/microsoft/select-sql-command.md)
