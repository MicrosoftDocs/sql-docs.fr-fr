---
title: Utiliser T-SQL (CREATE EXTERNAL LIBRARY) pour installer des packages R
description: Ajouter des packages R à QL Server 2016 R Services ou Machine Learning Services SQL Server (dans la base de données).
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2017
ms.openlocfilehash: 058cd3a573abfa427b1e71ac08ac66c4b0e17932
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074350"
---
# <a name="use-t-sql-create-external-library-to-install-r-packages-on-sql-server"></a>Utiliser T-SQL (CREATE EXTERNAL LIBRARY) pour installer des packages R sur SQL Server
[!INCLUDE [SQL Server 2017 only](../../includes/applies-to-version/sqlserver2017-only.md)]

Cet article explique comment installer de nouveaux packages R sur une instance de SQL Server où Machine Learning est activé. Vous avez le choix entre plusieurs approches. L’utilisation de T-SQL fonctionne mieux pour les administrateurs de serveur qui ne sont pas familiarisés avec R.

L’instruction [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) permet d’ajouter un package ou un ensemble de packages à une instance ou à une base de données spécifique sans exécuter directement le code R ou Python. Toutefois, cette méthode requiert la préparation du package et des autorisations de base de données supplémentaires.

+ Tous les packages doivent être disponibles en tant que fichiers zippés locaux, plutôt que téléchargés à la demande à partir d’Internet.

+ Toutes les dépendances doivent être identifiées par nom et par version et incluses dans le fichier zip. L’instruction échoue si les packages requis ne sont pas disponibles, y compris les dépendances de package en aval. 

+ Vous devez être **db_owner** ou disposer de l’autorisation CREATE EXTERNAL LIBRARY dans un rôle de base de données. Pour plus d’informations, consultez [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md).

## <a name="download-packages-in-archive-format"></a>Télécharger les packages au format d’archive

Si vous installez un package unique, téléchargez le package au format zippé.

Il est plus courant d’installer plusieurs packages en raison des dépendances du package. Lorsqu’un package requiert d’autres packages, vous devez vérifier qu’ils sont tous accessibles l’un à l’autre lors de l’installation. Nous vous recommandons [la création d’un référentiel local](create-a-local-package-repository-using-minicran.md) à l’aide de [miniCRAN](https://andrie.github.io/miniCRAN/) pour assembler une collection complète de packages, ainsi que [igraph](https://igraph.org/r/) pour l’analyse des dépendances des packages. L’installation d’une version incorrecte d’un package ou l’omission d’une dépendance de package peut entraîner l’échec d’une instruction CREATE EXTERNAL LIBRARY. 

## <a name="copy-the-file-to-a-local-folder"></a>Copier le fichier dans un dossier local

Copiez le fichier zippé contenant tous les packages dans un dossier local sur le serveur. Si vous n’avez pas accès au système de fichiers sur le serveur, vous pouvez également transmettre un package complet en tant que variable, en utilisant un format binaire. Pour plus d’informations, consultez [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md).

## <a name="run-the-statement-to-upload-packages"></a>Exécuter l’instruction pour télécharger des packages

Ouvrez une fenêtre **Requête**, à l’aide d’un compte avec des privilèges d’administrateur.

Exécutez l’instruction T-SQL `CREATE EXTERNAL LIBRARY` pour charger la collection de packages zippée dans la base de données.

Par exemple, l’instruction suivante nomme la source du package en tant que référentiel miniCRAN contenant le package **randomForest**, ainsi que ses dépendances. 

```sql
CREATE EXTERNAL LIBRARY [randomForest]
FROM (CONTENT = 'C:\Temp\Rpackages\randomForest_4.6-12.zip')
WITH (LANGUAGE = 'R');
```

Vous ne pouvez pas utiliser un nom arbitraire ; le nom de la bibliothèque externe doit avoir le même nom que celui que vous prévoyez d’utiliser lors du chargement ou de l’appel du package.

## <a name="verify-package-installation"></a>Vérifier l'installation du package

Si la bibliothèque est correctement créée, vous pouvez exécuter le package dans SQL Server, en l’appelant à l’intérieur d’une procédure stockée.
    
```sql
EXEC sp_execute_external_script
@language =N'R',
@script=N'library(randomForest)'
```

## <a name="see-also"></a>Voir aussi

+ [Obtenir des informations sur les packages R](r-package-information.md)
+ [Tutoriels sur R](../tutorials/r-tutorials.md)