---
description: ASSEMBLYPROPERTY (Transact-SQL)
title: ASSEMBLYPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ASSEMBLYPROPERTY_TSQL
- ASSEMBLYPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- ASSEMBLYPROPERTY statement
- assemblies [CLR integration], properties
ms.assetid: cf03d1b1-724c-48bf-a8df-3fe2586b150a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9e466b8d66760704ce7ee90028210dfa5dfabc22
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88417475"
---
# <a name="assemblyproperty-transact-sql"></a>ASSEMBLYPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette fonction retourne des informations sur une propriété d’un assembly.
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
ASSEMBLYPROPERTY('assembly_name', 'property_name')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*assembly_name*  
Nom de l'assembly.
  
*property_name*  
Nom de la propriété sur laquelle des informations doivent être extraites. *property_name* peut avoir l’une des valeurs suivantes :
  
|Valeur|Description|  
|---|---|
|**CultureInfo**|Paramètres régionaux de l'assembly.|  
|**PublicKey**|Clé publique ou jeton de clé publique de l'assembly.|  
|**MvID**|Numéro d'identification de version complet de l'assembly, qui est généré par le compilateur.|  
|**VersionMajor**|Composant principal (première partie) du numéro d'identification de version en quatre parties de l'assembly.|  
|**VersionMinor**|Composant mineur (deuxième partie) du numéro d'identification de version en quatre parties de l'assembly.|  
|**VersionBuild**|Composant de version (troisième partie) du numéro d'identification de version en quatre parties de l'assembly.|  
|**VersionRevision**|Composant de révision (quatrième partie) du numéro d'identification de version en quatre parties de l'assembly.|  
|**SimpleName**|Nom simple de l'assembly.|  
|**Architecture**|Architecture du processeur de l'assembly.|  
|**CLRName**|Chaîne canonique qui encode le nom simple, le numéro de version, les paramètres régionaux, la clé publique, et l'architecture de l'assembly. Cette valeur identifie de façon univoque l'assembly du côté CLR (Common Language Runtime).|  
  
## <a name="return-type"></a>Type de retour
**sql_variant**
  
## <a name="examples"></a>Exemples  
Cet exemple suppose qu'un assembly `HelloWorld` est enregistré dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Pour plus d’informations, consultez [Exemple Hello World](https://msdn.microsoft.com/library/fed6c358-f5ee-4d4c-9ad6-089778383ba7).
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ASSEMBLYPROPERTY ('HelloWorld' , 'PublicKey');  
```  
  
## <a name="see-also"></a>Voir aussi
[CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)  
[DROP ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-assembly-transact-sql.md)
  
  
