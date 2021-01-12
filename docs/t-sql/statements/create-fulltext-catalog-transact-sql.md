---
description: CREATE FULLTEXT CATALOG (Transact-SQL)
title: CREATE FULLTEXT CATALOG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CATALOG_TSQL
- CREATE_FULLTEXT_TSQL
- FULLTEXT_TSQL
- FULLTEXT CATALOG
- CREATE FULLTEXT CATALOG
- CREATE_FULLTEXT_CATALOG_TSQL
- CATALOG
- FULLTEXT_CATALOG_TSQL
- CREATE FULLTEXT
- FULLTEXT
dev_langs:
- TSQL
helpviewer_keywords:
- full-text catalogs [SQL Server], creating
- CREATE FULLTEXT CATALOG statement
ms.assetid: d7a8bd93-e2d7-4a40-82ef-39069e65523b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4d3520d2a9d8c1c90507bb52742bd888eb1925cd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095836"
---
# <a name="create-fulltext-catalog-transact-sql"></a>CREATE FULLTEXT CATALOG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crée un catalogue de texte intégral pour une base de données. Ce catalogue peut avoir plusieurs index de recherche en texte intégral, mais ce type d'index ne peut faire partie que d'un catalogue de texte intégral. Chaque base de données peut contenir plusieurs catalogues de texte intégral ou n'en contenir aucun.  
  
 Vous ne pouvez pas créer des catalogues de texte intégral dans les bases de données **Master**, **model** ou **tempdb**.  
  
> [!IMPORTANT]  
>  À compter de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], un catalogue de texte intégral est un objet virtuel qui n'appartient à aucun groupe de fichiers. Un catalogue de texte intégral est un concept logique qui fait référence à un groupe d'index de recherche en texte intégral.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
CREATE FULLTEXT CATALOG catalog_name  
     [ON FILEGROUP filegroup ]  
     [IN PATH 'rootpath']  
     [WITH <catalog_option>]  
     [AS DEFAULT]  
     [AUTHORIZATION owner_name ]  
  
<catalog_option>::=  
     ACCENT_SENSITIVITY = {ON|OFF}  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *CATALOG_NAME*  
 Nom du nouveau catalogue. Les catalogues doivent se différencier par des noms uniques dans la base de données active. De même, le nom du fichier correspondant au catalogue de texte intégral (voir ON FILEGROUP) doit être unique parmi tous les autres fichiers de la base de données. Si le nom du catalogue est déjà utilisé par un autre catalogue de la base de données, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] renvoie une erreur.  
  
 Le nom du catalogue ne doit pas dépasser 120 caractères.  
  
 ON FILEGROUP *filegroup*  
 À partir de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], cette clause n'a aucun effet.  
  
 IN PATH **'** _rootpath_ **'**  
 > [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 À partir de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], cette clause n'a aucun effet.  
  
 ACCENT_SENSITIVITY = {ON|OFF}  
 Spécifie si le catalogue respecte ou non les accents lors d'une indexation de texte intégral. Si cette propriété est modifiée, l'index doit être reconstitué. Le comportement par défaut est celui qui est spécifié pour les accents dans le classement de base de données. Pour consulter ce classement, utilisez la vue de catalogue **sys.databases**.  
  
 Pour déterminer si un catalogue de texte intégral respecte, ou non, les accents, utilisez la fonction FULLTEXTCATALOGPROPERTY avec la valeur de propriété **accentsensitivity** sur *catalog_name*. Si la valeur renvoyée est « 1 », le catalogue de texte intégral respecte les accents ; si la valeur est « 0 », il ne les respecte pas.  
  
 AS DEFAULT  
 Spécifie que le catalogue est le catalogue par défaut. Lorsque des index de texte intégral sont créés sans qu'un catalogue de texte intégral soit explicitement spécifié, le catalogue par défaut est utilisé. Si un catalogue de texte intégral est déjà marqué AS DEFAULT, le fait de définir le nouveau catalogue AS DEFAULT le désigne comme catalogue de texte intégral par défaut.  
  
 AUTHORIZATION *owner_name*  
 Associe le propriétaire du catalogue de texte intégral au nom d'un utilisateur ou d'un rôle de base de données. Si *owner_name* est un rôle, celui-ci doit être le nom d’un rôle dont l’utilisateur actuel est membre, ou bien l’utilisateur qui exécute l’instruction doit être le propriétaire de la base de données ou l’administrateur système.  
  
 Si *owner_name* est un nom d’utilisateur, ce nom doit être l’un des éléments suivants :  
  
-   le nom de l'utilisateur exécutant l'instruction ;  
  
-   le nom d'un utilisateur pour lequel l'utilisateur exécutant la commande dispose des autorisations d'emprunt d'identité ;  
  
-   l'utilisateur exécutant la commande doit être le propriétaire de la base de données ou l'administrateur système.  
  
 L’autorisation TAKE OWNERSHIP doit également être octroyée à *owner_name* sur le catalogue de texte intégral spécifié.  
  
## <a name="remarks"></a>Remarques  
 Les ID de catalogues de texte intégral commencent à 00005 et sont incrémentés d'une unité à chaque fois qu'un catalogue est créé.  
  
## <a name="permissions"></a>Autorisations  
 L’utilisateur doit disposer de l’autorisation CREATE FULLTEXT CATALOG sur la base de données ou être membre du rôle de base de données fixe **db_owner** ou **db_ddladmin**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée un catalogue de texte intégral ainsi qu'un index de texte intégral.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE FULLTEXT CATALOG ftCatalog AS DEFAULT;  
GO  
CREATE FULLTEXT INDEX ON HumanResources.JobCandidate(Resume) KEY INDEX PK_JobCandidate_JobCandidateID;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)   
 [ALTER FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)   
 [DROP FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md)   
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)   
 
  
  
