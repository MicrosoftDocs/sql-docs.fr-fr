---
title: Masquage dynamique des données | Microsoft Docs
description: Découvrez le masquage des données dynamique qui limite l’exposition des données sensibles en les masquant aux utilisateurs sans privilèges. Cela peut considérablement simplifier la sécurité dans SQL Server.
ms.date: 05/02/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
ms.assetid: a62f4ff9-2953-42ca-b7d8-1f8f527c4d66
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 33fb069efc355ebaa8892a743b926f927d8a435e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171281"
---
# <a name="dynamic-data-masking"></a>Masquage dynamique des données
[!INCLUDE [SQL Server 2016 ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

![Masquage dynamique des données](../../relational-databases/security/media/dynamic-data-masking.png)

Le masquage dynamique des données (DDM) limite l’exposition des données sensibles en les masquant aux utilisateurs sans privilège. Il peut être utilisé pour simplifier considérablement la conception et le codage de la sécurité dans votre application.  

Le masquage dynamique des données permet d’empêcher les accès non autorisés à des données sensibles. Pour cela, les clients peuvent spécifier la quantité de données sensibles à exposer avec un impact minimal sur la couche Application. Il peut être configuré sur les champs de base de données désignés afin de masquer les données sensibles dans les jeux de résultats des requêtes. Grâce au DDM, les données dans la base de données ne sont pas modifiées. Le masquage dynamique des données est facile à utiliser avec des applications existantes, car les règles de masquage sont appliquées dans les résultats de la requête. De nombreuses applications peuvent masquer des données sensibles sans modifier les requêtes existantes.

* Une stratégie de masquage des données centrale agit directement sur les champs sensibles de la base de données.
* Désignez les utilisateurs ou les rôles privilégiés qui ont accès aux données sensibles.
* Le masquage dynamique des données a des fonctions de masquage complet et partiel, ainsi qu’un masque aléatoire pour les données numériques.
* Des commandes [!INCLUDE[tsql_md](../../includes/tsql-md.md)] simples définissent et gèrent les masques.

Le masquage dynamique des données vise à limiter l’exposition de données sensibles, en empêchant des utilisateurs ne devant pas avoir accès à celles-ci de les consulter. En revanche, le masquage dynamique des données n’a pas pour but d’empêcher des utilisateurs d’une base de données de se connecter directement à celle-ci ou d’exécuter des requêtes exhaustives ayant pour effet d’exposer des éléments de données sensibles. Le masquage dynamique des données est complémentaire à d’autres fonctionnalités de sécurité de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (audit, chiffrement, sécurité au niveau des lignes...). Il est vivement recommandé de l’utiliser conjointement avec celles-ci pour mieux protéger les données sensibles contenues dans la base de données.  
  
Le masquage des données dynamiques est disponible dans [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)], et est configuré à l’aide de commandes [!INCLUDE[tsql](../../includes/tsql-md.md)] . Pour plus d’informations sur la configuration du masquage dynamique des données via le portail Azure, consultez [Prise en main du masquage dynamique des données de base de données SQL (portail Azure)](/azure/azure-sql/database/dynamic-data-masking-overview).  
  
## <a name="defining-a-dynamic-data-mask"></a>Définition d’un masque dynamique des données
 Il est possible de définir une règle de masquage sur une colonne d’une table, afin d’obfusquer les données qui y figurent. Quatre types de masques sont disponibles.  
  
|Fonction|Description|Exemples|  
|--------------|-----------------|--------------|  
|Default|Masquage complet en fonction des types de données des champs désignés.<br /><br /> Pour les données de type chaîne (string), utilisez XXXX, ou moins de X si la taille du champ est inférieure à 4 caractères (**char**, **nchar**,  **varchar**, **nvarchar**, **text**, **ntext**).  <br /><br /> Pour les données de type numérique, utilisez une valeur zéro (**bigint**, **bit**, **decimal**, **int**, **money**, **numeric**, **smallint**, **smallmoney**, **tinyint**, **float**, **real**).<br /><br /> Pour les données de type date et heure, utilisez 01.01.1900 00:00:00.0000000 (**date**, **datetime2**, **datetime**, **datetimeoffset**, **smalldatetime**, **time**).<br /><br />Pour les données de type binaire, utilisez un seul octet de valeur ASCII 0 (**binary**, **varbinary**, **image**).|Exemple de syntaxe de définition de colonne : `Phone# varchar(12) MASKED WITH (FUNCTION = 'default()') NULL`<br /><br /> Exemple de syntaxe alter : `ALTER COLUMN Gender ADD MASKED WITH (FUNCTION = 'default()')`|  
|E-mail|Méthode de masquage qui affiche la première lettre d’une adresse de messagerie et le suffixe de constante « .com », sous la forme d’une adresse de messagerie. `aXXX@XXXX.com`.|Exemple de syntaxe de définition : `Email varchar(100) MASKED WITH (FUNCTION = 'email()') NULL`<br /><br /> Exemple de syntaxe alter : `ALTER COLUMN Email ADD MASKED WITH (FUNCTION = 'email()')`|  
|Aléatoire|Fonction de masquage aléatoire à utiliser sur tout type de données numérique pour masquer la valeur d’origine à l’aide d’une valeur aléatoire dans une plage spécifiée.|Exemple de syntaxe de définition : `Account_Number bigint MASKED WITH (FUNCTION = 'random([start range], [end range])')`<br /><br /> Exemple de syntaxe alter : `ALTER COLUMN [Month] ADD MASKED WITH (FUNCTION = 'random(1, 12)')`|  
|Chaîne personnalisée|Méthode de masquage qui affiche les première et dernière lettres, et ajoute une chaîne de remplissage personnalisée au milieu. `prefix,[padding],suffix`<br /><br /> Remarque : Si la valeur d’origine est trop courte pour occuper la totalité du masque, une partie du préfixe ou du suffixe n’est pas exposée.|Exemple de syntaxe de définition : `FirstName varchar(100) MASKED WITH (FUNCTION = 'partial(prefix,[padding],suffix)') NULL`<br /><br /> Exemple de syntaxe alter : `ALTER COLUMN [Phone Number] ADD MASKED WITH (FUNCTION = 'partial(1,"XXXXXXX",0)')`<br /><br /> Autre exemple :<br /><br /> `ALTER COLUMN [Phone Number] ADD MASKED WITH (FUNCTION = 'partial(5,"XXXXXXX",0)')`|  
  
## <a name="permissions"></a>Autorisations  
 Vous n’avez pas besoin d’autorisation particulière pour créer une table avec un masque dynamique des données. Les autorisations de schéma standard **CREATE TABLE** et **ALTER** suffisent.  
  
 Pour ajouter, remplacer ou supprimer le masque d’une colonne, vous devez disposer des autorisations **ALTER ANY MASK** et **ALTER** sur la table. Il convient d’accorder l’autorisation **ALTER ANY MASK** à un responsable sécurité.  
  
 Les utilisateurs disposant de l’autorisation **SELECT** sur une table peuvent afficher les données de celle-ci. Les colonnes définies comme masquées affichent alors les données masquées. Accordez l’autorisation **UNMASK** à un utilisateur pour lui permettre de récupérer les données non masquées de colonnes pour lesquelles un masquage est défini.  
  
 L’autorisation **CONTROL** sur la base de données inclut les autorisations **ALTER ANY MASK** et **UNMASK** .  
  
## <a name="best-practices-and-common-use-cases"></a>Meilleures pratiques et des cas d’utilisation courants  
  
-   La création d’un masque sur une colonne n’empêche pas la mise à jour de celle-ci. Par conséquent, si les utilisateurs reçoivent des données masquées quand ils interrogent une colonne masquée, ils peuvent mettre à jour les données s’ils disposent d’autorisations en écriture. Il convient néanmoins d’utiliser une stratégie de contrôle d’accès appropriée pour limiter les autorisations de mise à jour.  
  
-   L’utilisation de `SELECT INTO` ou de `INSERT INTO` pour copier les données d’une colonne masquée dans une autre table a pour effet de masquer les données dans la table cible.  
  
-   Un masquage dynamique des données est appliqué pendant l’exécution d’opérations d’importation et d’exportation dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Une base de données contenant des colonnes masquées produit un fichier de données exportées dont les données sont masquées (en supposant qu’elle est exportée par un utilisateur sans privilèges **UNMASK**), et la base de données importée contient des données masquées statiquement.  
  
## <a name="querying-for-masked-columns"></a>Interrogation de colonnes masquées  
 Pour interroger des colonnes de table auxquelles une fonction de masquage est appliquée, utilisez la vue **sys.masked_columns** . Celle-ci hérite de la vue **sys.columns** . Elle retourne toutes les colonnes de la vue **sys.columns** , ainsi que les colonnes **is_masked** et **masking_function** , en indiquant si les colonnes sont masquées et, dans ce cas, la fonction de masquage est définie. Cette vue présente uniquement les colonnes auxquelles une fonction de masquage est appliquée.  
  
```sql 
SELECT c.name, tbl.name as table_name, c.is_masked, c.masking_function  
FROM sys.masked_columns AS c  
JOIN sys.tables AS tbl   
    ON c.[object_id] = tbl.[object_id]  
WHERE is_masked = 1;  
```  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Il n’est pas possible de définir une règle de masquage pour les types de colonnes suivants :  
  
-   Colonnes chiffrées (Always Encrypted)  
  
-   FILESTREAM  
  
-   COLUMN_SET, ou colonne éparse faisant partie d’un jeu de colonnes.  
  
-   Il n’est pas possible de configurer un masque sur une colonne calculée. En revanche, si la colonne calculée dépend d’une colonne masquée, la colonne calculée retourne des données masquées.  
  
-   Une colonne dont les données sont masquées ne peut servir de clé pour un index de texte intégral.  
  
 Pour les utilisateurs dépourvus de l’autorisation **UNMASK** , les instructions déconseillées **READTEXT**, **UPDATETEXT** et **WRITETEXT** ne fonctionnent pas correctement sur une colonne configurée pour le masquage dynamique des données. 
 
 L’ajout d’un masque de données dynamiques est implémenté comme un changement de schéma de la table sous-jacente, et ne peut donc pas être effectué sur une colonne ayant des dépendances. Pour contourner cette restriction, vous pouvez tout d’abord supprimer la dépendance, puis ajouter le masque de données dynamiques et recréer la dépendance. Par exemple, si la dépendance est liée à un index qui dépend de cette colonne, vous pouvez supprimer l’index, ajouter le masque, puis recréer l’index dépendant.
 

## <a name="security-note-bypassing-masking-using-inference-or-brute-force-techniques"></a>Note de sécurité : Ignorer le masquage à l’aide de techniques d’inférence ou de force brute

Le masquage des données dynamiques est conçu pour simplifier le développement d’applications en limitant l’exposition des données dans un ensemble de requêtes prédéfinies utilisées par l’application. Bien que le masquage dynamique des données puisse également être utile pour empêcher l’exposition accidentelle des données sensibles lorsque vous accédez directement à une base de données de production, il est important de noter que les utilisateurs non privilégiés bénéficiant d’autorisations de requête ad hoc peuvent appliquer des techniques pour accéder aux données réelles. S’il est nécessaire d’octroyer l’accès ad hoc, l’audit doit servir à surveiller toutes les activités de base de données et à limiter ce risque.
 
Par exemple, considérez un principal de base de données qui dispose de privilèges suffisants pour exécuter des requêtes ad hoc sur la base de données et essaie de « deviner » les données sous-jacentes, pour enfin déduire les valeurs réelles. Supposons que nous disposons d’un masque défini sur la colonne `[Employee].[Salary]` , et que cet utilisateur se connecte directement à la base de données et commence à deviner les valeurs, pour enfin déduire la valeur `[Salary]` d’un ensemble d’employés :
 

```sql
SELECT ID, Name, Salary FROM Employees
WHERE Salary > 99999 and Salary < 100001;
```

>    |  Id | Nom| Salaire |   
>    | ----- | ---------- | ------ | 
>    |  62543 | Jane Doe | 0 | 
>    |  91245 | John Smith | 0 |  

Cela montre que le masquage dynamique des données ne doit pas être utilisé comme une mesure isolée pour sécuriser totalement les données sensibles contre les utilisateurs exécutant des requêtes ad hoc sur la base de données. Il est approprié pour empêcher l’exposition accidentelle de données sensibles, mais ne protège pas contre des intentions malveillantes de déduire les données sous-jacentes.
 
Il est important de gérer correctement les autorisations sur la base de données, et de toujours suivre le principe des autorisations minimum requises. En outre, n’oubliez pas de laisser l’option d’audit activée afin de suivre toutes les activités survenant sur la base de données.

  
## <a name="examples"></a>Exemples  
  
### <a name="creating-a-dynamic-data-mask"></a>Création d’un masque dynamique des données  
 L’exemple suivant crée une table avec trois types différents de masques dynamiques des données. L’exemple remplit la table, puis affiche le résultat.  
  
```sql
CREATE TABLE Membership  
  (MemberID int IDENTITY PRIMARY KEY,  
   FirstName varchar(100) MASKED WITH (FUNCTION = 'partial(1,"XXXXXXX",0)') NULL,  
   LastName varchar(100) NOT NULL,  
   Phone varchar(12) MASKED WITH (FUNCTION = 'default()') NULL,  
   Email varchar(100) MASKED WITH (FUNCTION = 'email()') NULL);  
  
INSERT Membership (FirstName, LastName, Phone, Email) VALUES   
('Roberto', 'Tamburello', '555.123.4567', 'RTamburello@contoso.com'),  
('Janice', 'Galvin', '555.123.4568', 'JGalvin@contoso.com.co'),  
('Zheng', 'Mu', '555.123.4569', 'ZMu@contoso.net');  
SELECT * FROM Membership;  
```  
  
 Un nouvel utilisateur est créé, qui reçoit l’autorisation **SELECT** sur la table. Les requêtes exécutées en tant que `TestUser` affichent les données masquées.  
  
```sql 
CREATE USER TestUser WITHOUT LOGIN;  
GRANT SELECT ON Membership TO TestUser;  
  
EXECUTE AS USER = 'TestUser';  
SELECT * FROM Membership;  
REVERT;  
```  
  
 Le résultat montre les masques en modifiant les données de  
  
 `1    Roberto     Tamburello    555.123.4567    RTamburello@contoso.com`  
  
 en  
  
 `1    RXXXXXXX    Tamburello    xxxx            RXXX@XXXX.com`  
  
### <a name="adding-or-editing-a-mask-on-an-existing-column"></a>Ajout ou modification d’un masque sur une colonne existante  
 Utilisez l’instruction **ALTER TABLE** pour ajouter un masque à une colonne existante de la table ou pour modifier le masque appliqué à cette colonne.  
L’exemple suivant ajoute une fonction de masquage à la colonne `LastName` :  
  
```sql  
ALTER TABLE Membership  
ALTER COLUMN LastName ADD MASKED WITH (FUNCTION = 'partial(2,"XXX",0)');  
```  
  
 L’exemple suivant modifie une fonction de masquage appliquée à la colonne `LastName` :  

```sql  
ALTER TABLE Membership  
ALTER COLUMN LastName varchar(100) MASKED WITH (FUNCTION = 'default()');  
```  
  
### <a name="granting-permissions-to-view-unmasked-data"></a>Octroi d’autorisations d’afficher des données non masquées  
 L’octroi de l’autorisation **UNMASK** permet à `TestUser` d’afficher les données non masquées.  
  
```sql
GRANT UNMASK TO TestUser;  
EXECUTE AS USER = 'TestUser';  
SELECT * FROM Membership;  
REVERT;   
  
-- Removing the UNMASK permission  
REVOKE UNMASK TO TestUser;  
```  
  
### <a name="dropping-a-dynamic-data-mask"></a>Suppression d’un masque dynamique des données  
 L’instruction suivante supprime le masque appliqué à la colonne `LastName` , créé dans l’exemple précédent :  
  
```sql  
ALTER TABLE Membership   
ALTER COLUMN LastName DROP MASKED;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [column_definition &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-column-definition-transact-sql.md)   
 [sys.masked_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-masked-columns-transact-sql.md)   
 [Prise en main du masquage dynamique des données de Base de données SQL (portail Azure)](/azure/azure-sql/database/dynamic-data-masking-overview)
