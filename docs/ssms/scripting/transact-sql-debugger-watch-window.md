---
title: Espion (fenêtre)
description: En savoir plus sur les fenêtres Espion (jusqu’à quatre à la fois), qui affichent des informations sur les expressions que vous sélectionnez. Les informations s’affichent uniquement en mode débogage.
titleSuffix: T-SQL Debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Watch Window [Transact-SQL]
ms.assetid: 23f3baa4-14c2-4262-92f7-3f43fcfa0436
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.reviewer: ''
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 79fcf4a8f3591b0f6bde54c684bae09e32826aa4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346098"
---
# <a name="transact-sql-debugger---watch-window"></a>Débogueur Transact-SQL - Fenêtre Espion

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La fenêtre **Espion** affiche des informations sur les expressions que vous avez sélectionnées. Il peut y avoir jusqu’à quatre fenêtres Espion : **Espion 1**, **Espion 2, Espion 3** et **Espion 4**. Les expressions sont évaluées dans l’étendue du frame de pile des appels actuellement sélectionné dans la fenêtre **Pile des appels** . Vous devez être en mode débogage pour surveiller les variables et les expressions.  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="task-list"></a>Liste des tâches

**Pour accéder aux fenêtres Espion**  
  
-   Dans le menu **Déboguer** , cliquez sur **Fenêtres**, sur **Espion** puis sur **Espion 1**, **Espion 2, Espion 3** ou **Espion 4**.  
  
 **Pour modifier la valeur d'une expression**  
  
-   Cliquez avec le bouton droit sur l’expression, puis sélectionnez **Modifier la valeur**.  
  
## <a name="columns"></a>Colonnes  
 **Nom**  
 Expressions répertoriées par le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] . Les expressions suivantes sont prises en charge :  
  
-   variables ;  
  
-   Paramètres.  
  
-   Fonctions système dont le nom commence par @@.  
  
-   Expressions générées par l’application d’opérateurs à une ou plusieurs variables, un ou plusieurs paramètres ou une ou plusieurs fonctions système, comme @IntegerCounter + 1 ou FirstName + LastName.  
  
-   Instructions Transact-SQL qui retournent une valeur unique, comme : SELECT CharacterCol FROM MyTable WHERE PrimaryKey = 1.  
  
 **Valeur**  
 Affiche la valeur retournée après que le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] a évalué l’expression spécifiée dans **Nom**.  
  
 Si la longueur d'une expression dépasse la largeur de la colonne **Valeur** , une info-bulle affiche la valeur complète lorsque vous placez le pointeur sur la cellule **Valeur** de cette expression.  
  
 La présence d'une icône de loupe dans une cellule **Valeur** indique que le visualiseur du débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] est disponible. Dans la liste, vous pouvez spécifier **Visualiseur de texte**, **Visualiseur XML** ou **Visualiseur HTML**. Pour démarrer un visualiseur du débogueur, cliquez sur l'icône de loupe. Le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] ouvre une boîte de dialogue qui affiche les données dans un format adapté au type de données.  
  
 **Type**  
 Affiche le type de données de l'expression.  
  
## <a name="see-also"></a>Voir aussi  
 [Débogueur Transact-SQL](./transact-sql-debugger.md)   
 [Informations du débogueur Transact-SQL](./transact-sql-debugger-information.md)   
 [Variables locales (fenêtre)](./transact-sql-debugger-locals-window.md)   
 [Fenêtre Pile des appels](./transact-sql-debugger-call-stack-window.md)   
 [Boîte de dialogue Espion express](./transact-sql-debugger-quickwatch-dialog-box.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)