---
description: SÉQUENCEs (Transact-SQL)
title: SÉQUENCEs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/30/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SEQUENCES
- SEQUENCES_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SEQUENCES view
- INFORMATION_SCHEMA.SEQUENCES view
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 60a542af207d0de4936dda79c2560dc14dac9ba6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185472"
---
# <a name="sequences-transact-sql"></a>SÉQUENCEs (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne une ligne pour chaque séquence qui est accessible par l’utilisateur actuel dans la base de données actuelle.

Pour récupérer des informations de ces vues, spécifiez le nom complet de **INFORMATION_SCHEMA**_.view_name_.

|Nom de la colonne|Type de données|Description|
|-----------------|---------------|-----------------|
|**SEQUENCE_CATALOG**|**nvarchar(128)**|Qualificateur de séquence|
|**SEQUENCE_SCHEMA**|**nvarchar (** 128) * *|Nom du schéma qui contient la séquence|
|**SEQUENCE_NAME**|**nvarchar(128)**|Nom de la séquence|
|**DATA_TYPE**|**nvarchar (** 128 **)**|Type de données de séquence|
|**NUMERIC_PRECISION**|**tinyint**|Précision de la séquence|
|**NUMERIC_PRECISION_RADIX**|**smallint**|Base de précision des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|
|**NUMERIC_SCALE**|**int**|Échelle des données numériques approchées ou exactes, des données de type entier ou monétaire. Renvoie NULL dans les autres cas.|
|**START_VALUE**|**int**|Première valeur qui sera retournée par l'objet séquence.|
|**MINIMUM_VALUE**|**int**|Limites de l’objet séquence. La valeur minimale par défaut d'un nouvel objet séquence correspond à la valeur minimale du type de données de l'objet séquence. Il s’agit de zéro pour le type de données tinyint et d’un nombre négatif pour tous les autres types de données.|
|**MAXIMUM_VALUE**|**int**|Limites de l’objet séquence. La valeur maximale par défaut d'un nouvel objet séquence correspond à la valeur maximale du type de données de l'objet séquence.|
|**LIER**|**int**|Valeur utilisée pour incrémenter (ou décrémenter si la valeur est négative) la valeur de l'objet séquence pour chaque appel à la fonction NEXT VALUE FOR. Si l'incrément est une valeur négative, l'objet séquence décroît ; sinon, il croît. L'incrément ne peut pas avoir la valeur 0. L'incrément par défaut pour un nouvel objet séquence est 1.
|**CYCLE_OPTION**|**int**|Propriété qui spécifie si l'objet séquence doit redémarrer de la valeur minimale (ou maximale pour les objets de séquence décroissante) ou générer une exception lorsque sa valeur minimale ou maximale est dépassée. L'option de cycle par défaut pour les nouveaux objets séquences est NO CYCLE.
|**DECLARED_DATA_TYPE**|**int**|Type de données pour le type de données défini par l’utilisateur.|
|**DECLARED_DATA_PRECISION**|**int**|Précision pour le type de données défini par l’utilisateur.|
|**DECLARED_NUJMERIC_SCALE**|**int**|Échelle numérique pour le type de données défini par l’utilisateur.|

**Exemple** L’exemple suivant retourne des informations sur les schémas de la base de données de test :

```sql
SELECT * FROM test.INFORMATION_SCHEMA.SEQUENCES;
```

## <a name="see-also"></a>Voir aussi

- [Vues de schémas d’informations &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)
- [sys.sequences &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md)
