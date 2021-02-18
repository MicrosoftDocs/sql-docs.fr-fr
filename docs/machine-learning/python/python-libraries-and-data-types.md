---
title: Convertir les types de données SQL et Python
description: Passez en revue les conversions de types de données implicites et explicites entre Python et SQL Server dans les solutions de science des données et d’apprentissage automatique.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 99a677874a29b3784ed36f29876a332add21c920
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344434"
---
# <a name="data-type-mappings-between-python-and-sql-server"></a>Mappages de types de données entre Python et SQL Server
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Cet article liste les types de données pris en charge, ainsi que les conversions de types de données exécutées, lors de l’utilisation de la fonctionnalité d’intégration Python dans SQL Server Machine Learning Services.

Python prend en charge un nombre limité de types de données par rapport à SQL Server. Par conséquent, chaque fois que vous utilisez des données de SQL Server dans des scripts Python, les données SQL peuvent être implicitement converties en un type de données Python compatible. Toutefois, la plupart du temps, une conversion exacte ne peut pas être effectuée automatiquement et une erreur est retournée.

## <a name="python-and-sql-data-types"></a>Type de données Python et SQL

Ce tableau répertorie les conversions implicites qui sont fournies. Les autres types de données ne sont pas pris en charge.

| Type SQL             | Type Python | Description |
|----------------------|-------------|-------------|
| **bigint**           | `float64`   |
| **binary**           | `bytes`     |
| **bit**              | `bool`      |
| **char**             | `str`       |
| **date**             | `datetime`  |
| **datetime**         |`datetime`   | Pris en charge avec SQL Server 2017 CU6 et versions ultérieures (avec des tableaux **NumPy** de type `datetime.datetime` ou `pandas.Timestamp` **Pandas**). `sp_execute_external_script` gère maintenant les types `datetime` comportant des fractions de secondes.|
| **float**            | `float64`   |
| **nchar**            | `str`       |
| **nvarchar**         | `str`       |
| **nvarchar(max)**    | `str`       |
| **real**             | `float64`   |
| **smalldatetime**    | `datetime`  |
| **smallint**         | `int32`     |
| **tinyint**          | `int32`     |
| **uniqueidentifier** | `str`       |
| **varbinary**        | `bytes`     |
| **varbinary(max)**   | `bytes`     |
| **varchar(n)**       | `str`       |
| **varchar(max)**     | `str`       |

## <a name="see-also"></a>Voir aussi

+ [Mappages de types de données entre R et SQL Server](../r/r-libraries-and-data-types.md)
