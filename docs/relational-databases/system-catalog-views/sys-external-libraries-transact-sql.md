---
description: sys.external_libraries (Transact-SQL)
title: sys.external_libraries (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- external_libraries
- external_libraries_TSQL
- sys.external_libraries
- sys.external_libraries_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.external_libraries catalog view
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 6370cfdd213c7670b22066395534928f980766c3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354206"
---
# <a name="sysexternal_libraries-transact-sql"></a>sys.external_libraries (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Prend en charge la gestion des bibliothèques de packages relatives aux runtimes externes tels que R, Python et Java.

> [!NOTE]
> Dans SQL Server 2017, le langage R et la plateforme Windows sont pris en charge. R, Python et Java sur les plateformes Windows et Linux sont pris en charge dans SQL Server 2019 et ultérieur. Sur Azure SQL Managed Instance, R et Python sont pris en charge.

## <a name="sysexternal_libraries"></a>sys.external_libraries

L’affichage catalogue sys.external_libraries répertorie une ligne pour chaque bibliothèque externe qui a été chargée dans la base de données.

|Nom de la colonne |Type de données | Description|
|------|------|------|
|external_library_id |int | ID de l’objet de bibliothèque externe. |
|name |sysname |Nom de la bibliothèque externe. Est unique dans la base de données par propriétaire.|
|principal_id |int |ID du principal propriétaire de cette bibliothèque externe. |
|langage | sysname | Nom de la langue ou du runtime qui prend en charge la bibliothèque externe. Les valeurs valides sont « R », « Python » et « Java ». Des runtimes supplémentaires peuvent être ajoutés à l’avenir.|
|scope |int |0 pour l’étendue publique ; 1 pour l’étendue privée |  
|scope_desc |varchar (7) |Indique si le package est public ou privé|

## <a name="see-also"></a>Voir aussi  

+ [sys.external_library_files](sys-external-library-files-transact-sql.md)  
+ [CRÉER UNE BIBLIOTHÈQUE EXTERNE](../../t-sql/statements/create-external-library-transact-sql.md)  
+ [Installer de nouveaux packages R](../../machine-learning/package-management/install-additional-r-packages-on-sql-server.md)  
+ [Installer de nouveaux packages Python](../../machine-learning/package-management/install-additional-python-packages-on-sql-server.md)  
