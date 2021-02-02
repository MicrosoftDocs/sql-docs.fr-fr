---
description: MSSQLSERVER_5009
title: MSSQLSERVER_5009
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5009 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 70269ab1c31eb5b66c6853688243c872359aaddf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154277"
---
# <a name="mssqlserver_5009"></a>MSSQLSERVER_5009
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|5009|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|ALT_BADDISKS|
|Texte du message|Un ou plusieurs fichiers répertoriés dans l’instruction sont introuvables ou n’ont pas pu être initialisés.|
||

## <a name="explanation"></a>Explication

Cette erreur indique que vous avez spécifié dans la commande ALTER DATABASE ou DBCC SHRINK* un nom de fichier ou un ID de fichier qui n’a pas pu être résolu.

Considérez le scénario suivant :

- Vous disposez d’une base de données Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui utilise un mode de récupération complète ou un mode de récupération utilisant les journaux de transactions.
- Vous ajoutez un nouveau fichier de données nommé *db_file1* à la base de données.
- Vous choisissez « Données » comme type de fichier pour `db_file1`.
- Vous vous rendez compte que vous avez spécifié le mauvais type de fichier.
- Vous supprimez le fichier `db_file1`, puis vous sauvegardez le journal des transactions de cette base de données.
- Vous ajoutez un nouveau fichier journal nommé *db_file1* à la même base de données.
- Vous essayez de supprimer le fichier journal nommé *db_file1* à l’aide de l’instruction ALTER DATABASE ou à l’aide de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio.

Dans ce scénario, vous recevez un message d’erreur semblable au suivant :

> Msg 5009, Niveau 16, État 9, Ligne 1 - Un ou plusieurs fichiers répertoriés dans l’instruction sont introuvables ou n’ont pas pu être initialisés.

## <a name="possible-causes"></a>Causes possibles

Ce problème se produit lorsque le nom logique du fichier que vous essayez de supprimer n’est pas unique parmi les tables du catalogue système. Par exemple, ce problème se produira si ce fichier se trouvait dans la base de données, mais qu’il en a été supprimé.

Lorsque vous essaierez de supprimer un fichier portant le même nom logique, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tentera de supprimer le fichier logique qui a été supprimé. Cela déclenche l’affichage d’un message d’erreur.

## <a name="user-action"></a>Action requise

Pour contourner ce problème, effectuez les étapes suivantes.

> [!NOTE]
> Ces étapes entraînent la réutilisation des valeurs d’ID de fichier.

1. Utilisez l’instruction ALTER DATABASE pour créer un nouveau fichier logique avec un nom différent, mais avec le même type de données. Par exemple, nommez le fichier logique `different_remove_file_name` au lieu de `db_file1`, comme dans l’exemple suivant :

    ```sql
    ALTER DATABASE [DBNAME] ADD FILE ( NAME = N'different_remove_file_name',
    FILENAME = N'D:\MSSQL.1\MSSQL\DATA\db_file1.ndf', SIZE = 1MB, MAXSIZE = 1MB)
    ```

    > [!NOTE]
    > Vous pouvez utiliser n’importe quel nom ou chemin de fichier.

1. Utilisez l’instruction ALTER DATABASE pour supprimer le fichier logique que vous avez créé à l’étape 1, comme dans l’exemple suivant :

    ```sql
    ALTER DATABASE [DBNAME] REMOVE FILE [different_remove_file_name]
    ```

1. Créez une sauvegarde du journal des transactions de la base de données.
1. Réessayez de supprimer le fichier logique nommé *db_file1*.
