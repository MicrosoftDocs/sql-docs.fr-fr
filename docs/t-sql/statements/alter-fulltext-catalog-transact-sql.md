---
description: ALTER FULLTEXT CATALOG (Transact-SQL)
title: ALTER FULLTEXT CATALOG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_FULLEXT_CATALOG_TSQL
- ALTER FULLEXT CATALOG
dev_langs:
- TSQL
helpviewer_keywords:
- modifying full-text catalogs
- full-text catalogs [SQL Server], rebuilding
- accent sensitivity
- ALTER FULLTEXT CATALOG statement
- full-text catalogs [SQL Server], modifying
- full-text catalogs [SQL Server], reorganizing
ms.assetid: 31a47aaf-6c7f-48a4-a86a-d57aec66c9cb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: aedb9f32a845a7b25d856c8bfa5aff20facbf4a4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204233"
---
# <a name="alter-fulltext-catalog-transact-sql"></a>ALTER FULLTEXT CATALOG (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Modifie les propriétés d'un catalogue de texte intégral.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql 
ALTER FULLTEXT CATALOG catalog_name   
{ REBUILD [ WITH ACCENT_SENSITIVITY = { ON | OFF } ]  
| REORGANIZE  
| AS DEFAULT   
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *CATALOG_NAME*  
 Spécifie le nom du catalogue à modifier. S’il n’existe pas de catalogue portant le nom spécifié, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une erreur et n’exécute pas l’opération ALTER.  
  
 REBUILD  
 Indique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de reconstruire tout le catalogue. Dans ce cas, le catalogue existant est supprimé et un autre catalogue est créé à sa place. Toutes les tables qui comportent des références d'indexation de texte intégral sont associées au nouveau catalogue. La reconstruction redéfinit les métadonnées de texte intégral des tables système de la base de données.  
  
 WITH ACCENT_SENSITIVITY = {ON|OFF}  
 Indique si le catalogue à modifier respecte les accents ou non pour l'indexation et l'exécution de requêtes de texte intégral.  
  
 Pour déterminer si un catalogue de texte intégral respecte, ou non, les accents, utilisez la fonction FULLTEXTCATALOGPROPERTY avec la valeur de propriété **accentsensitivity** sur *catalog_name*. Si cette fonction retourne « 1 », le catalogue de texte intégral respecte les accents ; si elle retourne « 0 », il n'en tient pas compte.  
  
 Le catalogue et la base de données ont par défaut le même comportement quant au respect des accents.  
  
 REORGANIZE  
 Indique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d’effectuer une *fusion principale*, qui consiste à fusionner les plus petits index créés lors de l’indexation afin de constituer un seul grand index. La fusion des fragments d’index de recherche en texte intégral peut améliorer les performances, et libérer des ressource de disque et des ressources mémoire. Si le catalogue de texte intégral fait l'objet de modifications fréquentes, utilisez cette commande régulièrement pour le réorganiser.  
  
 REORGANIZE optimise également les structures d'index et de catalogue internes.  
  
 N'oubliez pas que, selon la quantité de données indexées, une fusion principale peut prendre du temps. La fusion principale d'une grande quantité de données peut créer une transaction dont l'exécution est longue, ce qui retarde la troncation du journal des transactions pendant le point de contrôle. Dans ce cas, la taille du journal des transactions peut s'accroître considérablement en mode de récupération complète. Il est fortement recommandé de vous assurer que votre journal des transactions contient un espace suffisant pour une transaction dont l'exécution est longue avant de réorganiser un index de recherche en texte intégral de grande taille dans une base de données qui utilise le mode de récupération complète. Pour plus d’informations, consultez [Gérer la taille du fichier journal des transactions](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md).  
  
 AS DEFAULT  
 Spécifie que ce catalogue est le catalogue par défaut. Lorsque des index de recherche en texte intégral sont créés sans qu'aucun catalogue n'ait été spécifié, c'est le catalogue par défaut qui est utilisé. S'il existe un catalogue de texte intégral par défaut, l'utilisation de l'argument AS DEFAULT pour ce catalogue remplace le catalogue par défaut existant.  
  
## <a name="permissions"></a>Autorisations  
 L’utilisateur doit disposer de l’autorisation ALTER sur le catalogue de texte intégral, ou bien être membre du rôle serveur fixe sysadmin ou membre des rôles de base de données fixes **db_owner** ou **db_ddladmin**.  
  
> [!NOTE]  
>  Pour utiliser ALTER FULLTEXT CATALOG AS DEFAULT, l'utilisateur doit disposer de l'autorisation ALTER sur le catalogue de texte intégral et de l'autorisation CREATE FULLTEXT CATALOG sur la base de données.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant modifie la propriété `accentsensitivity` du catalogue de texte intégral par défaut `ftCatalog`, qui respecte les accents.  
  
```sql  
--Change to accent insensitive  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT CATALOG ftCatalog   
REBUILD WITH ACCENT_SENSITIVITY=OFF;  
GO  
-- Check Accentsensitivity  
SELECT FULLTEXTCATALOGPROPERTY('ftCatalog', 'accentsensitivity');  
GO  
--Returned 0, which means the catalog is not accent sensitive.  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)   
 [CREATE FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [DROP FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md)   
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)  
  
  
