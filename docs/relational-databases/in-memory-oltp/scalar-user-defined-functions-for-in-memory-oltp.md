---
title: Fonctions scalaires définies par l’utilisateur pour l’OLTP en mémoire | Microsoft Docs
description: Découvrez comment créer et supprimer des fonctions scalaires définies par l’utilisateur compilées en mode natif pour l’OLTP en mémoire dans SQL Server. La compilation native améliore les performances.
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: d2546e40-fdfc-414b-8196-76ed1f124bf5
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6ed83ee45844bd7299ca58eeeeed95849d0399f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353662"
---
# <a name="scalar-user-defined-functions-for-in-memory-oltp"></a>Fonctions scalaires définies par l’utilisateur pour l’OLTP en mémoire
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], vous pouvez créer et supprimer des fonctions scalaires définies par l’utilisateur compilées en mode natif. Vous pouvez également modifier ces fonctions définies par l'utilisateur : La compilation native améliore les performances de l’évaluation de fonctions définies par l’utilisateur dans une instruction Transact-SQL.  
  
 Lorsque vous modifiez une fonction scalaire définie par l’utilisateur compilées en mode natif, l’application reste disponible pendant que l’opération est en cours d’exécution et que la nouvelle version de la fonction est en cours de compilation.  
  
 Pour les constructions T-SQL prises en charge, consultez [Fonctionnalités prises en charge pour les modules T-SQL compilés en mode natif](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).  
  
## <a name="creating-dropping-and-altering-user-defined-functions"></a>Création, suppression et modification de fonctions définies par l'utilisateur  
 La fonction CREATE vous permet de créer la fonction scalaire définie par l’utilisateur compilée en mode natif, la fonction DROP de supprimer la fonction définie par l’utilisateur et la fonction ALTER de la modifier. BEGIN ATOMIC WITH est requis pour les fonctions définies par l’utilisateur.  
  
 Pour plus d’informations sur la syntaxe prise en charge et les restrictions, consultez les rubriques suivantes.  
  
-   [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)  
  
-   [ALTER FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-function-transact-sql.md)  
  
-   [DROP FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-function-transact-sql.md)  
  
     La syntaxe DROP FUNCTION pour les fonctions scalaires définies par l’utilisateur compilées en mode natif est identique à celle des fonctions définies par l’utilisateur interprétées.  
  
-   [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)  
  
 La procédure stockée [sp_recompile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md) peut être utilisée avec la fonction scalaire définie par l’utilisateur compilée en mode natif. La fonction est alors recompilée à l’aide de la définition qui existe dans les métadonnées.  
  
 L’exemple suivant montre une fonction scalaire définie par l’utilisateur figurant dans la base de données exemple [AdventureWorks2016CTP3](https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks) .  
  
```sql  
CREATE FUNCTION [dbo].[ufnLeadingZeros_native](@Value int)   
RETURNS varchar(8)   
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS   
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'English')  
  
    DECLARE @ReturnValue varchar(8);  
    SET @ReturnValue = CONVERT(varchar(8), @Value);  
       DECLARE @i int = 0, @count int = 8 - LEN(@ReturnValue)  
  
    WHILE @i < @count  
       BEGIN  
            SET @ReturnValue = '0' + @ReturnValue;  
            SET @i += 1  
       END  
  
    RETURN (@ReturnValue);  
  
END  
```  
  
## <a name="calling-user-defined-functions"></a>Appel de fonctions définies par l'utilisateur  
 Les fonctions scalaires définies par l’utilisateur compilées en mode natif sont utilisables dans des expressions, au même titre que des fonctions scalaires intégrées et des fonctions scalaires définies par l’utilisateur interprétées. Les fonctions scalaires définies par l’utilisateur compilées en mode natif peuvent également être utilisées avec l’instruction EXECUTE, dans une instruction Transact-SQL et une procédure stockée compilée en mode natif.  
  
 Vous pouvez utiliser ces fonctions scalaires définies par l’utilisateur dans des procédures stockées compilées en mode natif et des fonctions définies par l’utilisateur compilées en mode natif et dans les cas où les fonctions intégrées sont autorisées. Vous pouvez également utiliser des fonctions scalaires définies par l’utilisateur compilées en mode natif dans des modules Transact-SQL traditionnels.  
  
 Vous pouvez utiliser ces fonctions scalaires définies par l’utilisateur en mode interop dans les cas où une fonction scalaire définie par l’utilisateur interprétée peut être utilisée. Cependant, des limitations de transaction entre conteneurs s’appliquent, comme décrit dans la section **Niveaux d’isolation pris en charge pour les transactions entre conteneurs** dans [Transactions with Memory-Optimized Tables](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)(Transactions avec des tables optimisées en mémoire). Pour plus d’informations sur le mode interop, consultez [Accéder aux tables mémoire optimisées à l’aide du Transact-SQL interprété](../../relational-databases/in-memory-oltp/accessing-memory-optimized-tables-using-interpreted-transact-sql.md).  
  
 Les fonctions scalaires définies par l’utilisateur compilées en mode natif nécessitent un contexte d’exécution explicite. Pour plus d’informations, consultez [Clause EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md). EXECUTE AS CALLER n’est pas pris en charge. Pour plus d’informations, consultez [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md).  
  
 Pour plus d’informations sur la syntaxe prise en charge pour les instructions Transact-SQL Execute et les fonctions scalaires définies par l’utilisateur compilées en mode natif, consultez [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md). Pour plus d’informations sur la syntaxe prise en charge pour l’exécution de fonctions définies par l’utilisateur dans une procédure stockée compilée en mode natif, consultez [Fonctionnalités prises en charge pour les modules T-SQL compilés en mode natif](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).  
  
## <a name="hints-and-parameters"></a>Indicateurs et paramètres  
 La prise en charge d’une table, d’une jonction et d’indicateurs de requête dans des fonctions scalaires définies par l’utilisateur compilées en mode natif est la même que la prise en charge de ces indicateurs dans des procédures stockées compilées en mode natif. Comme pour les fonctions scalaires définies par l’utilisateur interprétées, les indicateurs de requête inclus dans une requête Transact-SQL qui font référence à une fonction scalaire définie par l’utilisateur compilée en mode natif n’impactent pas le plan de requête pour cette fonction définie par l’utilisateur.  
  
 Les paramètres pris en charge pour les fonctions scalaires définies par l’utilisateur compilées en mode natif sont tous les paramètres pris en charge pour les procédures stockées compilées en mode natif, tant que les paramètres sont autorisés pour les fonctions scalaires définies par l’utilisateur. Le paramètre table est un exemple de paramètre pris en charge.  
  
## <a name="schema-bound"></a>Fonction liée à un schéma  
 Les remarques suivantes s’appliquent aux fonctions scalaires définies par l'utilisateur compilées en mode natif  
  
-   Elles doivent être liées au schéma, à l’aide de l’argument WITH SCHEMABINDING dans les fonctions CREATE et ALTER.  
  
-   Elles ne peuvent pas être supprimées ou modifiées si elles sont référencées par une procédure stockée liée à un schéma ou par une fonction définie par l’utilisateur.  
  
## <a name="showplan_xml"></a>SHOWPLAN_XML  
 Les fonctions scalaires définies par l'utilisateur compilées en mode natif prennent en charge SHOWPLAN_XML. Elles sont conformes au schéma SHOWPLAN_XML générales, comme les procédures stockées compilées en mode natif. L’élément de base des fonctions définies par l’utilisateur est `<UDF>`.  
  
 STATISTICS XML n’est pas pris en charge pour les fonctions scalaires définies par l’utilisateur compilées en mode natif. Lorsque vous exécutez une requête faisant référence à la fonction définie par l’utilisateur avec STATISTICS XML activé, le contenu XML est retourné sans la partie de la fonction définie par l’utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Comme avec les procédures stockées compilées en mode natif, les autorisations pour les objets référencés à partir d’une fonction scalaire définie par l’utilisateur compilée en mode natif sont vérifiées à la création de la fonction. La fonction CREATE échoue si l’utilisateur représenté ne dispose pas des autorisations appropriées. Si l’utilisateur représenté ne dispose plus des autorisations appropriées en raison d’une modification des autorisations, les exécutions ultérieures de la fonction définie par l’utilisateur échouent.  
  
 Lorsque vous utilisez une fonction scalaire définie par l’utilisateur compilée en mode natif dans une procédure stockée compilée en mode natif, les autorisations d’exécution de la fonction définie par l’utilisateur sont vérifiées à la création de la procédure externe. Si l’utilisateur représenté par la procédure externe ne dispose pas d’autorisations d’exécution de la fonction définie par l’utilisateur, la création de la procédure stockée échoue. Si l’utilisateur ne dispose plus des autorisations d’exécution en raison d’une modification des autorisations, l’exécution de la procédure externe échoue.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Enregistrer un plan d’exécution au format XML](../../relational-databases/performance/save-an-execution-plan-in-xml-format.md)  
  
  
