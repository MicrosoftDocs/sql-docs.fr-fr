---
description: Rédiger des instructions Transact-SQL internationales
title: Rédiger des instructions Transact-SQL internationales | Microsoft Docs
ms.custom: ''
ms.date: 04/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- writing international statements
- Transact-SQL international considerations
- international considerations [SQL Server], Transact-SQL
- Database Engine international considerations [SQL Server], Transact-SQL
- statements [SQL Server], international
- database international considerations [SQL Server], Transact-SQL
- dates [SQL Server], international considerations
ms.assetid: f0b10fee-27f7-45fe-aece-ccc3f63bdcdb
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8872cf6e593279a282c34868a2f019ac82e5bf01
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100339492"
---
# <a name="write-international-transact-sql-statements"></a>Rédiger des instructions Transact-SQL internationales
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Les directives suivantes facilitent la transition d'une langue à l'autre des bases de données et applications de bases de données qui utilisent des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] , ou leur permettent de prendre en charge directement plusieurs langues :  

-   À compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] et dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], utilisez :
    -   Les types de données **char**, **varchar** et **varchar(max)** avec un classement activé [UTF-8](../../relational-databases/collations/collation-and-unicode-support.md#utf8) pour que les données soient encodées en UTF-8.
    -   Les types de données **nchar**, **nvarchar** et **nvarchar(max)** avec un classement activé avec [caractère supplémentaire](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters) pour que les données soient encodées en UTF-16. L’utilisation d’un classement sans caractère supplémentaire génère des données encodées en UCS-2.      

    Cela évite les problèmes de conversion de la page de codes. Pour d’autres considérations, consultez [Différences de stockage entre UTF-8 et UTF-16](../../relational-databases/collations/collation-and-unicode-support.md#storage_differences).  

-   À partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], remplacez toutes les utilisations des types de données **char**, **varchar** et **text** par **nchar**, **nvarchar** et **nvarchar(max)**. Si vous utilisez un classement activé avec [caractère supplémentaire](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters), les données sont encodées au format UTF-16. L’utilisation d’un classement sans caractère supplémentaire génère des données encodées en UCS-2. Cela évite les problèmes de conversion de la page de codes. Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md). 

    > [!IMPORTANT]
    > Le type de données **texte** est déconseillé et ne doit pas être utilisé dans les nouveaux travaux de développement. Envisagez de convertir les données **texte** en données **varchar (max)**.
  
-   Lors des comparaisons et opérations sur les mois et jours de la semaine, utilisez la partie numérique de la date plutôt que les chaînes de noms. Selon les paramètres de langue, des noms de mois et de jours de la semaine différents sont retournés. Par exemple, `DATENAME(MONTH,GETDATE())` retourne `May` quand la langue est définie sur Anglais (États-Unis), `Mai` pour l’allemand et `mai` pour le français. Utilisez à la place une fonction comme [DATEPART](../../t-sql/functions/datepart-transact-sql.md) qui utilise le numéro du mois plutôt que son nom. Utilisez les noms DATEPART lorsque vous générez des ensembles de résultats qui seront affichés par un utilisateur, car les noms de date sont souvent plus clairs qu'une représentation numérique. Pour autant, ne codez aucun élément logique dépendant des noms affichés d’une langue spécifique.  
  
-   Pour spécifier des dates dans des comparaisons ou comme entrées d'instructions INSERT ou UPDATE, utilisez des constantes qui sont interprétées de la même manière, quelle que soit la langue définie :  
  
    -   Les applications ADO, OLE DB et ODBC doivent utiliser les clauses ODBC d'échappement de temps, de date et d'horodateur :  
  
         **{ ts'** _yyyy_ **-** _mm_ **-** _dd_ _hh_ **:** _mm_ **:** _ss_ [ **.** _fff_] **'}** tel que : **{ ts'1998-09-24 10:02:20'}**  
  
         **{ d'** _yyyy_ **-** _mm_ **-** _dd_ **'}** tel que : **{ d'1998-09-24'}**
  
         **{ t'** _hh_ **:** _mm_ **:** _ss_ **'}** tel que : **{ t'10:02:20'}**  
  
    -   Les applications qui utilisent d'autres API, ou encore des scripts, des procédures stockées ou des déclencheurs [!INCLUDE[tsql](../../includes/tsql-md.md)] , doivent utiliser les chaînes numériques non séparées. Par exemple, *yyyymmdd* comme 19980924.  
  
    -   Les applications qui utilisent d’autres API, ou encore des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)], des procédures stockées ou des déclencheurs, peuvent également utiliser l’instruction [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md) avec un paramètre de style explicite pour toutes les conversions entre les types de données **time**, **date**, **smalldate**, **datetime**, **datetime2** et **datetimeoffset** et les types de données de chaînes de caractères. Par exemple, l'instruction suivante est interprétée de la même façon, quels que soient les paramètres de connexion concernant la langue et le format de date :  
  
        ```sql  
        SELECT *  
        FROM AdventureWorks2012.Sales.SalesOrderHeader  
        WHERE OrderDate = CONVERT(DATETIME, '20060719', 101)  
        ```  
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)     
[DATEPART &#40;Transact-SQL&#41;](../../t-sql/functions/datepart-transact-sql.md)        
[Prise en charge d'Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md)      
