---
title: Nouveautés des extensions de langage SQL Server
titleSuffix: ''
description: Découvrez les nouveautés des extensions de langage SQL Server qui développent, étendent et améliorent l’intégration entre les langages externes et la plateforme de données.
author: dphansen
ms.author: davidph
ms.date: 11/09/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: e72b202230573352eca54bd8bc9d2afc0a58f29e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070549"
---
# <a name="whats-new-in-sql-server-language-extensions"></a>Nouveautés des extensions de langage SQL Server
[!INCLUDE [SQL Server 2019 and later](../includes/applies-to-version/sqlserver2019.md)]

Des fonctionnalités propres aux [extensions de langage](language-extensions-overview.md) sont ajoutées à chaque version de SQL Server, car nous continuons à développer, étendre et approfondir l’intégration entre les langages externes et la plateforme de données.

## <a name="sql-server-2019"></a>SQL Server 2019

Vous trouverez ci-dessous les nouvelles fonctionnalités [d’extensions de langage](language-extensions-overview.md) de SQL Server 2019. Pour plus d’informations sur toutes les fonctionnalités de cette version, consultez [Nouveautés de SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md) et [Notes de publication pour SQL Server 2019](../sql-server/sql-server-version-15-release-notes.md).

### <a name="new-python-and-r-language-extensions"></a>Nouvelles extensions de langage Python et R

- Un [runtime personnalisé Python](../machine-learning/install/custom-runtime-python.md) est disponible avec les extensions de langage. Pour plus d’informations, consultez [Guide pratique pour installer un runtime personnalisé Python sur Windows](../machine-learning/install/custom-runtime-python.md?view=sql-server-ver15&preserve-view=true) ou [Guide pratique pour installer un runtime personnalisé Python sur Linux](../machine-learning/install/custom-runtime-python.md?view=sql-server-linux-ver15&preserve-view=true).

- Un [runtime personnalisé R](../machine-learning/install/custom-runtime-r.md) est disponible avec les extensions de langage. Pour plus d’informations, consultez [Guide pratique pour installer un runtime personnalisé R sur Windows](../machine-learning/install/custom-runtime-r.md?view=sql-server-ver15&preserve-view=true) ou [Guide pratique pour installer un runtime personnalisé R sur Linux](../machine-learning/install/custom-runtime-r.md?view=sql-server-linux-ver15&preserve-view=true).

### <a name="new-java-language-extension"></a>Nouvelle extension de langage Java

- Le runtime Java par défaut sur Windows et Linux est Open Zulu JRE. Il est fourni lors de l’installation des extensions de langage SQL Server [sur Windows](install/windows-java.md) et [sur Linux](../linux/sql-server-linux-setup-language-extensions-java.md).
- [Types de données Java](how-to/java-to-sql-data-types.md) pris en charge.
- [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md) pour l’inscription d’un langage externe (par exemple, Java) auprès de SQL Server.
- [Kit SDK d’extensibilité Microsoft pour Java](how-to/extensibility-sdk-java-sql-server.md).
- Sur Windows et Linux, le code Java est accessible dans une bibliothèque externe à l’aide de l’instruction [CREATE EXTERNAL LIBRARY (Transact-SQL)](../t-sql/statements/create-external-library-transact-sql.md). En savoir plus : [Guide pratique pour appeler du code Java dans SQL Server](how-to/call-java-from-sql.md)
- [Extension de langage Java](language-extensions-overview.md) sur Windows et Linux. Vous pouvez rendre le code Java compilé accessible à SQL Server en affectant des autorisations et en définissant le chemin. Les applications clientes ayant accès à SQL Server peuvent utiliser des données et exécuter votre code en appelant [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), la même procédure que celle utilisée pour l’intégration de R et de Python sur SQL Server Machine Learning Services.

## <a name="next-steps"></a>Étapes suivantes

+ Installez les [Extensions de langage SQL Server sur Windows](install/windows-java.md) ou [sur Linux](../linux/sql-server-linux-setup-language-extensions-java.md).
