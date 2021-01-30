---
description: sp_addtype (Transact-SQL)
title: sp_addtype (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addtype
- sp_addtype_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addtype
ms.assetid: ed72cd8e-5ff7-4084-8458-2d8ed279d817
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ece3c9bb2d8952738f1f66c59b9cd49563ad00a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198390"
---
# <a name="sp_addtype-transact-sql"></a>sp_addtype (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée un type de données alias.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez à la place [Create type](../../t-sql/statements/create-type-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addtype [ @typename = ] type,   
    [ @phystype = ] system_data_type   
    [ , [ @nulltype = ] 'null_type' ] ;  
```  
  
## <a name="arguments"></a>Arguments  
`[ @typename = ] type` Nom du type de données de l’alias. Les noms des types de données alias doivent respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md) et doivent être uniques dans chaque base de données. *type* **sysname**, sans valeur par défaut.  
  
`[ @phystype = ] system_data_type`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Type de données physique ou fourni, sur lequel est basé le type de données de l’alias.*system_data_type* est de **type sysname**, sans valeur par défaut et peut prendre l’une des valeurs suivantes :  
  
||||  
|-|-|-|  
|**bigint**|**binary(n)**|**bit**|  
|**char(n)**|**datetime**|**decimal**|  
|**float**|**image**|**int**|  
|**money**|**nchar(n)**|**ntext**|  
|**numeric**|**nvarchar(n)**|**real**|  
|**smalldatetime**|**smallint**|**smallmoney**|  
|**sql_variant**|**text**|**tinyint**|  
|**uniqueidentifier**|**varbinary(n)**|**varchar(n)**|  
  
 Les guillemets sont obligatoires pour tous les paramètres comportant des espaces ou des signes de ponctuation. Pour plus d’informations sur les types de données disponibles, consultez [types de données &#40;&#41;Transact-SQL ](../../t-sql/data-types/data-types-transact-sql.md).  
  
 *n*  
 Entier non négatif qui indique la longueur du type de données choisi.  
  
 *P*  
 Entier non négatif qui indique le nombre total maximal de chiffres d'un nombre décimal pouvant figurer à gauche et à droite de la virgule décimale. Pour plus d’informations, consultez [decimal et numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md).  
  
 *s*  
 Entier non négatif qui indique le nombre maximal de décimales qui peuvent figurer à droite d'une virgule décimale ; il doit être inférieur ou égal à la précision. Pour plus d’informations, consultez [decimal et numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md).  
  
`[ @nulltype = ] 'null_type'` Indique la façon dont le type de données de l’alias gère les valeurs NULL. *null_type* est de type **varchar (** 8 **)**, avec NULL comme valeur par défaut et doit être placé entre guillemets simples ('null', 'not null’ou’NONULL'). Si *null_type* n’est pas définie explicitement par **sp_addtype**, elle est définie avec la valeur NULL par défaut actuelle. Utilisez la fonction système GETANSINULL pour déterminer la possibilité de valeurs NULL actuellement définie. Il est possible d'ajuster cette valeur au moyen de l'instruction SET ou ALTER DATABASE. La possibilité de valeurs NULL doit être définie explicitement. Si **\@ phystype** est de type **bit** et **\@ NULLtype** n’est pas spécifié, la valeur par défaut n’est pas null.  
  
> [!NOTE]  
>  Le paramètre *null_type* définit uniquement la possibilité de valeur NULL par défaut pour ce type de données. Si la possibilité d'utiliser des valeurs NULL est explicitement définie quand le type de données alias est utilisé lors de la création de la table, elle est prioritaire sur la possibilité de valeurs NULL actuellement définie. Pour plus d’informations, consultez [ALTER TABLE &#40;Transact-sql&#41;](../../t-sql/statements/alter-table-transact-sql.md) et [Create table &#40;transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Un nom de type de données alias doit être unique dans la base de données. Cependant, plusieurs types de données alias portant des noms différents peuvent avoir la même définition.  
  
 L’exécution de **sp_addtype** crée un type de données alias qui apparaît dans la vue de catalogue **sys. types** pour une base de données spécifique. Si le type de données alias doit être disponible dans toutes les nouvelles bases de données définies par l’utilisateur, ajoutez-le au **modèle**. Lorsque qu'un type de données alias est créé, vous pouvez l'utiliser dans CREATE TABLE ou ALTER TABLE, et y lier des valeurs par défaut et des règles. Tous les types de données d’alias scalaires créés à l’aide de **sp_addtype** sont contenus dans le schéma **dbo** .  
  
 Tous les types de données alias héritent du classement par défaut de la base de données. Les classements des colonnes et des variables de types d’alias sont définis dans les [!INCLUDE[tsql](../../includes/tsql-md.md)] instructions CREATE TABLE, ALTER TABLE et DECLARE @*local_variable* . La modification du classement par défaut de la base de données s'applique seulement aux nouvelles colonnes et variables de ce type ; elle ne modifie pas le classement des colonnes et des variables existantes.  
  
> [!IMPORTANT]  
>  À des fins de compatibilité descendante, le rôle de base de données **public** reçoit automatiquement l’autorisation REFERENCES sur les types de données d’alias créés à l’aide de **sp_addtype**. Notez que lorsque vous créez des types de données d’alias à l’aide de l’instruction CREATe TYPE au lieu de **sp_addtype**, cette attribution automatique n’est pas effectuée.  
  
 Les types de données alias ne peuvent pas être définis à l’aide des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] types de données **timestamp**, **table**, **XML**, **varchar (max)**, **nvarchar (max)** ou **varbinary (max)** .  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données fixe **db_owner** ou **db_ddladmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-an-alias-data-type-that-does-not-allow-for-null-values"></a>R. Création d'un type de données alias qui n'accepte pas les valeurs NULL  
 L’exemple suivant crée un type de données alias nommé `ssn` (numéro de sécurité sociale) basé sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] type de données **varchar** fourni. Le type de données `ssn` est utilisé pour les colonnes comportant des numéros de sécurité sociale à 11 chiffres (999-99-9999). Cette colonne ne peut pas avoir la valeur NULL.  
  
 Notez que `varchar(11)` est spécifié entre guillemets car il contient des signes de ponctuation (parenthèses).  
  
```  
USE master;  
GO  
EXEC sp_addtype ssn, 'varchar(11)', 'NOT NULL';  
GO  
```  
  
### <a name="b-creating-an-alias-data-type-that-allows-for-null-values"></a>B. Création d'un type de données alias qui autorise les valeurs NULL  
 L'exemple suivant crée un type de données alias (basé sur `datetime`) appelé `birthday`, qui autorise les valeurs NULL.  
  
```  
USE master;  
GO  
EXEC sp_addtype birthday, datetime, 'NULL';  
```  
  
### <a name="c-creating-additional-alias-data-types"></a>C. Création d'autres types de données alias  
 L'exemple suivant crée deux autres types de données alias, `telephone` et `fax`, pour des numéros de téléphone et de télécopie nationaux et internationaux.  
  
```  
USE master;  
GO  
EXEC sp_addtype telephone, 'varchar(24)', 'NOT NULL';  
GO  
EXEC sp_addtype fax, 'varchar(24)', 'NULL';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)   
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md)   
 [sp_bindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)   
 [sp_bindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md)   
 [sp_droptype &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droptype-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [sp_unbindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)   
 [sp_unbindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindrule-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
