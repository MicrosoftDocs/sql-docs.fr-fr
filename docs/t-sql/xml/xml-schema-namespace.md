---
description: xml_schema_namespace
title: xml_schema_namespace (Transact-SQL)
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- xml_schema_namespace_TSQL
- xml_schema_namespace
dev_langs:
- TSQL
helpviewer_keywords:
- XML schema collections [SQL Server], reconstructing schemas
- xml_schema_namespace function
- reconstructing schemas
- schemas [SQL Server], XML
- schema collections [SQL Server], reconstructing schemas
ms.assetid: ee9873d8-dd3a-4bff-a10c-68bbadbdf1a6
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 865f2548293fc572a56c90b0813bb83aa2776cbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181694"
---
# <a name="xml_schema_namespace"></a>xml_schema_namespace
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Reconstruit tous les schémas ou un schéma particulier dans la collection de schémas XML. Cette fonction renvoie une instance de type de données **xml** .  
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
xml_schema_namespace( Relational_schema , XML_schema_collection_name , [ Namespace ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *Relational_schema*  
 Nom du schéma relationnel. *Relational_schema* est de type **sysname**.  
  
 *XML_schema_collection_name*  
 Nom de la collection de schémas XML à reconstruire. *XML_schema_collection_name* est de type **sysname**.  
  
 *Espace de noms*  
 Espace de noms URI du schéma XML que vous voulez reconstruire. Il est limité à 1 000 caractères. Si cet argument n'est pas fourni, l'ensemble de la collection de schémas XML est reconstruit. *Namespace* est de type **nvarchar(4000)** .  
  
## <a name="return-types"></a>Types de retour  
 **xml**  
  
## <a name="remarks"></a>Notes  
 Lorsque vous importez des composants de schéma XML dans la base de données à l’aide de [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md) ou [ALTER XML SCHEMA COLLECTION](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md), les aspects du schéma utilisé pour la validation sont conservés. Par conséquent, le schéma reconstruit peut être lexicalement différent du document du schéma d'origine. En particulier, les commentaires, les espaces et les annotations sont perdus ; les informations de type implicite deviennent explicites. Par exemple, \<xs:element name="e1" /> devient \<xs:element name="e1" type="xs:anyType"/>. Également, les préfixes des espaces de noms ne sont pas conservés.  
  
 Si vous spécifiez un paramètre d'espace de noms, le document du schéma résultant contient les définitions de tous les composants de schéma dans cet espace de noms, même s'ils ont été ajoutés dans des documents de schémas différents ou dans des étapes DDL, ou dans les deux.  
  
 Vous ne pouvez pas utiliser cette fonction pour construire des documents de schémas XML à partir de la collection de schémas XML **sys.sys**.  
  
## <a name="examples"></a>Exemples  
 Le code exemple suivant extrait la collection de schémas XML `ProductDescriptionSchemaCollection` du schéma relationnel de production dans la base de données `AdventureWorks`.  
  
```sql
USE AdventureWorks;  
GO  
SELECT xml_schema_namespace(N'production',N'ProductDescriptionSchemaCollection');  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher une collection de schémas XML stockée](../../relational-databases/xml/view-a-stored-xml-schema-collection.md)   
 [Collections de schémas XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-schema-collections-sql-server.md)  
  
  
