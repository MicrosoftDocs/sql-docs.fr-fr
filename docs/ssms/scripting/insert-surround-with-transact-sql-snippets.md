---
title: Insérer des extraits de code d'entourage (surround-with) Transact-SQL
description: Découvrez comment insérer un extrait de code d’entourage comme point de départ pour placer des instructions dans des blocs de code.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- snippets [Transact-SQL], surround with
- IntelliSense [SQL Server], surround with snippets
- Transact-SQL snippets, surround with
ms.assetid: 5b5a8c6c-968e-4361-a7f5-9e2ac186d927
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a62f995d8f75eef26c6e27afdfac06f365251796
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354631"
---
# <a name="insert-surround-with-transact-sql-snippets"></a>Insérer des extraits de code d'entourage (surround-with) Transact-SQL
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Un extrait de code d'entourage est un modèle pouvant servir de point de départ lors de l'intégration d'un jeu d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] dans un bloc BEGIN, IF ou WHILE.  
  
## <a name="inserting-surround-with-snippets"></a>Insertion d'extraits de code d'entourage  
 Les extraits de code d’entourage peuvent être lancés de trois manières : par le biais d’un raccourci clavier, du menu **Édition** et du menu contextuel.  
  
 Après avoir inséré l'extrait de code, vous devez modifier le texte de remplacement pour former une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] valide. Pour plus d’informations, consultez [Compléter des extraits de code Transact-SQL](./complete-transact-sql-snippets.md).  
  
#### <a name="to-insert-a-surround-with-snippet"></a>Pour insérer un extrait de code d'entourage  
  
1.  Dans la fenêtre de l’Éditeur de requête [!INCLUDE[ssDE](../../includes/ssde-md.md)] , sélectionnez le jeu d’instructions à inclure dans le bloc.  
  
2.  Utilisez l'une de ces trois méthodes pour afficher la liste des extraits de code d'entourage :  
  
    -   Tapez CTRL+K, CTRL+S.  
  
    -   Dans le menu **Édition** , pointez sur **IntelliSense**, puis sélectionnez la commande **Entourer de** .  
  
    -   Cliquez avec le bouton droit sur le texte sélectionné, puis sélectionnez la commande **Entourer de** dans le menu contextuel.  
  
3.  Sélectionnez le nom de l'extrait de code (BEGIN, IF ou WHILE) sur la liste à l'aide de la souris ou en tapant le nom de l'extrait de code et en appuyant sur TABULATION ou ENTRÉE.  
  
## <a name="see-also"></a>Voir aussi  
 [Insérer des extraits de code Transact-SQL](./insert-transact-sql-snippets.md)  
  
