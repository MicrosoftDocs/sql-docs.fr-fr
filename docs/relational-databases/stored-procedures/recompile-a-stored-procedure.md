---
title: Recompiler une procédure stockée | Microsoft Docs
description: Découvrez comment recompiler une procédure stockée dans SQL Server 2019 (15.x) à l’aide de Transact-SQL.
ms.custom: ''
ms.date: 10/28/2019
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- sp_recompile
- WITH RECOMPILE clause
- recompiling stored procedures
- stored procedures [SQL Server], recompiling
ms.assetid: b90deb27-0099-4fe7-ba60-726af78f7c18
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 93974ff7f7ef5d0dd28ae8ce6989da2f0b496548
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250240"
---
# <a name="recompile-a-stored-procedure"></a>Recompiler une procédure stockée
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Cette rubrique explique comment recompiler une procédure stockée dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Il existe trois manières d’effectuer cette opération : via l’option **WITH RECOMPILE** dans la définition de la procédure ou lorsque la procédure est appelée, via l’indicateur de requête **RECOMPILE** sur des instructions ou en utilisant la procédure stockée système **sp_recompile** . Cette rubrique décrit l’utilisation de l’option WITH RECOMPILE lors de la création d’une définition de procédure et de l’exécution d’une procédure existante. Elle décrit également l’utilisation de la procédure stockée système sp_recompile pour recompiler une procédure existante.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour recompiler une procédure stockée à l'aide de :**  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Quand une procédure est compilée pour la première fois ou recompilée, son plan de requête est optimisé pour l’état actuel de la base de données et de ses objets. Si une base de données subit des modifications significatives au niveau de ses données ou de sa structure, la recompilation de la procédure a pour effet de mettre à jour et d’optimiser son plan de requête en fonction de ces modifications. Les performances de traitement de la procédure peuvent s’en trouver améliorées.  
  
-   Parfois, la recompilation de procédure doit être forcée, et parfois elle se produit automatiquement. La recompilation automatique se produit chaque fois que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] redémarre. Elle se produit également si une table sous-jacente référencée par la procédure a été modifiée au niveau de sa conception physique.  
  
-   Une autre raison pour forcer la recompilation d'une procédure est de contrer le comportement de détection des paramètres de la compilation de la procédure. Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécute des procédures, les valeurs des paramètres utilisés par la procédure lors de sa compilation sont incluses dans le cadre de la génération du plan de requête. Si ces valeurs représentent les valeurs standard utilisées pour appeler ultérieurement la procédure, celle-ci bénéficie du plan de requête à chaque compilation et exécution. Si les valeurs des paramètres de la procédure sont atypiques, l'application forcée d'une recompilation et un nouveau plan défini en fonction des valeurs des paramètres peuvent améliorer les performances.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet la recompilation des procédures au niveau de l’instruction. Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recompile des procédures, seule l'instruction ayant provoqué la recompilation est compilée, et non la procédure toute entière.  
  
-   Si certaines requêtes d'une procédure utilisent régulièrement des valeurs atypiques ou temporaires, les performances des procédures peuvent être améliorées en utilisant l'indicateur de requête RECOMPILE à l'intérieur de ces requêtes. Étant donné que seules les requêtes utilisant l'indicateur de requête seront recompilées au lieu de la procédure complète, le comportement de recompilation de l'instruction de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]est reproduit. Cependant, en plus d'utiliser les valeurs des paramètres actuels de la procédure, l'indicateur de requête RECOMPILE utilise également les valeurs des variables locales à l'intérieur de la procédure stockée lorsque vous compilez l'instruction. Pour plus d’informations, consultez [Indicateur de requête (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md).  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 **WITH RECOMPILE**  
 Si cette option est utilisée lorsque la définition de la procédure est créée, elle nécessite l'autorisation CREATE PROCEDURE dans la base de données et l'autorisation ALTER sur le schéma dans lequel la procédure est créée.  
  
 Si cette option est utilisée dans une instruction EXECUTE, elle nécessite des autorisations EXECUTE sur la procédure. Aucune autorisation n'est requise sur l'instruction EXECUTE elle-même, mais une autorisation est requise sur la procédure référencée dans l'instruction EXECUTE. Pour plus d’informations, consultez [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md).  
  
 Indicateur de requête **RECOMPILE**  
 Cette fonctionnalité est utilisée lorsque la procédure est créée et l’indicateur est inclus dans les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] de la procédure. Par conséquent, elle nécessite l'autorisation CREATE PROCEDURE dans la base de données et l'autorisation ALTER sur le schéma dans lequel la procédure est créée.  
  
 Procédure stockée système **sp_recompile**  
 Nécessite l'autorisation ALTER pour la procédure spécifiée.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  

1. Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
1. Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
1. Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple crée la définition de la procédure.  

   ```sql
   USE AdventureWorks2012;  
   GO  
   IF OBJECT_ID ( 'dbo.uspProductByVendor', 'P' ) IS NOT NULL   
       DROP PROCEDURE dbo.uspProductByVendor;  
   GO  
   CREATE PROCEDURE dbo.uspProductByVendor @Name varchar(30) = '%'  
   WITH RECOMPILE  
   AS  
       SET NOCOUNT ON;  
       SELECT v.Name AS 'Vendor name', p.Name AS 'Product name'  
       FROM Purchasing.Vendor AS v   
       JOIN Purchasing.ProductVendor AS pv   
         ON v.BusinessEntityID = pv.BusinessEntityID   
       JOIN Production.Product AS p   
         ON pv.ProductID = p.ProductID  
       WHERE v.Name LIKE @Name;  
   ```  
  
### <a name="to-recompile-a-stored-procedure-by-using-the-with-recompile-option"></a>Pour recompiler une procédure stockée à l'aide de l'option WITH RECOMPILE   
  
Sélectionnez **Nouvelle requête**, puis copiez et collez l’exemple de code suivant dans la fenêtre de requête et cliquez sur **Exécuter**. La procédure est alors exécutée et son plan de requête recompilé.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXECUTE HumanResources.uspProductByVendor WITH RECOMPILE;  
GO
```  
  
### <a name="to-recompile-a-stored-procedure-by-using-sp_recompile"></a>Pour recompiler une procédure stockée à l'aide de sp_recompile  

Sélectionnez **Nouvelle requête**, puis copiez et collez l’exemple suivant dans la fenêtre de requête et cliquez sur **Exécuter**. Cela n'exécute pas la procédure mais la marque pour la recompilation de sorte que son plan de requête sera mis à jour à la prochaine exécution.  

```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_recompile N'dbo.uspProductByVendor';   
GO
```  
  
## <a name="see-also"></a>Voir aussi  
 [Créer une procédure stockée](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [Modifier une procédure stockée](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)   
 [Renommer une procédure stockée](../../relational-databases/stored-procedures/rename-a-stored-procedure.md)   
 [Afficher la définition d'une procédure stockée](../../relational-databases/stored-procedures/view-the-definition-of-a-stored-procedure.md)   
 [Afficher les dépendances d’une procédure stockée](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md)   
 [DROP PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-procedure-transact-sql.md)  
  
  
