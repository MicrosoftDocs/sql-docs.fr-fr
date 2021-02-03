---
description: '&#x40;&#x40;Version - Fonctions de configuration Transact SQL'
title: '@@VERSION (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 03/20/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@VERSION'
- '@@VERSION_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@VERSION function'
- current SQL Server installation information
- versions [SQL Server], @@VERSION
- processors [SQL Server], types
ms.assetid: 385ba80e-7c28-41a5-9cdb-5648f3785983
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 39bf4fb339f27cd62929e5bc216e2bc27ad03def
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181808"
---
# <a name="x40x40version---transact-sql-configuration-functions"></a>&#x40;&#x40;Version - Fonctions de configuration Transact SQL
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne des informations système et de build pour l'installation actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
@@VERSION  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 **nvarchar**  
  
## <a name="remarks"></a>Notes  
 Les résultats de @@VERSION sont présentés sous la forme d’une chaîne nvarchar individuelle. Vous pouvez utiliser la fonction [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md) pour récupérer les valeurs de propriétés individuelles.  
  
 Pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les informations suivantes sont retournées.  
  
-   Version [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Architecture du processeur  
  
-   Date de build de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Déclaration de copyright  
  
-   Édition de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Version du système d'exploitation  
  
 Pour [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], les informations suivantes sont retournées.  
  
-   Édition : « Microsoft SQL Azure »  
  
-   Niveau du produit : « (RTM) »  
  
-   Version du produit  
  
-   Date de génération  
  
-   Déclaration de copyright  

> [!NOTE]  
> Nous savons que la version du produit signalée par @@VERSION pour Azure SQL Database est incorrecte. La version du moteur de base de données SQL Server exécutée par Azure SQL Database est toujours ultérieure à la version locale de SQL Server, et inclut les derniers correctifs de sécurité. Cela signifie que le niveau du correctif est toujours égal ou supérieur à celui de la version locale de SQL Server, et que les dernières fonctionnalités disponibles dans SQL Server sont disponibles dans Azure SQL Database.
>
> Pour déterminer l’édition du moteur par programmation, utilisez SELECT SERVERPROPERTY('EngineEdition'). Cette requête retourne « 5 » pour Azure SQL Database et « 8 » pour Azure SQL Managed Instance.
>
> Nous mettrons à jour la documentation lorsque ce problème aura été résolu.

  
## <a name="examples"></a>Exemples  
  
### <a name="a-return-the-current-version-of-ssnoversion"></a>A : Renvoi de la version actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 L'exemple suivant retourne les informations de version de l'installation actuelle.  
  
```sql
SELECT @@VERSION AS 'SQL Server Version';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-return-the-current-version-of-ssdw"></a>B. Renvoi de la version actuelle de [!INCLUDE[ssDW](../../includes/ssdw-md.md)]  
  
```sql
SELECT @@VERSION AS 'SQL Server PDW Version';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)  
  
  

