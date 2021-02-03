---
description: ALTER PROCEDURE (Transact-SQL)
title: ALTER PROCEDURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_PROCEDURE_TSQL
- ALTER_PROC_TSQL
- ALTER PROC
- ALTER PROCEDURE
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER PROCEDURE statement
- stored procedure modifications [SQL Server]
- modifying stored procedures
- stored procedures [SQL Server], modifying
ms.assetid: ed9b2f76-11ec-498d-a95e-75b490a75733
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ef55b1a81b4189f80bfe45410128e492a96bb191
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204145"
---
# <a name="alter-procedure-transact-sql"></a>ALTER PROCEDURE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifie une procédure déjà créée en exécutant l'instruction CREATE PROCEDURE dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
-- Syntax for SQL Server and Azure SQL Database
  
ALTER { PROC | PROCEDURE } [schema_name.] procedure_name [ ; number ]   
    [ { @parameter [ type_schema_name. ] data_type }   
        [ VARYING ] [ = default ] [ OUT | OUTPUT ] [READONLY]  
    ] [ ,...n ]   
[ WITH <procedure_option> [ ,...n ] ]  
[ FOR REPLICATION ]   
AS { [ BEGIN ] sql_statement [;] [ ...n ] [ END ] }  
[;]  
  
<procedure_option> ::=   
    [ ENCRYPTION ]  
    [ RECOMPILE ]  
    [ EXECUTE AS Clause ]  
```  
  
```syntaxsql  
-- Syntax for SQL Server CLR Stored Procedure  
  
ALTER { PROC | PROCEDURE } [schema_name.] procedure_name [ ; number ]   
    [ { @parameter [ type_schema_name. ] data_type }   
        [ = default ] [ OUT | OUTPUT ] [READONLY]  
    ] [ ,...n ]   
[ WITH EXECUTE AS Clause ]  
AS { EXTERNAL NAME assembly_name.class_name.method_name }  
[;]  
```  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
ALTER { PROC | PROCEDURE } [schema_name.] procedure_name  
    [ { @parameterdata_type } [= ] ] [ ,...n ]  
AS { [ BEGIN ] sql_statement [ ; ] [ ,...n ] [ END ] }  
[;]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *schema_name*  
 Nom du schéma auquel appartient la procédure.  
  
 *procedure_name*  
 Nom de la procédure à modifier. Les noms des procédures doivent respecter les conventions concernant les [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
 **;** *number*  
 Entier facultatif existant utilisé pour regrouper les procédures de même nom de façon à pouvoir les supprimer au moyen d’une instruction DROP PROCEDURE.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 **@** *parameter*  
 Paramètre de la procédure. Il est possible de spécifier jusqu'à 2 100 paramètres.  
  
 [ _type\_schema\_name_ **.** ] _data\_type_  
 Type de données du paramètre et du schéma auquel elle appartient.  
  
 Pour plus d’informations sur les restrictions applicables aux types de données, consultez [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md).  
  
 VARYING  
 Spécifie le jeu de résultats pris en charge comme paramètre de sortie. La procédure stockée construit dynamiquement ce paramètre. Son contenu est variable. S'applique seulement aux paramètres de type cursor. Cette option n'est pas valide pour les procédures CLR.  
  
 *default*  
 Valeur par défaut pour le paramètre.  
  
 OUT | OUTPUT  
 Indique que le paramètre est un paramètre renvoyé.  
  
 READONLY  
 Indique que le paramètre ne peut pas être mis à jour ou modifié dans le corps de la procédure. Si le type de paramètre est un type de table, READONLY doit être spécifié.  
  
 RECOMPILE  
 Indique que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] n'utilise pas le cache pour le plan de cette procédure et que la procédure est recompilée à l'exécution.  
  
 ENCRYPTION  
 **S’applique à** : SQL Server ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)].  
  
 Indique que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] se charge de convertir le texte d'origine provenant de l'instruction ALTER PROCEDURE dans un format d'obfuscation. La sortie générée par l'obfuscation n'est pas visible directement dans les affichages catalogue de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les utilisateurs n'ayant pas accès aux tables système ou aux fichiers de base de données ne peuvent pas récupérer le texte d'obfuscation. Le texte est cependant à la disposition des utilisateurs disposant de privilèges et qui peuvent accéder aux tables système par le biais du [port DAC](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md) ou qui accèdent directement aux fichiers de bases de données. Les utilisateurs qui peuvent associer un débogueur au processus serveur peuvent également récupérer la procédure d'origine de la mémoire au moment de l'exécution. Pour plus d’informations sur l’accès aux métadonnées système, consultez [Configuration de la visibilité des métadonnées](../../relational-databases/security/metadata-visibility-configuration.md).  
  
 Les procédures créées à l'aide de cette option ne peuvent pas être publiées dans le cadre d'une réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Cette option ne peut pas être spécifiée pour les procédures stockées CLR (Common Language Runtime).  
  
> [!NOTE]  
>  Au cours d’une mise à niveau, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilise les commentaires d’obfuscation stockés dans **sys.sql_modules** pour recréer des procédures.  
  
 EXECUTE AS  
 Spécifie le contexte de sécurité dans lequel exécuter la procédure stockée après y avoir accédé.  
  
 Pour plus d’informations, consultez [Clause EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
 FOR REPLICATION  

  
 Indique qu'il n'est pas possible d'exécuter sur l'Abonné les procédures stockées créées pour la réplication. Une procédure stockée créée avec l'option FOR REPLICATION est utilisée comme filtre de procédure stockée et n'est exécutée que lors de la réplication. Il n'est pas possible de déclarer des paramètres si FOR REPLICATION est spécifié. Cette option n'est pas valide pour les procédures CLR. L'option RECOMPILE est ignorée pour les procédures créées avec l'option FOR REPLICATION.  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données autonome.  
  
 { [ BEGIN ] *sql_statement* [;] [ ...*n* ] [ END ] }  
 Une ou plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] comprenant le corps de la procédure. Vous pouvez utiliser les mots clés facultatifs BEGIN et END pour délimiter les instructions. Pour plus d’informations, consultez les sections Recommandations, Remarques d’ordre général et Limitations et restrictions dans [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md).  
  
 EXTERNAL NAME _assembly\_name_ **.** _class\_name_ **.** _method\_name_  
 **S’applique à** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures.  
  
 Précise la méthode d’un assembly [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] pour créer une référence à une procédure stockée CLR. *class_name* doit être un identificateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide et doit exister en tant que classe dans l’assembly. Si la classe a un nom qualifié par un espace de noms, utilisez un point (**.**) pour séparer les parties de l’espace de noms. Le nom de la classe doit figurer entre crochets (**[]**) ou entre guillemets doubles (**""**). La méthode spécifiée doit être une méthode statique de la classe.  
  
 Par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas exécuter du code CLR. Vous pouvez créer, modifier et supprimer des objets d’une base de données qui font référence à des modules CLR (Common Language Runtime) ; cependant, vous ne pouvez pas exécuter ces références dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tant que vous n’avez pas activé l’[option CLR enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md). Pour activer cette option, utilisez [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
> [!NOTE]  
>  Les procédures CLR ne sont pas prises en charge dans une base de données autonome.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] pour les transformer en procédures stockées CLR et inversement.  
  
 ALTER PROCEDURE ne modifie pas les autorisations et n'affecte aucune procédure stockée ni aucun déclencheur dépendants. Cependant, les paramètres QUOTED_IDENTIFIER et ANSI_NULLS de la session active sont inclus dans la procédure stockée lorsque celle-ci est modifiée. Si les paramètres sont différents des paramètres actifs lors de la création de la procédure stockée, le comportement de celle-ci peut changer.  
  
 Si une procédure à été créée avec les options WITH ENCRYPTION ou WITH RECOMPILE, ces options sont activées seulement si elles figurent dans l'instruction ALTER PROCEDURE.  
  
 Pour plus d’informations sur les procédures stockées, consultez [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md).  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **ALTER** sur la procédure ou l’appartenance au rôle de base de données fixe **db_ddladmin**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée la procédure stockée `uspVendorAllInfo`. Cette procédure retourne le nom de tous les fournisseurs de [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)], les produits qu'ils vendent, leurs conditions de crédit et leur disponibilité. Lorsque cette procédure est créée, elle est ensuite modifiée pour renvoyer un jeu de résultats différent.  
  
```sql
IF OBJECT_ID ( 'Purchasing.uspVendorAllInfo', 'P' ) IS NOT NULL   
    DROP PROCEDURE Purchasing.uspVendorAllInfo;  
GO  
CREATE PROCEDURE Purchasing.uspVendorAllInfo  
WITH EXECUTE AS CALLER  
AS  
    SET NOCOUNT ON;  
    SELECT v.Name AS Vendor, p.Name AS 'Product name',   
      v.CreditRating AS 'Rating',   
      v.ActiveFlag AS Availability  
    FROM Purchasing.Vendor v   
    INNER JOIN Purchasing.ProductVendor pv  
      ON v.BusinessEntityID = pv.BusinessEntityID   
    INNER JOIN Production.Product p  
      ON pv.ProductID = p.ProductID   
    ORDER BY v.Name ASC;  
GO    
```  
  
 L'exemple suivant modifie la procédure stockée `uspVendorAllInfo`. Il supprime la clause EXECUTE AS CALLER et modifie le corps de la procédure pour qu'elle retourne uniquement les fournisseurs qui proposent le produit spécifié. Les fonctions `LEFT` et `CASE` personnalisent l'affichage du jeu de résultats.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER PROCEDURE Purchasing.uspVendorAllInfo  
    @Product VARCHAR(25)   
AS  
    SET NOCOUNT ON;  
    SELECT LEFT(v.Name, 25) AS Vendor, LEFT(p.Name, 25) AS 'Product name',   
    'Rating' = CASE v.CreditRating   
        WHEN 1 THEN 'Superior'  
        WHEN 2 THEN 'Excellent'  
        WHEN 3 THEN 'Above average'  
        WHEN 4 THEN 'Average'  
        WHEN 5 THEN 'Below average'  
        ELSE 'No rating'  
        END  
    , Availability = CASE v.ActiveFlag  
        WHEN 1 THEN 'Yes'  
        ELSE 'No'  
        END  
    FROM Purchasing.Vendor AS v   
    INNER JOIN Purchasing.ProductVendor AS pv  
      ON v.BusinessEntityID = pv.BusinessEntityID   
    INNER JOIN Production.Product AS p   
      ON pv.ProductID = p.ProductID   
    WHERE p.Name LIKE @Product  
    ORDER BY v.Name ASC;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```   
 Vendor               Product name  Rating    Availability  
-------------------- ------------- -------   ------------  
Proseware, Inc.      LL Crankarm   Average   No  
Vision Cycles, Inc.  LL Crankarm   Superior  Yes  
(2 row(s) affected)`  
```  

## <a name="see-also"></a>Voir aussi  
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [DROP PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-procedure-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Procédures stockées &#40;moteur de base de données&#41;](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)   
 [sys.procedures &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-procedures-transact-sql.md)  
  
  
