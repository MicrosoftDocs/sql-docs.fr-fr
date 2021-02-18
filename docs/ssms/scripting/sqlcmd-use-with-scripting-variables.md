---
title: Utiliser sqlcmd avec des variables de script
description: Découvrez comment utiliser des variables de script pour créer un script adapté à plusieurs scénarios.
ms.custom: seo-lt-2019
ms.date: 08/09/2016
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- scripts [SQL Server], sqlcmd utility
- variables [SQL Server], scripts
- scripting variables [SQL Server]
- sqlcmd utility, scripts
- setvar command
ms.assetid: 793495ca-cfc9-498d-8276-c44a5d09a92c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 799bc2eca98002343d8ff755782d8f3b71d2af77
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342645"
---
# <a name="sqlcmd---use-with-scripting-variables"></a>sqlcmd : utiliser avec des variables de script
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Les variables utilisées dans les scripts sont appelées des variables de script. Ces variables permettent à un script d'être utilisé dans plusieurs scénarios. Par exemple, pour exécuter un script sur plusieurs serveurs, vous pouvez utiliser une variable de script pour le nom du serveur au lieu de modifier le script pour chaque serveur. La modification du nom de serveur fourni à la variable de script permet d'exécuter le même script sur différents serveurs.  
  
 Les variables de script peuvent être définies explicitement à l’aide de la commande **setvar** ou implicitement à l’aide de l’option **sqlcmd -v** .  
  
 Cette rubrique contient également des exemples de définition de variables d’environnement dans l’invite de commandes Cmd.exe à l’aide de **SET**.  
  
## <a name="setting-scripting-variables-by-using-the-setvar-command"></a>Définition des variables de script à l'aide de la commande setvar  
 La commande **setvar** permet de définir des variables de script. Les variables définies à l’aide de la commande **setvar** sont stockées en interne. Il ne faut pas confondre les variables de script avec les variables d’environnement qui sont définies dans l’invite de commandes à l’aide de **SET**. Si un script fait référence à une variable qui n’est pas une variable d’environnement ou qui n’est pas définie à l’aide de **setvar**, un message d’erreur est renvoyé et l’exécution du script s’arrête. Pour plus d’informations, consultez l’option **-b** dans [Utilitaire sqlcmd](../../tools/sqlcmd-utility.md).  
  
## <a name="variable-precedence-low-to-high"></a>Priorité des variables (faible à élevée)  
 Si plusieurs types de variables ont le même nom, la variable dont la priorité est la plus élevée est utilisée.  
  
1.  Variables d'environnement de niveau système  
  
2.  Variables d'environnement de niveau utilisateur  
  
3.  Interface de commande (**SET X=Y**) définie dans l’invite de commandes avant le démarrage de **sqlcmd**  
  
4.  **sqlcmd -v** X=Y  
  
5.  **:Setvar** X Y  
  
> [!NOTE]  
>  Pour afficher les variables d’environnement, dans le **Panneau de configuration**, ouvrez **Système**, puis cliquez sur l’onglet **Avancé** .  
  
## <a name="implicitly-setting-scripting-variables"></a>Définition de variables de script de manière implicite  
 Lorsque vous démarrez **sqlcmd** avec une option associée à une variable **sqlcmd** , la variable **sqlcmd** prend implicitement la valeur spécifiée par l’option. Dans l'exemple suivant, `sqlcmd` démarre avec l'option `-l` . Cette opération définit implicitement la variable SQLLOGINTIMEOUT.  
  
```
c:\> sqlcmd -l 60
```
 
Vous pouvez également utiliser l’option **-v** pour définir une variable de script qui existe dans un script. Dans le script suivant (le nom de fichier est `testscript.sql`), `ColumnName` représente une variable de script.  
 
```
USE AdventureWorks2012;

SELECT x.$(ColumnName)
FROM Person.Person x
WHERE x.BusinessEntityID < 5;
```

Vous pouvez ensuite spécifier le nom de la colonne à retourner à l'aide de l'option `-v` :  
 
```
sqlcmd -v ColumnName ="FirstName" -i c:\testscript.sql
```

Pour retourner une autre colonne à l'aide du même script, modifiez la valeur de la variable de script `ColumnName` .  
  
```
sqlcmd -v ColumnName ="LastName" -i c:\testscript.sql
```

## <a name="guidelines-for-scripting-variable-names-and-values"></a>Instructions pour les noms et valeurs de variables de script  
 Lorsque vous nommez des variables de script, tenez compte des instructions suivantes :  
  
-   Les noms de variables ne doivent pas contenir d'espaces blancs ou de guillemets.  
  
-   Les noms de variables ne peuvent pas avoir la même forme qu’une expression de variable, telle que *$(var)* .  
  
-   Les variables de script ne respectent pas la casse.  
  
    > [!NOTE]  
    >  Si aucune valeur n’est affectée à une variable d’environnement **sqlcmd** , la variable est supprimée. Si vous déclarez **:setvar VarName** sans spécifier de valeur, la variable est supprimée.  
  
 Lorsque vous spécifiez des valeurs pour des variables de script, tenez compte des instructions suivantes :  
  
-   Les valeurs de variable définies à l’aide de **setvar** ou de l’option **-v** doivent être placées entre guillemets si la valeur de type chaîne contient des espaces.  
  
-   Si la valeur de variable contient des guillemets, ceux-ci doivent être placés dans une séquence d'échappement. Par exemple :`setvar MyVar "spac""e"`.  
  
## <a name="guidelines-for-cmdexe-set-variable-values-and-names"></a>Instructions pour utiliser Cmd.exe SET avec les noms et valeurs de variables  
 Les variables définies à l’aide de SET appartiennent à l’environnement Cmd.exe et peuvent être référencées par **sqlcmd**. Tenez compte des recommandations suivantes :  
  
-   Les noms de variables ne doivent pas contenir d'espaces blancs ou de guillemets.  
  
-   Les valeurs de variables peuvent contenir des espaces ou des guillemets.  
  
## <a name="sqlcmd-scripting-variables"></a>Variables de script sqlcmd  
 Les variables définies par **sqlcmd** sont reconnues comme des variables de script. Le tableau suivant répertorie les variables de script **sqlcmd** .  
  
|        Variable         | Option connexe | R/W (Lecture/écriture) |         Default         |
| ----------------------- | -------------- | --- | ----------------------- |
| SQLCMDUSER\*             | -U             | R   | ""                      |
| SQLCMDPASSWORD\*         | -P             | --  | ""                      |
| SQLCMDSERVER\*           | -S             | R   | "DefaultLocalInstance"  |
| SQLCMDWORKSTATION       | -H             | R   | "ComputerName"          |
| SQLCMDDBNAME            | -d             | R   | ""                      |
| SQLCMDLOGINTIMEOUT      | -l             | R/W (Lecture/écriture) | "8" (secondes)           |
| SQLCMDSTATTIMEOUT       | -T             | R/W (Lecture/écriture) | "0" = Attendre indéfiniment |
| SQLCMDHEADERS           | -H             | R/W (Lecture/écriture) | "0"                     |
| SQLCMDCOLSEP            | -S             | R/W (Lecture/écriture) | " "                     |
| SQLCMDCOLWIDTH          | -w             | R/W (Lecture/écriture) | "0"                     |
| SQLCMDPACKETSIZE        | -a             | R   | "4096"                  |
| SQLCMDERRORLEVEL        | -M             | R/W (Lecture/écriture) | "0"                     |
| SQLCMDMAXVARTYPEWIDTH   | -y             | R/W (Lecture/écriture) | "256"                   |
| SQLCMDMAXFIXEDTYPEWIDTH | -y             | R/W (Lecture/écriture) | "0" = illimitée         |
| SQLCMDEDITOR            |                | R/W (Lecture/écriture) | "edit.com"              |
| SQLCMDINI               |                | R   | ""                      |

Les variables SQLCMDUSER, SQLCMDPASSWORD et SQLCMDSERVER sont définies lorsque la commande **:Connect** est utilisée.  

R indique que la valeur ne peut être définie qu'une seule fois lors de l'initialisation du programme.  
  
R/W indique que la valeur peut être réinitialisée à l’aide de la commande **setvar** et que les commandes suivantes utiliseront la nouvelle valeur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-the-setvar-command-in-a-script"></a>R. Utilisation de la commande setvar dans un script  
 De nombreuses options de **sqlcmd** peuvent être contrôlées dans un script à l’aide de la commande **setvar** . Dans l'exemple suivant, le script `test.sql` est créé ; dans ce dernier, la variable `SQLCMDLOGINTIMEOUT` a la valeur `60` secondes et une variable de script, `server`, a la valeur `testserver`. Le code suivant figure dans le fichier `test.sql`.  

```
:setvar SQLCMDLOGINTIMEOUT 60
:setvar server "testserver"
:connect $(server) -l $(SQLCMDLOGINTIMEOUT)

USE AdventureWorks2012;

SELECT FirstName, LastName
FROM Person.Person;
```

Le script est alors appelé avec sqlcmd :

```
sqlcmd -i c:\test.sql
```
  
### <a name="b-using-the-setvar-command-interactively"></a>B. Utilisation de la commande setvar de façon interactive  
 L'exemple suivant illustre la définition d'une variable de script de façon interactive à l'aide de la commande `setvar` .  

```
sqlcmd
:setvar  MYDATABASE AdventureWorks2012
USE $(MYDATABASE);
GO
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Changed database context to 'AdventureWorks2012'
1>
```
  
### <a name="c-using-command-prompt-environment-variables-within-sqlcmd"></a>C. Utilisation de variables d'environnement à l'invite de commandes dans sqlcmd  
 Dans l’exemple suivant, quatre variables d’environnement ( `are` ) sont définies puis appelées depuis `sqlcmd`.  

```
C:\>SET tablename=Person.Person
C:\>SET col1=FirstName
C:\>SET col2=LastName
C:\>SET title=Ms.
C:\>sqlcmd -d AdventureWorks2012
1> SELECT TOP 5 $(col1) + ' ' + $(col2) AS Name
2> FROM $(tablename)
3> WHERE Title ='$(title)'
4> GO
```
  
### <a name="d-using-user-level-environment-variables-within-sqlcmd"></a>D. Utilisation de variables d'environnement au niveau utilisateur dans sqlcmd  
 Dans l'exemple suivant, la variable d'environnement au niveau utilisateur `%Temp%` est définie à l'invite de commandes et transmise au fichier d'entrée `sqlcmd` . Pour obtenir la variable d’environnement au niveau utilisateur, dans le **Panneau de configuration**, double-cliquez sur **Système**. Cliquez sur l'onglet **Avancé** , puis sur **Variables d'environnement**.  
  
 Le code suivant figure dans le fichier d'entrée `c:\testscript.txt`:

```
:OUT $(MyTempDirectory)
USE AdventureWorks2012;

SELECT FirstName
FROM AdventureWorks2012.Person.Person
WHERE BusinessEntityID` `< 5;
```

Le code suivant est entré à l'invite de commandes :

```
C:\ >SET MyTempDirectory=%Temp%\output.txt
C:\ >sqlcmd -i C:\testscript.txt
```

 Le résultat suivant est envoyé dans le fichier de sortie C:\Documents and Settings\\<utilisateur\>\Local Settings\Temp\output.txt.  

```
Changed database context to 'AdventureWorks2012'.
FirstName
--------------------------------------------------
Gustavo
Catherine
Kim
Humberto

(4 rows affected)
```

### <a name="e-using-a-startup-script"></a>E. Utilisation d'un script de démarrage  
 Un script de démarrage **sqlcmd** est exécuté au démarrage de **sqlcmd** . L'exemple suivant définit la variable d'environnement `SQLCMDINI`. Voici le contenu de `init.sql.`  

```
SET NOCOUNT ON
GO

DECLARE @nt_username nvarchar(128)
SET @nt_username = (SELECT rtrim(convert(nvarchar(128), nt_username))
FROM sys.dm_exec_sessions WHERE spid = @@SPID)
SELECT  @nt_username + ' is connected to ' +
rtrim(CONVERT(nvarchar(20), SERVERPROPERTY('servername'))) +
' (' +`  
rtrim(CONVERT(nvarchar(20), SERVERPROPERTY('productversion'))) +
')'
:setvar SQLCMDMAXFIXEDTYPEWIDTH 100
SET NOCOUNT OFF
GO

:setvar SQLCMDMAXFIXEDTYPEWIDTH
```

 Ce code appelle le fichier `init.sql` au démarrage de `sqlcmd` .  
  
```
c:\> SET sqlcmdini=c:\init.sql
>1 Sqlcmd
```

 Il s'agit de la sortie.  

```
>1 < user > is connected to < server > (9.00.2047.00)
```

  
> [!NOTE]  
>  L’option **-X** désactive la fonctionnalité de script de démarrage.  
  
### <a name="f-variable-expansion"></a>F. Expansion de variables  
 l’exemple suivant illustre l’utilisation de données sous la forme d’une variable **sqlcmd** .  

```
USE AdventureWorks2012;
CREATE TABLE AdventureWorks2012.dbo.VariableTest
(
Col1 nvarchar(50)
);
GO
```

 Insérez une ligne dans `Col1` de `dbo.VariableTest` qui contient la valeur `$(tablename)`.  

```
INSERT INTO AdventureWorks2012.dbo.VariableTest(Col1)
VALUES('$(tablename)');
GO
```
  
 À l'invite de commandes `sqlcmd` , si aucune variable n'a la valeur `$(tablename)`, les instructions suivantes retournent la ligne.  
  
```
C:\> sqlcmd
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(tablename)';
>2 GO
>3 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(tablename)';
>4 GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
>1 Col1
>2 ------------------
>3 $(tablename)
>4
>5 (1 rows affected)
```

 La variable `MyVar` a la valeur `$(tablename)`.  

```
>6 :setvar MyVar $(tablename)
```

 Ces instructions retournent la ligne ainsi que le message « La variable de script « tablename » n'est pas définie ».  

```
>6 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(tablename)';
>7 GO

>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(tablename)';
>2 GO
```

 Ces instructions retournent la ligne.  

```
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(MyVar)';
>2 GO
```

```
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(MyVar)';
>2 GO
```
  
## <a name="see-also"></a>Voir aussi  
 [Utiliser l’utilitaire sqlcmd](./sqlcmd-use-the-utility.md)   
 [Utilitaire sqlcmd](../../tools/sqlcmd-utility.md)   
 [Référence de l’utilitaire d’invite de commandes &#40;moteur de base de données&#41;](../../tools/command-prompt-utility-reference-database-engine.md)  
  
