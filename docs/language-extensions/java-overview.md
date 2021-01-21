---
title: Qu’est-ce que l’Extension de langage Java ?
titleSuffix: SQL Server Language Extensions
description: L’extension de langage Java est une fonctionnalité de SQL Server servant à exécuter du code Java externe. Les données relationnelles peuvent être utilisées dans le code Java externe avec le framework d’extensibilité.
author: dphansen
ms.author: davidph
ms.date: 11/10/2020
ms.topic: overview
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 4df5f2e96ff8795ac592c6aa61f7c79e03145c31
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596303"
---
# <a name="what-is-java-language-extension"></a>Qu’est-ce que l’Extension de langage Java ?
[!INCLUDE [SQL Server 2019 and later](../includes/applies-to-version/sqlserver2019.md)]

L’extension de langage Java est une fonctionnalité de SQL Server servant à exécuter du code Java externe. Les données relationnelles peuvent être utilisées dans le code Java externe avec le [framework d’extensibilité](concepts/extensibility-framework.md). L’extension de langage Java fait partie des [Extensions de langage SQL Server](language-extensions-overview.md).

Le runtime Java par défaut est Zulu Open JRE. Vous pouvez également utiliser un autre JRE ou SDK Java.

## <a name="what-you-can-do-with-the-java-language-extension"></a>Rôle de l’extension de langage Java

L’extension de langage Java utilise le framework d’extensibilité pour exécuter du code Java externe. L’exécution du code est isolée des processus du moteur de base, mais entièrement intégrée à l’exécution des requêtes SQL Server. Vous pouvez exécuter du code Java à la source des données, ce qui évite d’avoir à extraire ces dernières sur le réseau.

Le langage Java externe est défini avec [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md). La procédure stockée système [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) sert d’interface pour l’exécution du code Java.

## <a name="get-started-with-java-language-extension"></a>Prise en main de l’extension de langage Java

1. Installez l’extension de langage Java SQL Server [sur Windows](install/windows-java.md) ou [sur Linux](../linux/sql-server-linux-setup-language-extensions-java.md).

1. Configurez les outils de développement.

    + Utilisez l’IDE de votre choix pour développer du code Java.
    + Installez le [Kit SDK d’extensibilité Microsoft pour Java](how-to/extensibility-sdk-java-sql-server.md) pour exécuter du code Java sur SQL Server.
    + Utilisez [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md) pour exécuter du code externe sur SQL Server.
    + Utilisez la procédure stockée système [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) pour exécuter votre code Java sur SQL Server.

1. Écrivez votre premier code Java.

    + [Tutoriel : Expressions régulières avec Java](tutorials/search-for-string-using-regular-expressions-in-java.md)

## <a name="limitations"></a>Limites

Le nombre de valeurs dans les mémoires tampons d’entrée et de sortie ne peut pas dépasser `MAX_INT (2^31-1)`, car il s’agit du nombre maximal d’éléments pouvant être alloués dans un tableau dans Java.

## <a name="next-steps"></a>Étapes suivantes

+ Installer l’extension de langage Java SQL Server [sur Windows](install/windows-java.md) ou [sur Linux](../linux/sql-server-linux-setup-language-extensions-java.md).
+ Installer le [kit SDK d’extensibilité Microsoft pour Java](how-to/extensibility-sdk-java-sql-server.md).