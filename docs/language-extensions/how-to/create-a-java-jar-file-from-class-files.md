---
title: Créer un fichier jar Java à partir de fichiers class
description: Empaquetez vos fichiers de classes en un fichier jar quand vous utilisez les extensions de langage SQL Server pour exécuter du code Java.
author: dphansen
ms.author: davidph
ms.date: 11/05/2019
ms.topic: how-to
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 076b20d277ef8608fd8c9fc30b4bce303d4ef06b
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870164"
---
# <a name="create-a-java-jar-file-from-class-files"></a>Créer un fichier jar Java à partir de fichiers class
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Découvrez comment packager vos fichiers de classes en un fichier jar quand vous utilisez les [extensions de langage SQL Server](../language-extensions-overview.md) pour exécuter du code Java. Nous vous recommandons de packager vos fichiers.

## <a name="create-a-jar-file"></a>Créer un fichier jar

Pour créer un fichier jar à partir de fichiers class, accédez au dossier contenant votre fichier class, puis exécutez la commande suivante :

```cmd
jar -cf <MyJar.jar> *.class
```

Vérifiez que le chemin de `jar.exe` fait partie de la variable de chemin système. Vous pouvez également spécifier le chemin complet du fichier jar, qui se trouve sous `/bin`, dans le dossier du kit JDK. Par exemple :

```cmd
C:\Users\MyUser\Desktop\jdk1.8.0_201\bin\jar -cf <MyJar.jar> *.class
```

## <a name="next-steps"></a>Étapes suivantes

+ [Guide pratique pour appeler le runtime Java dans les extensions de langage SQL Server](../how-to/call-java-from-sql.md)