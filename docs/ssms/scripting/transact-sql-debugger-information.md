---
title: Informations du débogueur Transact-SQL
description: Découvrez comment afficher le résultat du débogueur Transact-SQL, incluant des informations telles que les piles d’appels, les threads, les points d’arrêt, le code, les variables et les commandes.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, Locals Window
- Transact-SQL debugger, Watch Window
- Transact-SQL debugger, Threads Window
- Transact-SQL debugger, Call Stack Window
- Transact-SQL debugger, QuickWatch
- Transact-SQL debugger, viewing information
ms.assetid: b99819cc-f388-41a1-b304-36e78ce24147
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 412ae1788542d4fa00df6a17df1b6450f7605851
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341002"
---
# <a name="transact-sql-debugger---information"></a>Débogueur Transact-SQL - Informations

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Chaque fois que le débogueur suspend l'exécution du code au niveau d'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] spécifique, vous pouvez utiliser les différentes fenêtres du débogueur pour afficher l'état d'exécution actuel. 

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="debugger-windows"></a>Fenêtres du débogueur  

En mode débogage, le débogueur ouvre deux fenêtres en bas de la fenêtre principale de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . Le débogueur affiche toutes ses informations dans ces deux fenêtres. Chaque fenêtre du débogueur contient des onglets que vous pouvez sélectionner pour contrôler le jeu d'informations à afficher dans la fenêtre. La fenêtre gauche du débogueur contient les onglets **Variables locales**, **Espion 1**, **Espion 2**, **Espion 3** et **Espion 4** . La fenêtre droite du débogueur contient les onglets **Pile des appels**, **Threads**, **Points d’arrêt**, **Fenêtre de commande** et **Sortie** .  
  
> [!NOTE]  
>  Les descriptions précédentes s'appliquent aux emplacements par défaut des fenêtres du débogueur. Vous pouvez faire glisser un onglet pour le déplacer dans une autre fenêtre, ou vous pouvez détacher un onglet pour créer une nouvelle fenêtre que vous pouvez positionner où bon vous semble.  
  
 Par défaut, ces onglets ou fenêtres ne sont pas tous actifs. Vous pouvez ouvrir une fenêtre particulière en procédant de l'une des manières suivantes :  
  
-   Dans le menu **Déboguer** , cliquez sur **Fenêtres**, puis sélectionnez la fenêtre de votre choix.  
  
-   Dans la barre d’outils **Déboguer** , cliquez sur **Points d’arrêt**, puis sélectionnez la fenêtre désirée.  
  
## <a name="transact-sql-expressions"></a>Expressions Transact-SQL  
 Les expressions sont des clauses [!INCLUDE[tsql](../../includes/tsql-md.md)] qui prennent une valeur scalaire unique, par exemple des variables ou des paramètres. La fenêtre gauche du débogueur peut afficher les valeurs de données qui sont actuellement affectées à des expressions dans cinq onglets ou fenêtres au maximum : **Variables locales, Espion 1**, **Espion 2**, **Espion 3** et **Espion 4**.  
  
 La fenêtre **Variables locales** affiche des informations sur les variables locales dans l’étendue actuelle du débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] . L’ensemble d’expressions répertorié dans la fenêtre **Variables locales** change à mesure que le débogueur exécute les différentes parties du code.  
  
 Les expressions figurant dans **Espion Express** et les quatre fenêtres **Espion** ne servent pas uniquement à indiquer l’identificateur d’une variable. Vous pouvez spécifier une expression [!INCLUDE[tsql](../../includes/tsql-md.md)] qui prend une valeur unique, comme l'ajout d'un nombre à une variable, ou une instruction SELECT qui prend une valeur unique. Voici quelques exemples :  
  
-   Nom d’une variable de type, comme @IntegerCounter.  
  
-   Opération arithmétique sur une variable, par exemple @IntegerCounter + 1.  
  
-   Opération de chaîne sur deux variables de caractères, par exemple @FirstName+ @LastName.  
  
-   Une instruction SELECT qui retourne une seule valeur, par exemple SELECT CharCol FROM MyTable WHERE PrimaryKey = 1.  
  
 Vous pouvez utiliser la fenêtre **Espion express** pour afficher la valeur d’une expression [!INCLUDE[tsql](../../includes/tsql-md.md)] , puis enregistrer cette expression dans une fenêtre **Espion** . Pour sélectionner une expression dans la fenêtre **Espion express**, sélectionnez ou entrez le nom de l’expression dans la zone **Expression** .  
  
 Les quatre fenêtres **Espion** affichent des informations sur les variables et les expressions que vous avez sélectionnées. L’ensemble d’expressions répertorié dans la fenêtre **Variables locales** reste inchangé, sauf si vous modifiez ou supprimez des expressions dans la liste.  
  
 Pour ajouter une expression dans une fenêtre **Espion** , vous pouvez sélectionner **Ajouter un espion** dans la boîte de dialogue **Espion express** ou entrer le nom de l’expression dans la colonne **Nom** d’une ligne vide dans une fenêtre **Espion** .  
  
 Vous pouvez définir les valeurs de données pour des variables dans les fenêtres **Variables locales**, **Espion** ou **Espion express** en cliquant avec le bouton droit sur la ligne, puis en sélectionnant **Modifier la valeur**. Les colonnes **Valeur** dans la fenêtre **Variables locales** , la fenêtre **Espion** et la boîte de dialogue **Espion express** prennent toutes en charge les visualiseurs de données texte, XML et HTML. Les visualiseurs sont représentés par une bulle d’informations en forme de loupe à droite de la colonne **Valeurs** . Vous pouvez utiliser les visualiseurs pour afficher des valeurs de données texte, XML ou HTML dans des affichages qui correspondent aux types de données, par exemple des fichiers XML dans une fenêtre de navigateur.  
  
 En mode débogage, lorsque vous déplacez le pointeur de la souris sur un identificateur, une fenêtre contextuelle **Info express** affiche le nom de l’expression et sa valeur actuelle. Pour plus d’informations, consultez [Info express &#40;IntelliSense&#41;](./quick-info-intellisense.md).  
  
## <a name="breakpoints"></a>Points d’arrêt  
 Vous pouvez utiliser la fenêtre **Points d’arrêt** pour afficher et gérer les points d’arrêt actuellement définis. Pour plus d’informations, consultez [Exécuter pas à pas du code Transact-SQL](./step-through-transact-sql-code.md).  
  
## <a name="call-stacks"></a>Pile des appels  
 La fenêtre **Pile des appels** affiche l’emplacement d’exécution actuel et des informations sur la façon dont l’exécution a atteint l’emplacement d’exécution actuel à partir de la fenêtre de l’éditeur d’origine via des modules [!INCLUDE[tsql](../../includes/tsql-md.md)] (fonctions, procédures stockées ou déclencheurs). Chaque ligne dans la fenêtre **Pile des appels** est appelée un « frame de pile » et représente l’un des éléments suivants :  
  
-   l'emplacement d'exécution actuel ;  
  
-   un appel d'un module à un autre ;  
  
-   un appel d'une fenêtre d'éditeur à un module [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 La pile est ordonnée dans le sens inverse de l'appel des modules. L'emplacement d'exécution actuel se trouve en haut de la pile, et l'appel d'origine en bas. Une flèche jaune dans la marge gauche du frame de pile identifie le frame dans lequel le débogueur a suspendu l'exécution.  
  
 La colonne **Nom** enregistre les informations suivantes :  
  
-   le module source contenant la ligne de code ayant appelé le niveau suivant ;  
  
-   la ligne de code ayant appelé le module suivant dans la pile.  
  
-   Si l'appel est allé à une procédure stockée ou à une fonction ayant accepté des paramètres, les noms, types de données et valeurs de tous les paramètres sont également répertoriés.  
  
 Les expressions dans les fenêtres **Variables locales**, **Espion** et **Espion express** sont évaluées pour le frame de pile actuel. Par défaut, le frame de pile actuel est le premier frame de la pile, où le débogueur a suspendu l'exécution. Lorsque vous spécifiez un autre frame de pile en tant que frame actuel, les expressions dans les fenêtres **Variables locales**, **Espion** et **Espion express** sont réévaluées pour le nouveau frame de pile. Vous pouvez modifier le frame de pile actuel en double-cliquant sur un frame ou en cliquant sur un frame et en sélectionnant **Basculer vers le frame**. À ce stade, les expressions dans les fenêtres **Variables locales**, **Espion** et **Espion express** sont réévaluées pour le nouveau frame. Lorsque le frame de pile actuel n'est pas le premier frame de la pile, une flèche verte dans la marge gauche du frame de pile identifie le frame de pile actuel.  
  
 Lorsque vous cliquez avec le bouton droit sur un frame de pile et que vous sélectionnez **Atteindre le code source**, le code pour ce frame est affiché dans une fenêtre de l’éditeur de requête. Toutefois, ce frame ne devient pas le frame actuel, et le contenu des fenêtres **Variables locales**, **Espion** et **Espion express** n’est pas modifié.  
  
## <a name="system-information-and-transact-sql-results"></a>Informations système et résultats Transact-SQL  
 Le débogueur répertorie son état et des messages d’événements dans la fenêtre **Sortie** . Ces informations indiquent notamment à quel moment le débogueur se joint à d'autres processus et à quel moment les threads du débogueur se terminent.  
  
 En mode débogage, les onglets **Résultats** et **Messages** sont toujours actifs dans l’éditeur de requête. L’onglet **Résultats** continue à afficher les jeux de résultats des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui sont exécutées au cours d’une session de débogage. L’onglet **Messages** continue à afficher des messages système, tels que *xx* lignes affectées et la sortie des instructions PRINT et RAISERROR.  
  
## <a name="see-also"></a>Voir aussi  
 [Variables locales (fenêtre)](./transact-sql-debugger-locals-window.md)   
 [Espion (fenêtre)](./transact-sql-debugger-watch-window.md)   
 [Boîte de dialogue Espion express](./transact-sql-debugger-quickwatch-dialog-box.md)   
 [Fenêtre Points d'arrêt](./transact-sql-debugger-breakpoints-window.md)   
 [Fenêtre Pile des appels](./transact-sql-debugger-call-stack-window.md)   
 [Fenêtre Threads](./transact-sql-debugger-threads-window.md)   
 [Fenêtre Sortie](./transact-sql-debugger-output-window.md)   
 [Débogueur Transact-SQL](./transact-sql-debugger.md)  
  
