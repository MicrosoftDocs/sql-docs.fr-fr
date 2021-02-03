---
description: SET SHOWPLAN_XML (Transact-SQL)
title: SET SHOWPLAN_XML (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET SHOWPLAN_XML
- SET_SHOWPLAN_XML_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- statements [SQL Server], estimates
- SET SHOWPLAN_XML statement
- execution information and estimates [SQL Server]
- statements [SQL Server], information without processing
- XML [SQL Server], statement execution information
- SHOWPLAN_XML option
- estimated execution information [SQL Server]
ms.assetid: a467a1b3-10a5-43c4-9085-13d8aed549c9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d0065350fbf3c3c774602f28aeb98d6e7685df4c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206953"
---
# <a name="set-showplan_xml-transact-sql"></a>SET SHOWPLAN_XML (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Empêche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'exécuter des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]. À la place, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne des informations détaillées sur le mode d’exécution des instructions, sous la forme d’un document XML bien défini.

![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```syntaxsql
SET SHOWPLAN_XML { ON | OFF }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Notes

L'option SET SHOWPLAN_XML est définie lors de l'exécution, et non pas durant l'analyse.

Si SET SHOWPLAN_XML a la valeur ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne des informations sur le plan d’exécution de chaque instruction sans l’exécuter, et les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ne sont pas exécutées. Une fois cette option activée, des informations sur le plan d'exécution de toutes les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes sont retournées jusqu'à sa désactivation (OFF). Par exemple, si une instruction CREATE TABLE est exécutée alors que l'option SET SHOWPLAN_XML est activée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne un message d'erreur pour toute instruction SELECT ultérieure se rapportant à cette table car elle n'existe pas. Par conséquent, les prochaines références à cette table échoueront. Lorsque SET SHOWPLAN_XML a la valeur OFF, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécute les instructions sans générer de rapport.

SET SHOWPLAN_XML est censé retourner le résultat en **nvarchar(max)** pour des applications telles que l’utilitaire **sqlcmd**, où la sortie XML est utilisée ultérieurement par d’autres outils pour afficher et traiter les informations du plan de requête.

> [!NOTE]
> La vue de gestion dynamique, **sys.dm_exec_query_plan**, retourne les mêmes informations que SET SHOWPLAN XML dans le type de données **xml**. Ces informations sont retournées de la colonne **query_plan** de **sys.dm_exec_query_plan**. Pour plus d’informations, consultez [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md).

SET SHOWPLAN_XML ne peut pas être spécifié dans une procédure stockée. Elle doit être la seule instruction d'un traitement.

SET SHOWPLAN_XML retourne les informations sous forme d'un jeu de documents XML. Chaque traitement après l'instruction SET SHOWPLAN_XML ON est présenté dans le résultat par un document unique. Chaque document contient le texte des instructions du traitement, suivi des détails des étapes de l'exécution. Le document présente les coûts estimés, le nombre de lignes, les index accédés et les types d'opérateurs utilisés, l'ordre de jointure et d'autres informations relatives aux plans d'exécution.

> [!NOTE]
> Si l’option **Inclure le plan d’exécution réel** est sélectionnée dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], cette option SET ne produit aucune sortie de la représentation au format XML. Désactivez l’option **Inclure le plan d’exécution réel** avant d’utiliser l’option SET.

### <a name="location-of-showplan-output"></a>Emplacement de la sortie de SHOWPLAN

Le document contenant le schéma XML pour la sortie XML provenant de SET SHOWPLAN_XML est copié pendant l'installation sur un répertoire local de l'ordinateur sur lequel Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé. Le document se trouve sur le lecteur contenant les fichiers d’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], dans un chemin similaire à celui-ci :

- `\Microsoft SQL Server\130\Tools\Binn\schemas\sqlserver\2004\07\showplan\showplanxml.xsd`

Dans le chemin précédent, le nœud `130\` est utilisé par SQL Server 2016. Le nombre 130 est dérivé du premier nœud de la valeur retournée par `SELECT @@VERSION`, qui est 13. Pour SQL Server 2017, le chemin utilise `140\`, car le premier nœud de sa valeur pour @@VERSION est 14. Pour SQL Server 2019, la première valeur de @@VERSION est 15.

Le schéma Showplan est également disponible sur [ce site web](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409).

## <a name="permissions"></a>Autorisations

Pour utiliser SET SHOWPLAN_XML, vous devez disposer des autorisations suffisantes pour exécuter les instructions sur lesquelles SET SHOWPLAN_XML est exécuté, ainsi que de l'autorisation SHOWPLAN pour toutes les bases de données contenant les objets auxquels elles font référence.

Concernant les instructions SELECT, INSERT, UPDATE, DELETE, EXEC *stored_procedure* et EXEC *user_defined_function*, pour produire une sortie Showplan, l’utilisateur doit :

- disposer des autorisations appropriées pour exécuter les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ;

- disposer de l'autorisation SHOWPLAN sur toutes les bases de données contenant les objets auxquels les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] font référence, tels que les tables, les vues, etc.

Pour toutes les autres instructions, notamment DDL, USE *database_name*, SET, DECLARE, SQL dynamique, etc., seules les autorisations appropriées pour exécuter les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] sont nécessaires.

## <a name="examples"></a>Exemples

Les deux instructions suivantes utilisent l'option SET SHOWPLAN_XML, activée puis désactivée, pour montrer comment [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] analyse et optimise l'utilisation des index dans les requêtes.

La première requête utilise l'opérateur de comparaison Égal à (=) dans la clause WHERE sur une colonne indexée. La seconde requête utilise l'opérateur LIKE dans la clause WHERE. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit utiliser une analyse d'index cluster et rechercher les données répondant à la condition spécifiée par la clause WHERE. Les valeurs des attributs **EstimateRows** et **EstimatedTotalSubtreeCost** sont plus petites pour la première requête indexée, ce qui indique qu’elle est traitée beaucoup plus rapidement que la requête non indexée, tout en utilisant moins de ressources.

```sql
USE AdventureWorks2012;
GO
SET SHOWPLAN_XML ON;
GO
-- First query.
SELECT BusinessEntityID
FROM HumanResources.Employee
WHERE NationalIDNumber = '509647174';
GO
-- Second query.
SELECT BusinessEntityID, JobTitle
FROM HumanResources.Employee
WHERE JobTitle LIKE 'Production%';
GO
SET SHOWPLAN_XML OFF;
```

## <a name="see-also"></a>Voir aussi

[Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)
