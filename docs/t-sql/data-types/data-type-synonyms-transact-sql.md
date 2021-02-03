---
description: Synonymes des types de données (Transact-SQL)
title: Synonymes des types de données (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], synonyms
- alternate names [SQL Server]
- synonyms [SQL Server], data types
ms.assetid: 390eef67-1a49-4185-a971-e07765be9717
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: de20f321f030d0ddfe7e7eb274577f5b4c3d3a70
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202311"
---
# <a name="data-type-synonyms-transact-sql"></a>Synonymes des types de données (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Les synonymes des types de données sont inclus dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la conformité aux normes ISO. Le tableau suivant répertorie les synonymes et les types de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auxquels ils sont mappés.
  
|Synonyme|Type de données système SQL Server|  
|---|---|
|**binary varying**|**varbinary**|  
|**char varying**|**varchar**|  
|**character**|**char**|  
|**character**|**char(1)**|  
|**character(**_n_**)**|**char(n)**|  
|**character varying(**_n_**)**|**varchar(n)**|  
|**Dec**|**decimal**|  
|**Double precision**|**float**|  
|**float**[ **(** _n_ **)** ] for _n_ = 1-7|**real**|  
|**float**[ **(** _n_ **)** ] for _n_ = 8-15|**float**|  
|**integer**|**int**|  
|**national character(**_n_**)**|**nchar(n)**|  
|**national char(**_n_**)**|**nchar(n)**|  
|**national character varying(**_n_**)**|**nvarchar(n)**|  
|**national char varying(**_n_**)**|**nvarchar(n)**|  
|**national text**|**ntext**|  
|**timestamp**|rowversion|  
  
Les synonymes des types de données peuvent être utilisés à la place du nom du type de données de base correspondant dans les instructions DDL (Data Definition Language). Ces instructions incluent la *\@variable* CREATE TABLE, CREATE PROCEDURE et DECLARE. Cependant, la visibilité des synonymes est nulle après la création de l'objet. Une fois l'objet créé, il reçoit le type de données de base associé au synonyme. La consignation ne spécifie pas que le synonyme a été utilisé dans l'instruction ayant créé l'objet.
  
Les objets issus de l'objet d'origine, tels que les expressions ou colonnes de l'ensemble de résultats, reçoivent le type de données de base. Les fonctions de métadonnées ultérieurement utilisant l'objet d'origine ou des objets dérivés font état du type de données de base, non du synonyme, notamment :

* Opérations sur les métadonnées, comme **sp_help** et autres procédures stockées sur le système,
* Affichages des schémas d'information, et
* Opérations de métadonnées d’API d’accès aux données qui indiquent les types de données de table ou les colonnes des jeux de résultats.
  
Vous pouvez, par exemple, créer une table en spécifiant `national character varying` :
  
```sql
CREATE TABLE ExampleTable (PriKey int PRIMARY KEY, VarCharCol national character varying(10))  
```  
  
`VarCharCol` reçoit un type de données **nvarchar(10)** et toutes les fonctions de métadonnées ultérieures signalent la colonne comme étant **nvarchar(10)**. Les fonctions de métadonnées ne les signaleront jamais comme colonne **national character varying(10)**.
  
## <a name="see-also"></a>Voir aussi
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
  
  
