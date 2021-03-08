---
title: Injection SQL | Microsoft Docs
description: Découvrez comment fonctionnent les attaques par injection de code SQL. Atténuez ces attaques en validant les entrées et en examinant le code pour l’injection de code SQL dans SQL Server.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: security, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- SQL Injection
ms.assetid: eb507065-ac58-4f18-8601-e5b7f44213ab
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 074033472f37787c0bcf1da6f261a5f835f0cbec
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838974"
---
# <a name="sql-injection"></a>l’injection de code SQL ;
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Une attaque par injection de code SQL est une attaque dans laquelle un code malveillant est inséré dans les chaînes transmises ultérieurement à une instance de SQL Server pour analyse et exécution. Les procédures qui permettent de créer des instructions SQL doivent être vérifiées et analysées à la recherche d’éventuelles failles autorisant cette injection, car SQL Server exécute toutes les requêtes syntaxiquement correctes qu’il reçoit. Même les données paramétrables peuvent être manipulées par un pirate compétent et déterminé.  
  
## <a name="how-sql-injection-works"></a>Comment fonctionne l’injection SQL  
 Les injections SQL prennent principalement la forme d'insertions directes de code dans les variables d'entrée utilisateur qui sont concaténées avec des commandes SQL et exécutées. Des attaques par injection moins directes insèrent un code malveillant dans les chaînes destinées à être stockées dans une table ou en tant que métadonnées. Lorsque les chaînes stockées sont ensuite concaténées dans une commande SQL dynamique, le code nuisible est exécuté.  
  
 Le processus d'injection consiste à terminer prématurément une chaîne de texte et à ajouter une nouvelle commande. Étant donné que la commande insérée peut avoir d'autres chaînes ajoutées préalablement à son exécution, l'attaquant termine la chaîne injectée par une marque de commentaire « -- ». Le texte qui vient à la suite est ignoré au moment de l'exécution.  
  
 Le script suivant montre un exemple d'injection SQL simple. Il crée une requête SQL en concaténant des chaînes codées de manière irréversible avec une chaîne entrée par l'utilisateur :  
  
```csharp
var Shipcity;  
ShipCity = Request.form ("ShipCity");  
var sql = "select * from OrdersTable where ShipCity = '" + ShipCity + "'";  
```  
  
 L'utilisateur est invité à entrer le nom d'une ville. Si l'utilisateur entre `Redmond`, la requête assemblée par le script ressemble à ce qui suit :  
  
```sql
SELECT * FROM OrdersTable WHERE ShipCity = 'Redmond'  
```  
  
 Mais supposons que l'utilisateur entre ce qui suit :  
  
```sql
Redmond'; drop table OrdersTable--  
```  
  
 Dans ce cas, la requête assemblée par le script est la suivante :  
  
```sql
SELECT * FROM OrdersTable WHERE ShipCity = 'Redmond';drop table OrdersTable--'  
```  
  
 Le point-virgule « ; » indique la fin d'une requête et le début d'une autre. Le double tiret « -- » signifie que le reste de la ligne courante est un commentaire et qu'elle doit être ignorée. Si le code modifié est syntaxiquement correct, il sera exécuté par le serveur. Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécute cette instruction, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] va d'abord sélectionner tous les enregistrements `OrdersTable` où `ShipCity` est `Redmond`. Ensuite, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supprime `OrdersTable`.  
  
 Tant que le code SQL injecté est syntaxiquement correct, il n'est pas possible de détecter par programme cette modification. Par conséquent, vous devez valider toutes les entrées utilisateur et vérifier très attentivement le code qui exécute les commandes SQL construites dans le serveur utilisé. Les méthodes de codage conseillées sont détaillées dans les sections suivantes de cette rubrique.  
  
## <a name="validate-all-input"></a>Validation de toutes les entrées  
 Validez toujours les entrées utilisateur en testant leur type, leur longueur, leur format et leur plage. Lorsque vous mettez en place les consignes à suivre contre les entrées malveillantes, tenez compte de l'architecture et des scénarios de déploiement de votre application. Rappelez-vous que les programmes conçus pour s'exécuter dans un environnement sécurisé peuvent être copiés dans un environnement non sécurisé. Les suggestions suivantes peuvent être considérées comme des pratiques recommandées :  
  
-   Ne présumez pas de la taille, du type ou du contenu des données reçues par votre application. Par exemple, vous pouvez réaliser l'évaluation suivante :  
  
    -   Comment votre application se comportera-t-elle si un utilisateur itinérant ou malintentionné entre un fichier MPEG de 10 Mo alors que votre application attend un code postal ?  
  
    -   Comment votre application réagira-t-elle si une instruction `DROP TABLE` est intégrée dans un champ de texte ?  
  
-   Testez la taille et le type de données de l'entrée et appliquez les limites appropriées. Cela peut permettre d'éviter des dépassements de mémoire tampon délibérés.  
  
-   Testez le contenu des variables de chaîne et acceptez uniquement les valeurs attendues. Rejetez les entrées contenant des données binaires, des séquences de caractères d'échappement et des caractères de commentaire. Cela peut empêcher l'injection de scripts et protéger contre l'utilisation de dépassements de mémoire tampon.  
  
-   Lorsque vous utilisez des documents XML, validez toutes les données par rapport à leurs schémas lorsqu'elles sont entrées.  
  
-   Ne créez jamais d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] directement à partir d'entrées utilisateur.  
  
-   Utilisez des procédures stockées pour valider les entrées utilisateur.  
  
-   Dans des environnements à plusieurs niveaux, toutes les données doivent être validées avant leur acceptation dans la zone de confiance. Les données qui ne passent pas le processus de validation doivent être rejetées et une erreur doit être renvoyée au niveau précédent.  
  
-   Implémentez plusieurs niveaux de validation. Les précautions que vous prenez contre les utilisateurs malintentionnés occasionnels peuvent s'avérer inefficaces contre les pirates déterminés. Une meilleure pratique consiste à valider les entrées dans l'interface utilisateur, puis à tous les points ultérieurs auxquels elles rencontrent une limite de confiance.   
    Par exemple, la validation des données dans une application côté client peut empêcher l'injection de scripts simples. En revanche, si le niveau suivant considère que son entrée a déjà été validée, un utilisateur malveillant capable de contourner un client peut disposer d'un accès complet à un système.  
  
-   Ne concaténez jamais une entrée utilisateur qui n'est pas validée. La concaténation de chaîne est le point d'entrée principal pour l'injection de scripts.  
  
-   N’acceptez pas les chaînes suivantes dans les champs à partir desquels les noms de fichiers peuvent être construits : AUX, CLOCK$, COM1 à COM8, CON, CONFIG$, LPT1 à LPT8, NUL et PRN.  
  
 Si possible, rejetez les entrées qui contiennent les caractères suivants.  
  
|Caractère entré|Signification dans Transact-SQL|  
|---------------------|------------------------------|  
|**;**|Délimiteur de requête|  
|**'**|Délimiteur de chaîne de données de caractères|  
|**--**|Délimiteur de commentaire sur une seule ligne. Le texte suivant **--** jusqu’à la fin de cette ligne n’est pas évalué par le serveur.|  
|**/\**_ ... _*\*/**|Délimiteurs de commentaire. Le serveur n’évalue pas le texte qui figure entre les caractères **/\* *_ et _* \*/** .|  
|**xp_**|Figure au début du nom des procédures stockées étendues de catalogue, telles que `xp_cmdshell`.|  
  
### <a name="use-type-safe-sql-parameters"></a>Utilisation de paramètres SQL de type sécurisé  
 La collection **Parameters** de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit le contrôle du type et la validation de la longueur. Si vous utilisez la collection **Parameters** , les entrées sont traitées en tant que valeurs littérales et non pas en tant que code exécutable. L’utilisation de la collection **Parameters** présente un avantage supplémentaire, car elle permet d’appliquer des contrôles de type et de longueur. Les valeurs qui ne sont pas comprises dans les limites autorisées déclenchent une exception. L’extrait de code suivant illustre l’utilisation de la collection **Parameters** :  
  
```csharp
SqlDataAdapter myCommand = new SqlDataAdapter("AuthorLogin", conn);  
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure;  
SqlParameter parm = myCommand.SelectCommand.Parameters.Add("@au_id",  
     SqlDbType.VarChar, 11);  
parm.Value = Login.Text;  
```  
  
 Dans cet exemple, le paramètre `@au_id` est traité en tant que valeur littérale et non pas en tant que code exécutable. Le type et la longueur de cette valeur font l'objet d'un contrôle. Si la valeur du paramètre `@au_id` n'est pas conforme aux contraintes de type et de longueur, une exception est déclenchée.  
  
### <a name="use-parameterized-input-with-stored-procedures"></a>Utilisation d'entrées paramétrables avec des procédures stockées  
 Les procédures stockées peuvent être la cible des injections SQL si elles utilisent des entrées non filtrées. Par exemple, le code suivant est vulnérable :  
  
```csharp
SqlDataAdapter myCommand =   
new SqlDataAdapter("LoginStoredProcedure '" +   
                               Login.Text + "'", conn);  
```  
  
 Si vous utilisez des procédures stockées, vous devez utiliser des paramètres en entrée.  
  
### <a name="use-the-parameters-collection-with-dynamic-sql"></a>Utilisation de la collection Parameters avec des instructions SQL dynamiques  
 Si vous ne pouvez pas utiliser de procédure stockée, vous pouvez toujours utiliser des paramètres, comme illustré dans l’exemple de code suivant :  
  
```csharp
SqlDataAdapter myCommand = new SqlDataAdapter(  
"SELECT au_lname, au_fname FROM Authors WHERE au_id = @au_id", conn);  
SQLParameter parm = myCommand.SelectCommand.Parameters.Add("@au_id",   
                        SqlDbType.VarChar, 11);  
Parm.Value = Login.Text;  
```  
  
### <a name="filtering-input"></a>Filtrage des entrées  
 Le filtrage des entrées peut également être utile pour protéger des risques d'injection SQL par la suppression des caractères d'échappement. Toutefois, en raison du grand nombre de caractères susceptibles de poser des problèmes, ce moyen de défense n'est pas fiable. L'exemple suivant permet de rechercher les délimiteurs de chaînes de caractères.  
  
```csharp
private string SafeSqlLiteral(string inputSQL)  
{  
  return inputSQL.Replace("'", "''");  
}  
```  
  
### <a name="like-clauses"></a>Clauses LIKE  
 Notez que si vous utilisez une clause `LIKE` , les caractères génériques devront quand même être séparés par des caractères d’échappement :  
  
```csharp
s = s.Replace("[", "[[]");  
s = s.Replace("%", "[%]");  
s = s.Replace("_", "[_]");  
```  
  
## <a name="reviewing-code-for-sql-injection"></a>Examen du code à la recherche d'injection SQL  
 Vous devez examiner l’ensemble du code appelant `EXECUTE`, `EXEC`, ou `sp_executesql`. Vous pouvez utiliser des requêtes similaires à l'exemple suivant pour faciliter l'identification des procédures contenant ces instructions. Cette requête recherche les 1, 2, 3 ou 4 espaces après les mots `EXECUTE` ou `EXEC`.  
  
```sql
SELECT object_Name(id) FROM syscomments  
WHERE UPPER(text) LIKE '%EXECUTE (%'  
OR UPPER(text) LIKE '%EXECUTE  (%'  
OR UPPER(text) LIKE '%EXECUTE   (%'  
OR UPPER(text) LIKE '%EXECUTE    (%'  
OR UPPER(text) LIKE '%EXEC (%'  
OR UPPER(text) LIKE '%EXEC  (%'  
OR UPPER(text) LIKE '%EXEC   (%'  
OR UPPER(text) LIKE '%EXEC    (%'  
OR UPPER(text) LIKE '%SP_EXECUTESQL%';  
```  
  
### <a name="wrapping-parameters-with-quotename-and-replace"></a>Enveloppement de paramètres avec QUOTENAME() et REPLACE()  
 Dans chaque procédure stockée sélectionnée, vérifiez que toutes les variables utilisées dans du Transact-SQL dynamique sont traitées correctement. Les données issues des paramètres d'entrée de la procédure stockée ou lues à partir d'une table doivent être enveloppées dans QUOTENAME() ou REPLACE(). Gardez à l’esprit que la valeur de @variable transmise à QUOTENAME() est de type sysname et sa longueur maximale de 128 caractères.  
  
|\@variable|Wrapper recommandé|  
|---------------|-------------------------|  
|Nom d'un élément sécurisable|`QUOTENAME(@variable)`|  
|Chaîne ≤ à 128 caractères|`QUOTENAME(@variable, '''')`|  
|Chaîne de > 128 caractères|`REPLACE(@variable,'''', '''''')`|  
  
 Lorsque vous utilisez cette technique, une instruction SET peut être révisée comme suit :  
  
```sql
-- Before:  
SET @temp = N'SELECT * FROM authors WHERE au_lname ='''   
 + @au_lname + N'''';  
  
-- After:  
SET @temp = N'SELECT * FROM authors WHERE au_lname = '''   
 + REPLACE(@au_lname,'''','''''') + N'''';  
```  
  
### <a name="injection-enabled-by-data-truncation"></a>Injection activée par la troncature des données  
 Tout code [!INCLUDE[tsql](../../includes/tsql-md.md)] dynamique affecté à une variable sera tronqué s'il est d'une taille supérieure à la mémoire tampon allouée à cette variable. Un attaquant capable de forcer la troncature d'une instruction en transmettant des chaînes inhabituellement longues à une procédure stockée peut manipuler le résultat. Par exemple, la procédure stockée créée par le script suivant court un risque d'injection permis par la troncature.  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
@loginname sysname,  
@old sysname,  
@new sysname  
AS  
-- Declare variable.  
-- Note that the buffer here is only 200 characters long.   
DECLARE @command varchar(200)  
-- Construct the dynamic Transact-SQL.  
-- In the following statement, we need a total of 154 characters   
-- to set the password of 'sa'.   
-- 26 for UPDATE statement, 16 for WHERE clause, 4 for 'sa', and 2 for  
-- quotation marks surrounded by QUOTENAME(@loginname):  
-- 200 - 26 - 16 - 4 - 2 = 154.  
-- But because @new is declared as a sysname, this variable can only hold  
-- 128 characters.   
-- We can overcome this by passing some single quotation marks in @new.  
SET @command= 'update Users set password='   
    + QUOTENAME(@new, '''') + ' where username='   
    + QUOTENAME(@loginname, '''') + ' AND password = '   
    + QUOTENAME(@old, '''')  
  
-- Execute the command.  
EXEC (@command)  
GO  
```  
  
 En transmettant 154 caractères dans une mémoire tampon de 128 caractères, un attaquant peut définir un nouveau mot de passe pour sa sans connaître l’ancien.  
  
```sql
EXEC sp_MySetPassword 'sa', 'dummy',   
'123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012'''''''''''''''''''''''''''''''''''''''''''''''''''   
```  
  
 Pour cette raison, vous devez utiliser une mémoire tampon de grande taille pour une variable de commande ou exécuter directement le [!INCLUDE[tsql](../../includes/tsql-md.md)] dynamique à l’intérieur de l’instruction `EXECUTE` .  
  
### <a name="truncation-when-quotenamevariable--and-replace-are-used"></a>Troncature lors de l’utilisation de QUOTENAME(@variable, '''') et REPLACE()  
 Les chaînes renvoyées par QUOTENAME() et REPLACE() seront tronquées en mode silencieux si elles dépassent l'espace alloué. La procédure stockée créée dans l'exemple suivant illustre les conséquences possibles.  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
    @loginname sysname,  
    @old sysname,  
    @new sysname  
AS  
  
-- Declare variables.  
    DECLARE @login sysname  
    DECLARE @newpassword sysname  
    DECLARE @oldpassword sysname  
    DECLARE @command varchar(2000)  
  
-- In the following statements, the data stored in temp variables  
-- will be truncated because the buffer size of @login, @oldpassword,  
-- and @newpassword is only 128 characters, but QUOTENAME() can return  
-- up to 258 characters.  
    SET @login = QUOTENAME(@loginname, '''')  
    SET @oldpassword = QUOTENAME(@old, '''')  
    SET @newpassword = QUOTENAME(@new, '''')  
  
-- Construct the dynamic Transact-SQL.  
-- If @new contains 128 characters, then @newpassword will be '123... n  
-- where n is the 127th character.   
-- Because the string returned by QUOTENAME() will be truncated,   
-- it can be made to look like the following statement:  
-- UPDATE Users SET password ='1234. . .[127] WHERE username=' -- other stuff here  
    SET @command = 'UPDATE Users set password = ' + @newpassword   
     + ' where username =' + @login + ' AND password = ' + @oldpassword;  
  
-- Execute the command.  
EXEC (@command);  
GO  
```  
  
 Par conséquent, l’instruction suivante va définir les mots de passe de tous les utilisateurs à la valeur qui a été transmise dans le code précédent.  
  
```sql
EXEC sp_MyProc '--', 'dummy', '12345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678'  
```  
  
 Vous pouvez forcer la troncature de chaîne en dépassant l'espace de mémoire tampon alloué lors de l'utilisation de REPLACE(). La procédure stockée créée dans l'exemple suivant illustre les conséquences possibles.  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
    @loginname sysname,  
    @old sysname,  
    @new sysname  
AS  
  
-- Declare variables.  
    DECLARE @login sysname  
    DECLARE @newpassword sysname  
    DECLARE @oldpassword sysname  
    DECLARE @command varchar(2000)  
  
-- In the following statements, data will be truncated because   
-- the buffers allocated for @login, @oldpassword and @newpassword   
-- can hold only 128 characters, but QUOTENAME() can return   
-- up to 258 characters.   
    SET @login = REPLACE(@loginname, '''', '''''')  
    SET @oldpassword = REPLACE(@old, '''', '''''')  
    SET @newpassword = REPLACE(@new, '''', '''''')  
  
-- Construct the dynamic Transact-SQL.  
-- If @new contains 128 characters, @newpassword will be '123...n   
-- where n is the 127th character.   
-- Because the string returned by QUOTENAME() will be truncated, it  
-- can be made to look like the following statement:  
-- UPDATE Users SET password='1234...[127] WHERE username=' -- other stuff here   
    SET @command= 'update Users set password = ''' + @newpassword + ''' where username='''   
     + @login + ''' AND password = ''' + @oldpassword + '''';  
  
-- Execute the command.  
EXEC (@command);  
GO  
```  
  
 Comme avec QUOTENAME(), la troncature de chaîne par REPLACE() peut être évitée en déclarant des variables temporaires de taille suffisante pour tous les cas. Dans la mesure du possible, vous devez appeler QUOTENAME() ou REPLACE() directement à l'intérieur du [!INCLUDE[tsql](../../includes/tsql-md.md)]dynamique. Sinon, vous pouvez calculer la taille requise de la mémoire tampon de la manière suivante. Pour `@outbuffer = QUOTENAME(@input)`, la taille de `@outbuffer` doit être `2*(len(@input)+1)`. Lorsque vous utilisez `REPLACE()` et des apostrophes doubles, comme dans l’exemple précédent, une mémoire tampon de `2*len(@input)` est suffisante.  
  
 Le calcul suivant couvre tous les cas :  
  
```sql
WHILE LEN(@find_string) > 0, required buffer size =  
ROUND(LEN(@input)/LEN(@find_string),0) * LEN(@new_string)   
 + (LEN(@input) % LEN(@find_string))  
```  
  
### <a name="truncation-when-quotenamevariable--is-used"></a>Troncature lors de l’utilisation de QUOTENAME(@variable, ']')  
 La troncature peut se produire quand le nom d'un élément sécurisable [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est transmis à des instructions utilisant la forme `QUOTENAME(@variable, ']')`. L'exemple suivant illustre cela.  
  
```sql
CREATE PROCEDURE sp_MyProc  
    @schemaname sysname,  
    @tablename sysname,  
AS  
  
-- Declare a variable as sysname. The variable will be 128 characters.  
-- But @objectname actually must allow for 2*258+1 characters.   
DECLARE @objectname sysname  
SET @objectname = QUOTENAME(@schemaname)+'.'+ QUOTENAME(@tablename)   
-- Do some operations.  
GO  
```  
  
 Lorsque vous concaténez des valeurs de type sysname, vous devez utiliser des variables temporaires de taille suffisante pour contenir le maximum de 128 caractères par valeur. Dans la mesure du possible, appelez `QUOTENAME()` directement à l’intérieur du [!INCLUDE[tsql](../../includes/tsql-md.md)]dynamique. Sinon, vous pouvez calculer la taille requise de la mémoire tampon comme expliqué dans la section précédente.  
  
## <a name="see-also"></a>Voir aussi  
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)   
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)   
 [sp_executesql &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md)   
 [Sécurisation de SQL Server](../../relational-databases/security/securing-sql-server.md)  
  
  
