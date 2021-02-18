---
title: Modifier des scripts SQLCMD à l'aide de l'Éditeur de requête
description: Vous utilisez des scripts SQLCMD lorsque vous devez traiter des commandes Windows System et des instructions Transact-SQL dans le même script. Découvrez comment écrire et modifier des scripts SQLCMD à l’aide de l’éditeur de requête du moteur de base de données.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- scripts [SQL Server], SQLCMD scripts
- SQLCMD scripts
- modifying scripts
- Query Editor [Database Engine], SQLCMD scripts
- scripts [SQL Server], SQL Server Management Studio
ms.assetid: f77b866d-c330-47c9-9e74-0b8d8dff4b31
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 60c88bce82f6eb13d1441ded4cb815cecd0842ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354672"
---
# <a name="edit-sqlcmd-scripts-with-query-editor"></a>Modifier des scripts SQLCMD à l'aide de l'Éditeur de requête

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Grâce à l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , vous pouvez écrire et modifier des requêtes en tant que scripts SQLCMD. Vous utilisez des scripts SQLCMD lorsque vous devez traiter des commandes Windows System et des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] dans le même script.  
  
## <a name="sqlcmd-mode"></a>Mode SQLCMD  
 Pour utiliser l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] moteur de base de données afin d'écrire ou de modifier des scripts SQLCMD, vous devez activer le mode de script SQLCMD. Par défaut, il n'est pas activé dans l'Éditeur de requête. Vous pouvez l'activer en cliquant sur l'icône **Mode SQLCMD** dans la barre d'outils ou en sélectionnant **Mode SQLCMD** dans le menu **Requête** .  
  
> [!NOTE]  
>  L'activation du mode SQLCMD désactive IntelliSense et le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] dans l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 Dans l'Éditeur de requête, les scripts SQLCMD peuvent utiliser les mêmes fonctionnalités que tous les scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] . Ces fonctionnalités incluent les suivantes :  
  
-   codage en couleurs ;  
  
-   exécution des scripts ;  
  
-   Contrôle de la source  
  
-   analyse des scripts ;  
  
-   Showplan  
  
## <a name="enable-sqlcmd-scripting-in-query-editor"></a>Activation des scripts SQLCMD dans l'Éditeur de requête  
 Pour activer le script SQLCMD pour une fenêtre active de l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] moteur de base de données, utilisez la procédure suivante.  
  
#### <a name="to-switch-a-database-engine-query-editor-window-to-sqlcmd-mode"></a>Pour basculer l'Éditeur de requête du moteur de base de données en mode SQLCMD  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur le serveur et choisissez **Nouvelle requête** pour ouvrir une nouvelle fenêtre de l’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
2.  Dans le menu **Requête** , cliquez sur **Mode SQLCMD**.  
  
     L'éditeur de requête exécute les instructions **sqlcmd** dans son contexte.  
  
3.  Dans la barre d'outils **Éditeur SQL** , dans la liste **Bases de données disponibles** , sélectionnez [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
4.  Dans la fenêtre de l’éditeur de requête, tapez les deux instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes et l’instruction `!!DIR` **sqlcmd** :  
  
    ```  
    SELECT DISTINCT Type FROM Sales.SpecialOffer;  
    GO  
    !!DIR  
    GO  
    SELECT ProductCategoryID, Name FROM Production.ProductCategory;  
    GO  
    ```  
  
5.  Appuyez sur F5 pour exécuter toute la section d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] et MS-DOS.  
  
     Consultez les deux volets de résultats SQL générés par la première et la troisième instruction.  
  
6.  Dans le volet **Résultats** , cliquez sur l'onglet **Messages** pour afficher les messages des trois instructions :  
  
    -   (6 lignes affectées)  
  
    -   \<The directory information>  
  
    -   (4 lignes affectées)  
  
> [!IMPORTANT]  
>  Lorsque l'utilitaire **sqlcmd** est exécuté à partir de la ligne de commande, il permet une interaction totale avec le système d’exploitation. Lorsque vous utilisez l'Éditeur de requête en **Mode SQLCMD**, vous devez veiller à ne pas exécuter d'instructions interactives. L'Éditeur de requêtes ne peut pas répondre aux invites du système d'exploitation.  
  
 Pour plus d'informations sur l'exécution de SQLCMD, consultez [sqlcmd Utility](../../tools/sqlcmd-utility.md)ou suivez le didacticiel qui lui est consacré.  
  
## <a name="enable-sqlcmd-scripting-by-default"></a>Activation des scripts SQLCMD par défaut  
 Pour activer les scripts SQLCMD par défaut, dans le menu **Outils** , sélectionnez **Options**, développez **Exécution de la requête**, **SQL Server**, cliquez sur la page **Général** , puis activez la case à cocher **Par défaut, ouvrir les nouvelles requêtes en mode SQLCMD** .  
  
## <a name="writing-and-editing-sqlcmd-scripts"></a>Écriture et modification de scripts SQLCMD  
 Une fois le mode de scripts activé, vous pouvez écrire des commandes SQLCMD et des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] . Les règles suivantes s’appliquent :  
  
-   Les commandes SQLCMD doivent être la première instruction d'une ligne.  
  
-   Une seule commande SQLCMD est autorisée par ligne.  
  
-   Les commandes SQLCMD peuvent être précédées de commentaires ou d'espaces.  
  
-   Les commandes SQLCMD placées entre des caractères de commentaires ne sont pas exécutées.  
  
-   Les caractères de commentaires d'une seule ligne sont deux tirets (`--)` qui doivent être placés au début d'une ligne.  
  
-   Les commandes du système d'exploitation doivent être précédées de deux points d'exclamation (`!!`). La commande avec deux points d'exclamation entraîne l'exécution de l'instruction qui suit les points d'exclamation avec le processeur de commandes `cmd.exe` . Le texte situé après `!!` est transmis en tant que paramètre à `cmd.exe`. La ligne de commande finale s'exécute donc en tant que : `"%SystemRoot%\system32\cmd.exe /c <text after !!>"`.  
  
-   Pour distinguer clairement les commandes SQLCMD des commandes [!INCLUDE[tsql](../../includes/tsql-md.md)], toutes les commandes SQLCMD doivent être précédées d'un symbole deux-points (`:`).  
  
-   La commande `GO` peut être utilisée sans préfixe, ou bien précédée de `!!:`  
  
-   L’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] prend en charge les variables d’environnement et les variables définies dans un script SQLCMD, mais il ne prend pas en charge les variables SQLCMD intégrées ni les variables **osql** . Le traitement SQLCMD de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] respecte la casse pour les variables. Par exemple, PRINT '$ (COMPUTERNAME)' génère le bon résultat et PRINT '$ (ComputerName)' retourne une erreur.  
  
> [!CAUTION]
>  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] utilise [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]SqlClient pour les exécutions en mode normal et SQLCMD. Lorsqu'il est exécuté à partir de la ligne de commande, SQLCMD utilise le fournisseur OLE DB. Dans la mesure où des options par défaut peuvent s'appliquer, il est possible d'obtenir un comportement différent pendant l'exécution de la même requête en mode SQLCMD [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et dans l'utilitaire SQLCMD.  
  
## <a name="supported-sqlcmd-syntax"></a>Syntaxe SQLCMD prise en charge  
 L'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] prend en charge les mots clés de script SQLCMD suivants :  
  
 `[!!:]GO[count]`  
  
 `!! <command>`  
  
 `:exit(statement)`  
  
 `:Quit`  
  
 `:r <filename>`  
  
 `:setvar <var> <value>`  
  
 `:connect server[\instance] [-l login_timeout] [-U user [-P password]]`  
  
 `:on error [ignore|exit]`  
  
 `:error <filename>|stderr|stdout`  
  
 `:out <filename>|stderr|stdout`  
  
> [!NOTE]  
>  Pour `:error` et `:out`, `stderr` et `stdout` envoient la sortie dans l'onglet des messages.  
  
 Les commandes SQLCMD qui ne sont pas répertoriées ci-dessus ne sont pas prises en charge dans l'Éditeur de requête. Lorsqu'un script contenant des mots clés SQLCMD qui ne sont pas pris en charge est exécuté, l'Éditeur de requête envoie un message « Commande > ignorée *\<ignored command*> » à la destination pour chaque mot clé non pris en charge. Le script s'exécute correctement mais les commandes non prises en charge sont ignorées.  
  
> [!CAUTION]  
>  Étant donné que vous ne démarrez pas SQLCMD à partir de la ligne de commande, l'exécution de l'Éditeur de requête en mode SQLCMD est quelque peu limitée. Vous ne pouvez pas transmettre des paramètres de ligne de commande tels que des variables, et, comme l'éditeur de requête n'a pas la possibilité de répondre aux invites du système d'exploitation, vous ne devez pas exécuter d'instructions interactives.  
  
## <a name="color-coding-in-sqlcmd-scripts"></a>Codage en couleurs dans les scripts SQLCMD  
 Lorsque les scripts SQLCMD sont activés, les scripts sont codés en couleurs. Le codage en couleurs des mots clés [!INCLUDE[tsql](../../includes/tsql-md.md)] reste inchangé. Les commandes SQLCMD sont présentées avec un arrière-plan ombré.  
  
## <a name="example"></a>Exemple  
 L’exemple ci-après utilise une instruction **sqlcmd** pour créer un fichier de sortie nommé testoutput.txt et exécute deux instructions SELECT [!INCLUDE[tsql](../../includes/tsql-md.md)] ainsi qu’une commande du système d’exploitation (pour imprimer le répertoire actuel). Le fichier résultant contient la sortie du message provenant de l'instruction `DIR` , suivie des résultats des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
```  
:out C:\testoutput.txt  
SELECT @@VERSION As 'Server Version'  
!!DIR  
!!:GO  
SELECT @@SERVERNAME AS 'Server Name'  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Utilitaire sqlcmd](../../tools/sqlcmd-utility.md)  
  
  
