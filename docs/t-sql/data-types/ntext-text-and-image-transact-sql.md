---
description: Types ntext, text et image (Transact-SQL)
title: ntext, text et image (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ntext_TSQL
- ntext
dev_langs:
- TSQL
helpviewer_keywords:
- text data type, about text data type
- text [SQL Server], data types
- ntext data type
- ntext data type, about ntext data type
- image data type, about image data type
ms.assetid: b0d8769c-7598-4f97-8162-ace5f182b5bc
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: a7f89f1cb8585befbcd04aa03cdf8309cbe07d10
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187903"
---
# <a name="ntext-text-and-image-transact-sql"></a>Types ntext, text et image (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Types de données de longueur fixe et variable, permettant de stocker un grand nombre de caractères Unicode et non-Unicode, ainsi que des données binaires. Les données Unicode utilisent le jeu de caractères UNICODE UCS-2.
  
>**IMPORTANT !**  Les types de données **ntext**, **text** et **image** seront supprimés dans une future version de SQL Server. Évitez d'utiliser ces types de données dans un nouveau développement. Prévoyez de modifier les applications qui les utilisent actuellement. Utilisez plutôt les types de données [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md)et [varbinary(max)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md) .  
  
## <a name="arguments"></a>Arguments
**ntext**  
Données Unicode de longueur variable ne pouvant pas dépasser 2^30 - 1 octets (c'est-à-dire 1 073 741 823). La taille de stockage, en octets, est le double de la longueur de chaîne entrée. Le synonyme ISO de **ntext** est **national text**.
  
**text**  
Données non-Unicode de longueur variable figurant dans la page de codes du serveur et ne pouvant pas dépasser en longueur 2^31 - 1 octets (2 147 483 647). Lorsque la page de codes du serveur utilise des caractères sur deux octets, le stockage est tout de même de 2 147 483 647 octets. En fonction de la chaîne de caractères, la taille de stockage peut être inférieure à 2 147 483 647 octets.
  
**image**  
Données binaires de longueur variable occupant de 0 à 2^31 - 1 (2 147 483 647) octets.
  
## <a name="remarks"></a>Remarques  
Les fonctions et instructions suivantes peuvent être utilisées en conjonction avec les données de type **ntext**, **text** ou **image**.
  
|Fonctions|Instructions|  
|---|---|
|[DATALENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/datalength-transact-sql.md)|[READTEXT &#40;Transact-SQL&#41;](../../t-sql/queries/readtext-transact-sql.md)|  
|[PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)|[SET TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/statements/set-textsize-transact-sql.md)|  
|[SUBSTRING &#40;Transact-SQL&#41;](../../t-sql/functions/substring-transact-sql.md)|[UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)|  
|[TEXTPTR &#40;Transact-SQL&#41;](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md)|[WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)|  
|[TEXTVALID &#40;Transact-SQL&#41;](../../t-sql/functions/text-and-image-functions-textvalid-transact-sql.md)||  
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Conversion de type de données &#40;moteur de base de données&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[Prise en charge d'Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md)

