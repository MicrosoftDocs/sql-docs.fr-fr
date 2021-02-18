---
title: Utiliser l'utilitaire sqlcmd
description: Découvrez comment utiliser l’utilitaire sqlcmd pour une exécution interactive ad hoc de scripts et d’instructions Transact-SQL et pour des tâches de script Transact-SQL.
ms.custom: seo-lt-2019
ms.date: 06/06/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- Transact-SQL statements, executing
- command prompt utilities [SQL Server], sqlcmd
- statements [SQL Server], executing
- sqlcmd utility, about sqlcmd utility
ms.assetid: 3ec89119-7314-43ef-9e91-12e72bb63d62
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6a1be2303d4496957f7352d4b103b548902b48d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339383"
---
# <a name="sqlcmd---use-the-utility"></a>sqlcmd - Utiliser l’utilitaire
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  L’utilitaire **sqlcmd** est un utilitaire de ligne de commande destiné à l’exécution ad hoc et interactive des instructions et des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] , et à l’automatisation des tâches de script [!INCLUDE[tsql](../../includes/tsql-md.md)] . Pour utiliser **sqlcmd** de façon interactive ou pour créer des fichiers de script destinés à être exécutés avec **sqlcmd**, les utilisateurs doivent connaître [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'utilitaire **sqlcmd** est généralement utilisé des façons suivantes :  
  
-   Les utilisateurs entrent les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] de façon interactive comme s’ils travaillaient à partir de l’invite de commandes. Les résultats s'affichent dans l'invite de commandes. Pour ouvrir une fenêtre d’invite de commandes, entrez « cmd » dans la zone de recherche de Windows, puis cliquez sur **Invite de commandes**. À l'invite de commandes, tapez **sqlcmd** suivi d'une liste des options de votre choix. Pour obtenir une liste complète des options prises en charge par l’utilitaire **sqlcmd**, consultez [Utilitaire sqlcmd](../../tools/sqlcmd-utility.md).  
  
-   Les utilisateurs soumettent un travail **sqlcmd** soit en spécifiant une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] unique à exécuter, soit en indiquant à l'utilitaire un fichier texte contenant les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] à exécuter. La sortie est généralement envoyée dans un fichier texte, mais elle peut aussi être affichée à l’invite de commandes.  
  
-   [Mode SQLCMD](./edit-sqlcmd-scripts-with-query-editor.md) dans l'Éditeur de requête [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
-   SMO (SQL Server Management Objects)  
  
-   Travaux CmdExec de SQL Server Agent.  
  
## <a name="typically-used-sqlcmd-options"></a>Options sqlcmd généralement utilisées  
  
-   L’option de serveur ( **-S**) identifie l’instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle l’utilitaire **sqlcmd** se connecte.  
  
-   Les options d’authentification ( **-E**, **-U** et **-P**) définissent les informations d’identification qu’utilise **sqlcmd** pour se connecter à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **REMARQUE :** L’option **-E** est la valeur par défaut et n’a pas besoin d’être spécifiée.  
  
-   Les options d’entrée ( **-Q**, **-q** et **-i**) identifient l’emplacement de l’entrée dans **sqlcmd**.  
  
-   L’option de sortie ( **-o**) spécifie le fichier dans lequel **sqlcmd** place sa sortie.  
  
## <a name="connect-to-the-sqlcmd-utility"></a>Se connecter à l’utilitaire sqlcmd  
  
-   Connexion à une instance par défaut à l'aide de l'authentification Windows pour exécuter de manière interactive des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
    ```  
    sqlcmd -S <ComputerName>  
    ```  
  
    > **REMARQUE :** dans l’exemple précédent, **-E** n’est pas définie, car il s’agit de l’option par défaut et **sqlcmd** se connecte à l’instance par défaut en utilisant l’authentification Windows.  
  
-   Connexion à une instance nommée à l'aide de l'authentification Windows pour exécuter de manière interactive des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
    ```  
    sqlcmd -S <ComputerName>\<InstanceName>  
    ```  
  
     or  
  
    ```  
    sqlcmd -S .\<InstanceName>  
    ```  
  
-   Connexion à une instance nommée en utilisant l'authentification Windows et définition des fichiers d'entrée et de sortie :  
  
    ```  
    sqlcmd -S <ComputerName>\<InstanceName> -i <MyScript.sql> -o <MyOutput.rpt>  
    ```  
  
-   Connexion à l'instance par défaut sur l'ordinateur local à l'aide de l'authentification Windows, exécution d'une requête et maintien de **sqlcmd** actif à la fin de la requête :  
  
    ```  
    sqlcmd -q "SELECT * FROM AdventureWorks2012.Person.Person"  
    ```  
  
-   Connexion à l'instance par défaut sur l'ordinateur local à l'aide de l'authentification Windows, exécution d'une requête, envoi de la sortie vers un fichier et fin de l'exécution de **sqlcmd** à la fin de la requête :  
  
    ```  
    sqlcmd -Q "SELECT * FROM AdventureWorks2012.Person.Person" -o MyOutput.txt  
    ```  
  
-   Connexion à une instance nommée à l'aide de l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour exécuter des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] de manière interactive, avec **sqlcmd** demandant un mot de passe :  
  
    ```  
    sqlcmd -U MyLogin -S <ComputerName>\<InstanceName>  
    ```  
  
    > **CONSEIL !!!** Pour obtenir une liste des options prises en charge par l’utilitaire **sqlcmd** , exécutez `sqlcmd -?`.  
  
## <a name="run-transact-sql-statements-interactively-by-using-sqlcmd"></a>Exécuter des instructions Transact-SQL de manière interactive à l’aide de sqlcmd  
 Vous pouvez utiliser l'utilitaire **sqlcmd** interactivement pour exécuter des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] dans une fenêtre d'invite de commandes. Pour exécuter interactivement des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] à l’aide de **sqlcmd**, exécutez l’utilitaire sans utiliser les options **-Q**, **-q**, **-Z** ou **-i** pour spécifier des fichiers ou des requêtes d’entrée. Par exemple :  
  
 `sqlcmd -S <ComputerName>\<InstanceName>`  
  
 Lorsque la commande est exécutée sans fichiers ou requêtes d'entrée, **sqlcmd** se connecte à l'instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et affiche ensuite une nouvelle ligne comportant un `1>` suivi d'un trait de soulignement clignotant, appelé invite **sqlcmd** . Le `1` signifie qu'il s'agit de la première ligne d'une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] et l'invite **sqlcmd** représente le point à partir duquel l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] commencera lorsque vous la taperez.  
  
 À l'invite **sqlcmd** , vous pouvez taper à la fois des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] et des commandes **sqlcmd** , telles que **GO** et **EXIT**. Chaque instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] est placée dans une mémoire tampon, appelée cache d'instruction. Ces instructions sont envoyées à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dès lors que vous avez tapé la commande **GO** et appuyé sur la touche ENTRÉE. Pour quitter **sqlcmd**, tapez **EXIT** ou **QUIT** au début d'une nouvelle ligne.  
  
 Pour effacer le cache d'instruction, tapez **:RESET**. Taper **^C** provoque la fin de **sqlcmd** . **^C** peut également être utilisé pour arrêter l'exécution du cache d'instruction après la saisie d'une commande **GO** .  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] entrées dans une session interactive peuvent être modifiées en entrant la commande **:ED** et l'invite **sqlcmd** . L'éditeur s'ouvre et après avoir modifié l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] et refermé l'éditeur, l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] révisée s'affiche dans la fenêtre de commandes. Entrez **GO** pour exécuter l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] révisée.  
  
## <a name="quoted-strings"></a>Chaînes entre guillemets  
 Les caractères entourés par des guillemets sont utilisés sans autre prétraitement, à l'exception des guillemets insérés au sein d'une chaîne en entrant deux guillemets consécutifs. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] traite cette séquence de caractères comme un seul guillemet. (La traduction s'effectue toutefois sur le serveur). Les variables des scripts ne sont pas développées lorsqu'elles apparaissent au sein d'une chaîne.  
  
 Par exemple :  
  
 `sqlcmd`  
  
 `PRINT "Length: 5"" 7'";`  
  
 `GO`  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `Length: 5" 7'`  
  
## <a name="strings-that-span-multiple-lines"></a>Chaînes qui s’étendent sur plusieurs lignes  
 **sqlcmd** prend en charge les scripts comportant des chaînes qui s'étendent sur plusieurs lignes. Par exemple, l'instruction `SELECT` suivante s'étend sur plusieurs lignes mais constitue une seule chaîne exécutée lorsque vous appuyez sur la touche ENTRÉE après avoir tapé `GO`.  
  
 `SELECT First line`  
  
 `FROM Second line`  
  
 `WHERE Third line;`  
  
 `GO`  
  
## <a name="interactive-sqlcmd-example"></a>Exemple d’utilisation interactive de sqlcmd  
 Il s'agit d'un exemple de ce que vous voyez lorsque vous exécutez **sqlcmd** interactivement.  
  
 Lorsque vous ouvrez une fenêtre d'invite de commandes, elle ne comporte qu'une seule ligne similaire à la ligne suivante :  
  
 `C:\> _`  
  
 Cela signifie que le dossier `C:\` est le dossier en cours et que si vous spécifiez un nom de fichier, Windows recherchera le fichier dans ce dossier.  
  
 Tapez **sqlcmd** pour vous connecter à l'instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] située sur l'ordinateur local. Le contenu de la fenêtre d'invite de commandes sera alors :  
  
 `C:\>sqlcmd`  
  
 `1> _`  
  
 Cela signifie que vous vous êtes connecté à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et que `sqlcmd` est maintenant prêt à accepter des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ainsi que des commandes `sqlcmd` . Le trait de soulignement clignotant situé après `1>` est l'invite de `sqlcmd` qui marque l'emplacement où les instructions et les commandes que vous tapez sont affichées. À présent, tapez **USE AdventureWorks2012** et appuyez sur Entrée, puis tapez **GO** et appuyez sur Entrée. La fenêtre d'invite de commandes affiche les éléments suivants :  
  
 `sqlcmd`  
  
 `USE AdventureWorks2012;`  
  
 `GO`  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `Changed database context to 'AdventureWorks2012'.`  
  
 `1> _`  
  
 Appuyer sur la touche ENTRÉE après avoir tapé `USE AdventureWorks2012` correspond à demander à `sqlcmd` de commencer une nouvelle ligne. Le fait d'appuyer sur la touche ENTRÉE après avoir tapé `GO,` revient à demander à `sqlcmd` d'envoyer l'instruction `USE AdventureWorks2012` à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. `sqlcmd` retourne alors un message pour indiquer que l'instruction `USE` a correctement abouti et affiche une nouvelle invite `1>` qui vous signale que vous pouvez entrer une nouvelle instruction ou une nouvelle commande.  
  
 L'exemple suivant affiche le contenu de la fenêtre d'invite de commandes si vous tapez une instruction `SELECT` , une commande `GO` pour exécuter `SELECT`et une commande `EXIT` pour quitter `sqlcmd`:  
  
 `sqlcmd`  
  
 `USE AdventureWorks2012;`  
  
 `GO`  
  
 `SELECT TOP (3) BusinessEntityID, FirstName, LastName`  
  
 `FROM Person.Person;`  
  
 `GO`  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `BusinessEntityID   FirstName                 LastName`  
  
 `----------- -------------------------------- -----------`  
  
 `1           Syed                             Abbas`  
  
 `2           Catherine                        Abel`  
  
 `3           Kim                              Abercrombie`  
  
 `(3 rows affected)`  
  
 `1> EXIT`  
  
 `C:\>`  
  
 Les lignes situées après la ligne `3> GO` sont les données de sortie d'une instruction `SELECT` . Une fois les données de sortie générées, `sqlcmd` réinitialise l'invite `sqlcmd` et affiche `1>`. Après avoir entré `EXIT` sur la ligne `1>`, la fenêtre d'invite de commandes affiche la même ligne que celle qu'elle a affichée lorsque vous avez ouvert l'invite de commandes la première fois. Ceci indique que `sqlcmd` a mis fin à sa session. Vous pouvez maintenant fermer la fenêtre d'invite de commandes en tapant une autre commande `EXIT` .  
  
## <a name="running-transact-sql-script-files-using-sqlcmd"></a>Exécution de fichiers de script Transact-SQL à l’aide de sqlcmd  
 Vous pouvez utiliser **sqlcmd** pour exécuter des fichiers de script de base de données. Il s'agit de fichiers texte contenant un mélange d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] , de commandes **sqlcmd** et de variables de script. Pour plus d’informations sur la façon de générer un script pour des variables, consultez [Utiliser sqlcmd avec des variables de script](./sqlcmd-use-with-scripting-variables.md). **sqlcmd** fonctionne avec les instructions, les commandes et les variables de script contenues dans un fichier de script de manière très similaire à son fonctionnement avec des instructions et des commandes entrées de manière interactive. La principale différence est que **sqlcmd** lit le fichier d'entrée sans marquer de pause au lieu d'attendre que l'utilisateur entre les instructions, les commandes et les variables de script.  
  
 Il existe plusieurs manières de créer des fichiers de script de base de données :  
  
-   Vous pouvez construire et déboguer de manière interactive un ensemble d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis enregistrer le contenu de la fenêtre Requête en tant que fichier de script.  
  
-   Vous pouvez créer un fichier texte contenant des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] à l'aide d'un éditeur de texte tel que le Bloc-notes.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-running-a-script-by-using-sqlcmd"></a>R. Exécution d'un script à l'aide de sqlcmd  
 Démarrez le Bloc-notes et tapez les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes :  
  
 `USE AdventureWorks2012;`  
  
 `GO`  
  
 `SELECT TOP (3) BusinessEntityID, FirstName, LastName`  
  
 `FROM Person.Person;`  
  
 `GO`  
  
 Créez un dossier intitulé `MyFolder` puis enregistrez le script sous le fichier `MyScript.sql` dans le dossier `C:\MyFolder`. Entrez les instructions suivantes à l'invite de commandes pour exécuter le script et diriger la sortie dans `MyOutput.txt` dans `MyFolder`:  
  
 `sqlcmd -i C:\MyFolder\MyScript.sql -o C:\MyFolder\MyOutput.txt`  
  
 Lorsque vous affichez le contenu de `MyOutput.txt` dans le Bloc-notes, vous découvrez son contenu :  
  
 `Changed database context to 'AdventureWorks2012'.`  
  
 `BusinessEntityID FirstName   LastName`  
  
 `---------------- ----------- -----------`  
  
 `1                Syed        Abbas`  
  
 `2                Catherine   Abel`  
  
 `3                Kim         Abercrombie`  
  
 `(3 rows affected)`  
  
### <a name="b-using-sqlcmd-with-a-dedicated-administrative-connection"></a>B. Utilisation de sqlcmd avec une connexion d'administration dédiée  
 Dans l'exemple suivant, `sqlcmd` permet de se connecter à un serveur ayant un problème de blocage à l'aide de la connexion administrateur dédiée.  
  
 `C:\>sqlcmd -S ServerName -A`  
  
 `1> SELECT session_id, blocking_session_id FROM sys.dm_exec_requests WHERE blocking_session_id <> 0;`  
  
 `2> GO`  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `session_id   blocking_session_id`  
  
 `------ -------`  
  
 `62       64`  
  
 `(1 rows affected)`  
  
 Utilisez `sqlcmd` pour mettre fin au processus de blocage.  
  
 `1> KILL 64;`  
  
 `2> GO`  
  
### <a name="c-using-sqlcmd-to-execute-a-stored-procedure"></a>C. Utilisation de sqlcmd pour exécuter une procédure stockée  
 L'exemple suivant montre comment exécuter une procédure stockée à l'aide de `sqlcmd`. Créez la procédure stockée suivante.  
  
 `USE AdventureWorks2012;`  
  
 `IF OBJECT_ID ( ' dbo.ContactEmailAddress, 'P' ) IS NOT NULL`  
  
 `DROP PROCEDURE dbo.ContactEmailAddress;`  
  
 `GO`  
  
 `CREATE PROCEDURE dbo.ContactEmailAddress`  
  
 `(`  
  
 `@FirstName nvarchar(50)`  
  
 `,@LastName nvarchar(50)`  
  
 `)`  
  
 `AS`  
  
 `SET NOCOUNT ON`  
  
 `SELECT EmailAddress`  
  
 `FROM Person.Person`  
  
 `WHERE FirstName = @FirstName`  
  
 `AND LastName = @LastName;`  
  
 `SET NOCOUNT OFF`  
  
 À l'invite `sqlcmd` , entrez :  
  
 `C:\sqlcmd`  
  
 `1> :Setvar FirstName Gustavo`  
  
 `1> :Setvar LastName Achong`  
  
 `1> EXEC dbo.ContactEmailAddress $(FirstName),$(LastName)`  
  
 `2> GO`  
  
 `EmailAddress`  
  
 `-----------------------------`  
  
 `gustavo0@adventure-works.com`  
  
### <a name="d-using-sqlcmd-for-database-maintenance"></a>D. Utilisation de sqlcmd pour la maintenance de base de données  
 L'exemple suivant montre comment utiliser `sqlcmd` pour une tâche de maintenance de base de données. Créez `C:\BackupTemplate.sql` à l'aide du code suivant.  
  
 `USE master;`  
  
 `BACKUP DATABASE [$(db)] TO DISK='$(bakfile)';`  
  
 À l'invite `sqlcmd` , entrez :  
  
 `C:\ >sqlcmd`  
  
 `1> :connect <server>`  
  
 `Sqlcmd: Successfully connected to server <server>.`  
  
 `1> :setvar db msdb`  
  
 `1> :setvar bakfile c:\msdb.bak`  
  
 `1> :r c:\BackupTemplate.sql`  
  
 `2> GO`  
  
 `Changed database context to 'master'.`  
  
 `Processed 688 pages for database 'msdb', file 'MSDBData' on file 2.`  
  
 `Processed 5 pages for database 'msdb', file 'MSDBLog' on file 2.`  
  
 `BACKUP DATABASE successfully processed 693 pages in 0.725 seconds (7.830 MB/sec)`  
  
### <a name="e-using-sqlcmd-to-execute-code-on-multiple-instances"></a>E. Utilisation de sqlcmd pour exécuter du code sur plusieurs instances  
 Le code suivant dans un fichier illustre la connexion d'un script à deux instances. Notez `GO` avant la connexion à la deuxième instance.  
  
 `:CONNECT <server>\,<instance1>`  
  
 `EXEC dbo.SomeProcedure`  
  
 `GO`  
  
 `:CONNECT <server>\,<instance2>`  
  
 `EXEC dbo.SomeProcedure`  
  
 `GO`  
  
### <a name="e-returning-xml-output"></a>E. Retour d'une sortie XML  
 L'exemple suivant montre comment la sortie XML est retournée, sans mise en forme, dans un flux continu.  
  
 `C:\>sqlcmd -d AdventureWorks2012`  
  
 `1> :XML ON`  
  
 `1> SELECT TOP 3 FirstName + ' ' + LastName + ', '`  
  
 `2> FROM Person.Person`  
  
 `3> GO`  
  
 `Syed Abbas, Catherine Abel, Kim Abercrombie,`  
  
### <a name="f-using-sqlcmd-in-a-windows-script-file"></a>F. Utilisation de sqlcmd dans un fichier de script Windows  
 Une commande **sqlcmd**, telle que `sqlcmd -i C:\InputFile.txt -o C:\OutputFile.txt,` , peut s'exécuter dans un fichier .bat en même temps que VBScript. Dans ce cas, n'utilisez pas les options interactives. **sqlcmd** doit être installé sur l'ordinateur qui exécute le fichier .bat.  
  
 Commencez par créer les quatre fichiers suivants :  
  
-   C:\badscript.sql  
  
    ```  
    SELECT batch_1_this_is_an_error  
    GO  
    SELECT 'batch #2'  
    GO  
    ```  
  
-   C:\goodscript.sql  
  
    ```  
    SELECT 'batch #1'  
    GO  
    SELECT 'batch #2'  
    GO  
    ```  
  
-   C:\returnvalue.sql  
  
    ```  
    :exit(select 100)  
    ```  
  
-   C:\windowsscript.bat  
  
    ```  
    @echo off  
  
    echo Running badscript.sql  
    sqlcmd -i badscript.sql -b -o out.log  
    if not errorlevel 1 goto next1  
    echo == An error occurred   
  
    :next1  
  
    echo Running goodscript.sql  
    sqlcmd -i goodscript.sql -b -o out.log  
    if not errorlevel 1 goto next2  
    echo == An error occurred   
  
    :next2  
    echo Running returnvalue.sql  
    sqlcmd -i returnvalue.sql -o out.log  
    echo SQLCMD returned %errorlevel% to the command shell  
  
    :exit  
    ```  
  
 Puis, à l'invite de commandes, exécutez `C:\windowsscript.bat`:  
  
 `C:\>windowsscript.bat`  
  
 `Running badscript.sql`  
  
 `== An error occurred`  
  
 `Running goodscript.sql`  
  
 `Running returnvalue.sql`  
  
 `SQLCMD returned 100 to the command shell`  
  
### <a name="g-using-sqlcmd-to-set-encryption-on-azure-sql-database"></a>G. Utilisation de sqlcmd pour définir le chiffrement sur Azure SQL Database  
 Une commande **sqlcmd** peut être exécutée sur une connexion aux données [!INCLUDE[ssSDS](../../includes/sssds-md.md)] pour spécifier le chiffrement et l'approbation de certificat. Deux options **sqlcmd**`` sont disponibles :  
  
-   Le commutateur -N est utilisé par le client pour demander une connexion chiffrée. Cette option est équivalente à l'option ADO.net `ENCRYPT = true`.  
  
-   Le commutateur -C est utilisé par le client pour le configurer de façon à approuver implicitement le certificat de serveur sans pour autant le valider. Cette option est équivalente à l'option ADO.net `TRUSTSERVERCERTIFICATE = true`.  
  
 Le service [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ne prend pas en charge toutes les options `SET` disponibles sur une instance SQL Server. Les options suivantes provoquent une erreur lorsque l'option `SET` correspondante est définie sur `ON` ou `OFF`:  
  
-   SET ANSI_DEFAULTS  
  
-   SET ANSI_NULLS  
  
-   SET REMOTE_PROC_TRANSACTIONS  
  
-   SET ANSI_NULL_DEFAULT  
  
 Les options SET suivantes ne provoquent pas d'exceptions mais ne peuvent pas être utilisées. Elles sont déconseillées :  
  
-   SET CONCAT_NULL_YIELDS_NULL  
  
-   SET ANSI_PADDING  
  
-   SET QUERY_GOVERNOR_COST_LIMIT  
  
#### <a name="syntax"></a>Syntaxe  
 Les exemples suivants font référence à des cas où les paramètres du fournisseur Native Client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluent : `ForceProtocolEncryption = False`, `Trust Server Certificate = No`  
  
 Connexion à l'aide des informations d'identification Windows et d'une communication chiffrée :  
  
```  
SQLCMD -E -N  
  
```  
  
 Connexion à l'aide des informations d'identification Windows et d'un certificat de serveur de confiance :  
  
```  
SQLCMD -E -C  
  
```  
  
 Connexion à l'aide des informations d'identification Windows, d'une communication chiffrée et d'un certificat de serveur de confiance :  
  
```  
SQLCMD -E -N -C  
  
```  
  
 Les exemples suivants font référence à des cas où les paramètres du fournisseur Native Client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluent : `ForceProtocolEncryption = True`, `TrustServerCertificate = Yes`.  
  
 Connexion à l'aide des informations d'identification Windows, d'une communication chiffrée et d'un certificat de serveur de confiance :  
  
```  
SQLCMD -E  
  
```  
  
 Connexion à l'aide des informations d'identification Windows, d'une communication chiffrée et d'un certificat de serveur de confiance :  
  
```  
SQLCMD -E -N  
  
```  
  
 Connexion à l'aide des informations d'identification Windows, d'une communication chiffrée et d'un certificat de serveur de confiance :  
  
```  
SQLCMD -E -C  
  
```  
  
 Connexion à l'aide des informations d'identification Windows, d'une communication chiffrée et d'un certificat de serveur de confiance :  
  
```  
SQLCMD -E -N -C  
  
```  
  
 Si le fournisseur spécifie `ForceProtocolEncryption = True` , alors le chiffrement est activé même si `Encrypt=No` est indiqué dans la chaîne de connexion.  
  
## <a name="more-about-sqlcmd"></a>En savoir plus sur sqlcmd  
 [Utilitaire sqlcmd](../../tools/sqlcmd-utility.md)   
 [Utiliser sqlcmd avec des variables de script](./sqlcmd-use-with-scripting-variables.md)   
 [Modifier des scripts SQLCMD à l'aide de l'Éditeur de requête](./edit-sqlcmd-scripts-with-query-editor.md)   
 [Gérer les étapes de travail](../../ssms/agent/manage-job-steps.md)   
 [Créer une étape de travail CmdExec](../../ssms/agent/create-a-cmdexec-job-step.md)  
  
