---
description: nchar et nvarchar (Transact-SQL)
title: nchar et nvarchar (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- nvarchar data type
- nchar data type
ms.assetid: 81ee5637-ee31-4c4d-96d0-56c26a742354
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 038cb4e80af0423a5581ef2b7b51c1bbc54bb5cc
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237628"
---
# <a name="nchar-and-nvarchar-transact-sql"></a>nchar et nvarchar (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Les types de données de caractères qui sont soit de taille fixe, **nchar**, soit de taille variable, **nvarchar**. À partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], quand un classement prenant en charge les [caractères supplémentaires (SC)](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters) est utilisé, ces types de données stockent la plage complète des données caractères [Unicode](../../relational-databases/collations/collation-and-unicode-support.md#Unicode_Defn) et utilisent le codage de caractères [UTF-16 ](https://www.wikipedia.org/wiki/UTF-16). Si un classement autre que SC est spécifié, ces types de données stockent uniquement le sous-ensemble de données caractères pris en charge par le codage de caractères [UCS-2](https://www.wikipedia.org/wiki/Universal_Coded_Character_Set#Encoding_forms).

## <a name="arguments"></a>Arguments
**nchar** [ ( n ) ]  
Données de type chaîne de taille fixe. *n* définit la taille de chaîne en paires d’octets et doit être une valeur comprise entre 1 et 4 000. La taille de stockage est le double de *n* octets. Pour l'encodage [UCS-2](https://www.wikipedia.org/wiki/UTF-16#U+0000_to_U+D7FF_and_U+E000_to_U+FFFF), la taille de stockage est deux fois *n* octets et le nombre de caractères pouvant être stockés est également *n*. Pour l'encodage UTF-16, la taille de stockage est toujours deux fois *n* octets, mais le nombre de caractères pouvant être stockés peut être inférieur à *n*, car les caractères supplémentaires utilisent deux paires d'octets (également appelées [paires de remplacement](https://www.wikipedia.org/wiki/UTF-16#U+010000_to_U+10FFFF)). Les synonymes ISO de **nchar** sont **national char** et **national character**.
  
**nvarchar** [ ( n | **max** ) ]  
Données de type chaîne de taille variable. *n* définit la taille de chaîne en paires d’octets et peut être une valeur comprise entre 1 et 4 000. **max** indique que la taille de stockage maximale est de 2^30-1 caractères (2 Go). La taille de stockage est deux fois *n* octets + 2 octets. Pour l'encodage [UCS-2](https://www.wikipedia.org/wiki/UTF-16#U+0000_to_U+D7FF_and_U+E000_to_U+FFFF), la taille de stockage est deux fois *n* octets + 2 octests et le nombre de caractères pouvant être stockés est également *n*. Pour l'encodage UTF-16, la taille de stockage est toujours deux fois *n* octets + 2 octets, mais le nombre de caractères pouvant être stockés peut être inférieur à *n*, car les caractères supplémentaires utilisent deux paires d'octets (également appelées [paires de remplacement](https://www.wikipedia.org/wiki/UTF-16#U+010000_to_U+10FFFF)). Les synonymes ISO de **nvarchar** sont **national char varying** et **national character varying**.
  
## <a name="remarks"></a>Remarques  
Contrairement à l’idée fausse qui circule sur [NCHAR(*n*) et NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), *n* ne définit pas le nombre de caractères. Dans [NCHAR(*n*) et NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), *n* définit la longueur de chaîne en **paires d’octets** (0-4 000). *n* ne définit jamais des nombres de caractères pouvant être stockés. Cette définition est similaire à celle de [CHAR(*n*) et de VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md).   
En fait, lors de l’utilisation de caractères définis dans la plage Unicode 0-65 535, un caractère peut être stocké pour chaque paire d’octets, d’où cette idée fausse. Toutefois, dans les plages Unicode supérieures (65 536-1 114 111), un caractère peut utiliser deux paires d’octets. Par exemple, dans une colonne définie en tant que NCHAR(10), [!INCLUDE[ssde_md](../../includes/ssde_md.md)] peut stocker 10 caractères qui utilisent une paire d’octets (plage Unicode 0-65 535), mais moins de 10 caractères lors de l’utilisation de deux paires d’octets (plage Unicode 65 536-1 114 111). Pour plus d’informations sur le stockage Unicode et les plages de caractères, consultez [Différences de stockage entre UTF-8 et UTF-16](../../relational-databases/collations/collation-and-unicode-support.md#storage_differences).     

Quand la valeur de *n* n’est spécifiée ni dans une définition de données ni dans une instruction de déclaration de variable, la longueur par défaut est 1. Quand la valeur de *n* n’est pas précisée avec la fonction CAST, la longueur par défaut est 30.

Si vous utilisez **nchar** ou **nvarchar**, nous vous recommandons de procéder comme suit :
- Utilisez **nchar** quand les tailles des entrées de données de la colonne sont cohérentes.  
- Utilisez **nvarchar** quand les tailles des entrées de données de la colonne varient considérablement.  
- Utilisez **nvarchar(max)** quand les tailles des entrées de données de la colonne varient considérablement et que la longueur de chaîne peut dépasser 4 000 paires d’octets.  
  
**sysname** est un type de données défini par l’utilisateur fourni par le système qui présente la même fonctionnalité que **nvarchar(128)**, à la différence qu’il n’accepte pas la valeur Null. **sysname** est utilisé pour faire référence aux noms des objets de base de données.
  
Les objets qui utilisent **nchar** ou **nvarchar** reçoivent le classement par défaut de la base de données, sauf si un classement spécifique est affecté à l’aide de la clause COLLATE.
  
SET ANSI_PADDING a toujours la valeur ON pour **nchar** et **nvarchar**. SET ANSI_PADDING OFF ne s’applique pas aux types de données **nchar** ni **nvarchar**.
  
Préfixer une constante de chaîne de caractères Unicode avec la lettre N pour signaler une entrée UCS-2 ou UTF-16, selon qu'un classement SC est utilisé ou non. Sans le préfixe N, la chaîne est convertie en page de codes par défaut de la base de données et ne reconnaîtra peut-être pas certains caractères. En commençant par [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], lorsqu’un classement prenant en charge UTF-8 est utilisé, la page de codes par défaut est capable de stocker le jeu de caractères UNICODE UTF-8. 
 
> [!NOTE]  
> Lorsque vous préfixez une constante de chaîne avec la lettre N, la conversion implicite donnera une chaîne UCS-2 ou UTF-16 si la constante à convertir ne dépasse pas la longueur maximale pour le type de données de chaîne nvarchar (4 000). Sinon, la conversion implicite génère un nvarchar(max) de valeur élevée.
  
> [!WARNING]  
> Chaque colonne **varchar(max)** ou **nvarchar(max)** non Null demande 24 octets d’allocation fixe supplémentaire calculée par rapport à la limite de 8 060 octets par ligne pendant une opération de tri. Ces octets supplémentaires peuvent produire une limite implicite du nombre de colonnes **varchar(max)** ou **nvarchar(max)** non Null dans une table. Aucune erreur spéciale n'est fournie quand la table est créée (mis à part l’avertissement habituel indiquant que la taille maximale de ligne dépasse la taille maximale autorisée de 8 060 octets) ou quand les données sont insérées. Cette grande taille de ligne peut provoquer des erreurs (comme l’erreur 512) que les utilisateurs peuvent ne pas anticiper au cours d’opérations normales.  Deux exemples d’opérations sont la mise à jour d’une clé d’index cluster ou le tri de l’intégralité du jeu de colonnes.
  
## <a name="converting-character-data"></a>Conversion des données de type caractère  
Pour plus d’informations sur la conversion de données caractères, consultez [char et varchar &#40;Transact-SQL&#41;](../../t-sql/data-types/char-and-varchar-transact-sql.md).
  
## <a name="see-also"></a>Voir aussi
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[COLLATE &#40;Transact-SQL&#41;](../statements/collations.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)  
[SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)    
[Prise en charge d'Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md)     
[Jeux de caractères codés sur un octet et multioctets](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets)  
