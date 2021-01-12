---
description: CREATE AGGREGATE (Transact-SQL)
title: CREATE AGGREGATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_AGGREGATE_TSQL
- CREATE AGGREGATE
- AGGREGATE_TSQL
- AGGREGATE
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE AGGREGATE statement
- aggregate functions [SQL Server], user-defined
- user-defined functions [CLR integration]
ms.assetid: 62eebc19-9f15-4245-94fa-b3fcd64a9d42
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 773c2010cdefa2455e0800ed0370cbc118c2dab9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098429"
---
# <a name="create-aggregate-transact-sql"></a>CREATE AGGREGATE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée une fonction d'agrégation définie par l'utilisateur dont l'implémentation est définie dans une classe d'un assembly dans le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Pour que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] lie la fonction d’agrégation à son implémentation, vous devez d’abord charger l’assembly [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] qui contient cette implémentation dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide d’une instruction CREATE ASSEMBLY.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
CREATE AGGREGATE [ schema_name . ] aggregate_name  
        (@param_name <input_sqltype>   
        [ ,...n ] )  
RETURNS <return_sqltype>  
EXTERNAL NAME assembly_name [ .class_name ]  
  
<input_sqltype> ::=  
        system_scalar_type | { [ udt_schema_name. ] udt_type_name }  
  
<return_sqltype> ::=  
        system_scalar_type | { [ udt_schema_name. ] udt_type_name }  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *schema_name*  
 Nom du schéma auquel appartient la fonction d'agrégation définie par l'utilisateur.  
  
 *aggregate_name*  
 Nom de la fonction d'agrégation que vous voulez créer.  
  
 **@** _param_name_  
 Un ou plusieurs paramètres dans l'agrégat défini par l'utilisateur. L'utilisateur doit fournir la valeur d'un paramètre lors de l'exécution de la fonction d'agrégation. Spécifiez un nom de paramètre en plaçant le signe **@** comme premier caractère. Le nom du paramètre doit respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md). Les paramètres sont locaux à la fonction.  
  
 *system_scalar_type*  
 Un des types de données scalaires système de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contiennent la valeur du paramètre d'entrée ou la valeur de retour. Tous les types de données scalaires peuvent être utilisés comme paramètre d’une agrégation définie par l’utilisateur, à l’exception de **text**, de **ntext** et d’**image**. Les types non scalaires, tels que **cursor** et **table**, ne peuvent pas être spécifiés.  
  
 *udt_schema_name*  
 Nom du schéma auquel appartient le type CLR défini par l'utilisateur. S’il n’est pas spécifié, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] fait référence à *udt_type_name* dans l’ordre suivant :  
  
-   l'espace de noms de type SQL natif ;  
  
-   Le schéma par défaut de l'utilisateur actuel dans la base de données active  
  
-   Schéma **dbo** dans la base de données active.  
  
 *udt_type_name*  
 Nom d'un type CLR défini par l'utilisateur et déjà créé dans la base de données active. Si le paramètre *udt_schema_name* n’est pas spécifié, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suppose que le type appartient au schéma de l’utilisateur actuel.  
  
 *assembly_name* [ **.** _class_name_ ]  
 Spécifie l'assembly à lier à la fonction d'agrégation définie par l'utilisateur et, éventuellement, le nom du schéma auquel appartient l'assembly et le nom de la classe d'assembly qui met en œuvre l'agrégation définie par l'utilisateur. L'assembly doit avoir été déjà créé dans la base de données à l'aide de l'instruction CREATE ASSEMBLY. Le paramètre *class_name* doit être un identificateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide et doit correspondre au nom d’une classe qui existe dans l’assembly. *class_name* peut être un nom qualifié par l’espace de noms, si le langage de programmation utilisé pour écrire la classe utilise des espaces de noms, comme C#. Si le paramètre *class_name* n’est pas spécifié, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suppose qu’il est identique à *aggregate_name*.  
  
## <a name="remarks"></a>Remarques  
 Par défaut, la possibilité de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'exécuter du code CLR est désactivée. Vous pouvez créer, modifier et supprimer des objets de base de données qui font référence à des modules de code managé. Cependant, le code de ces modules ne s’exécute pas dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sauf si l’[option clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) est activée à l’aide de l’argument [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
 La classe de l’assembly référencé dans *assembly_name* et ses méthodes doivent répondre à toutes les exigences en matière d’implémentation d’une fonction d’agrégation définie par l’utilisateur dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Agrégats CLR définis par l’utilisateur](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-aggregates.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite les autorisations CREATE AGGREGATE et REFERENCES pour l'assembly mentionné dans la clause EXTERNAL NAME.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant suppose qu'un exemple d'application windows StringUtilities.csproj est compilé. Pour plus d’informations, consultez [Exemple de fonctions d’utilitaire de chaîne](/previous-versions/sql/sql-server-2016/ff878119(v=sql.130)).  
  
 L'exemple crée une agrégation `Concatenate`. Avant la création de celle-ci, l'assembly `StringUtilities.dll` est enregistré dans la base de données locale.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SamplesPath nvarchar(1024)  
-- You may have to modify the value of the this variable if you have  
--installed the sample some location other than the default location.  
  
SELECT @SamplesPath = REPLACE(physical_name, 'Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\master.mdf', 'Microsoft SQL Server\130\Samples\Engine\Programmability\CLR\')   
     FROM master.sys.database_files   
     WHERE name = 'master';  
  
CREATE ASSEMBLY StringUtilities FROM @SamplesPath + 'StringUtilities\CS\StringUtilities\bin\debug\StringUtilities.dll'  
WITH PERMISSION_SET=SAFE;  
GO  
  
CREATE AGGREGATE Concatenate(@input nvarchar(4000))  
RETURNS nvarchar(4000)  
EXTERNAL NAME [StringUtilities].[Microsoft.Samples.SqlServer.Concatenate];  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DROP AGGREGATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-aggregate-transact-sql.md)  
  
