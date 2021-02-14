---
title: Synchronisation des packages R à partir du système de fichiers
description: Mettez à jour les bibliothèques R sur SQL Server avec des versions plus récentes installées sur le système de fichiers.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 06/13/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2017
ms.openlocfilehash: 72f10f291041f44c0598d40b21ca0bfe379a42d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073870"
---
# <a name="r-package-synchronization-for-sql-server"></a>Synchronisation de packages R pour SQL Server
[!INCLUDE [SQL Server 2017 only](../../includes/applies-to-version/sqlserver2017-only.md)]

La version de RevoScaleR incluse dans SQL Server 2017 comprend la possibilité de synchroniser des collections de packages R entre le système de fichiers et l’instance et la base de données où les packages sont utilisés.

Cette fonctionnalité a été fournie pour faciliter la sauvegarde des collections de packages R associées aux bases de données SQL Server. À l’aide de cette fonctionnalité, un administrateur peut restaurer non seulement la base de données, mais aussi les packages R qui ont été utilisés par les scientifiques de données qui travaillent dans cette base de données.

Cet article décrit la fonctionnalité de synchronisation de packages et l’utilisation de la fonction [rxSyncPackages](/machine-learning-server/r-reference/revoscaler/rxsyncpackages) pour effectuer les tâches suivantes :

+ Synchroniser une liste de packages pour une base de données SQL Server entière

+ Synchroniser les packages utilisés par un utilisateur individuel ou par un groupe d’utilisateurs

+ Si un utilisateur passe à un autre SQL Server, vous pouvez effectuer une sauvegarde de la base de données de travail de l’utilisateur et le restaurer sur le nouveau serveur et les packages de l’utilisateur seront installés dans le système de fichiers sur le nouveau serveur, comme requis par R.

Par exemple, vous pouvez utiliser la synchronisation dans les scénarios suivants :

+ L’administrateur de bases de données a restauré une instance de SQL Server sur un nouvel ordinateur et demande aux utilisateurs de se connecter à partir de leurs clients R et d’exécuter `rxSyncPackages` pour actualiser et restaurer leurs packages.

+ Vous pensez qu’un package R sur le système de fichiers est endommagé, exécutez ainsi `rxSyncPackages` sur SQL Server.

## <a name="requirements"></a>Spécifications

Avant de pouvoir utiliser la synchronisation des packages, vous devez disposer de la version appropriée de Microsoft R ou Machine Learning Server. Cette fonctionnalité est fournie dans Microsoft R version 9.1.0 ou ultérieure. 

Vous devez également activer la [fonctionnalité de gestion des packages](r-package-how-to-enable-or-disable.md) sur le serveur.

### <a name="determine-whether-your-server-supports-package-management"></a>Déterminer si votre serveur prend en charge la gestion des packages

Cette fonctionnalité est disponible dans SQL Server 2017 CTP 2 ou version ultérieure.

Vous pouvez ajouter cette fonctionnalité à une instance de SQL Server 2016 en mettant à niveau l’instance pour utiliser la dernière version de Microsoft R. Pour plus d’informations, consultez [Utiliser SqlBindR.exe pour mettre à niveau SQL Server R Services](../install/upgrade-r-and-python.md).

### <a name="enable-the-package-management-feature"></a>Activer la fonctionnalité de gestion des packages

Pour utiliser la synchronisation des packages, vous devez activer la nouvelle fonctionnalité de gestion des packages sur l’instance et sur des bases de données individuelles. Pour plus d’informations, consultez [Activer ou désactiver la gestion des packages pour SQL Server](r-package-how-to-enable-or-disable.md).

1. L’administrateur du serveur active la fonctionnalité pour l’instance.
2. Pour chaque base de données, l’administrateur octroie aux utilisateurs individuels la possibilité d’installer ou de partager des packages R à l’aide des rôles de base de données.

Une fois cette opération effectuée, vous pouvez utiliser des fonctions RevoScaleR, telles que [rxInstallPackages](/machine-learning-server/r-reference/revoscaler/rxinstallpackages) pour installer des packages dans une base de données.  Les informations sur les utilisateurs et les packages qu’ils peuvent utiliser sont stockées dans l’instance. 

Chaque fois que vous ajoutez un nouveau package à l’aide des fonctions de gestion des packages, les deux enregistrements dans SQL Server et le système de fichiers sont mis à jour. Ces informations peuvent être utilisées pour restaurer les informations sur les packages pour l’ensemble de la base de données.

### <a name="permissions"></a>Autorisations

+ La personne qui exécute la fonction de synchronisation des packages doit être un principal de sécurité sur l’instance et la base de données qui contient les packages.

+ L’appelant de la fonction doit être membre de l’un de ces rôles de gestion des package : **rpkgs-Shared** ou **rpkgs-Private**.

+ Pour synchroniser les packages marqués comme **partagés**, la personne qui exécute la fonction doit être membre du rôle **rpkgs-Shared** et les packages qui sont déplacés doivent avoir été installés dans une bibliothèque d’étendues partagée.

+ Pour synchroniser des packages marqués comme **privés**, le propriétaire du package ou l’administrateur doit exécuter la fonction et les packages doivent être privés.

+ Pour synchroniser des packages pour le compte d’autres utilisateurs, le propriétaire doit être membre du rôle de base de données **db_owner**.

## <a name="how-package-synchronization-works"></a>Fonctionnement de la synchronisation des packages

Pour utiliser la synchronisation des packages, appelez [rxSyncPackages](/r-server/r-reference/revoscaler/rxsyncpackages), qui est une nouvelle fonction dans [RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler). 

Pour chaque appel à `rxSyncPackages`, vous devez spécifier une instance et une base de données. Ensuite, répertoriez les packages à synchroniser ou spécifiez l’étendue du package.

1. Créez le contexte de calcul SQL Server à l’aide de la fonction `RxInSqlServer`. Si vous ne spécifiez pas de contexte de calcul, le contexte de calcul actuel est utilisé.

2. Indiquez le nom d’une base de données sur l’instance dans le contexte de calcul spécifié. Les packages sont synchronisés par base de données.

3. Spécifiez les packages à synchroniser à l’aide de l’argument d’étendue.

    Si vous utilisez l’étendue **privée**, seuls les packages appartenant au propriétaire spécifié sont synchronisés. Si vous spécifiez l’étendue **partagée**, tous les packages non privés dans la base de données sont synchronisés. 
    
    Si vous exécutez la fonction sans spécifier l’étendue **privée** ou **partagée**, tous les packages sont synchronisés.

4. Si la commande réussit, les packages existants dans le système de fichiers sont ajoutés à la base de données, avec l’étendue et le propriétaire spécifiés.

    Si le système de fichiers est endommagé, les packages sont restaurés en fonction de la liste gérée dans la base de données.

    Si la fonctionnalité de gestion des packages n’est pas disponible sur la base de données cible, une erreur est générée : « La fonctionnalité de gestion des packages n’est pas activée sur SQL Server ou la version est trop ancienne »

### <a name="example-1-synchronize-all-package-by-database"></a>Exemple 1. Synchroniser tous les packages par base de données

Cet exemple obtient tous les nouveaux packages du système de fichiers local et installe les packages dans la base de données [TestDB]. Comme aucun propriétaire n’est spécifique, la liste comprend tous les packages qui ont été installés pour les étendues privées et partagées.

```R
connectionString <- "Driver=SQL Server;Server=myServer;Database=TestDB;Trusted_Connection=True;"
computeContext <- RxInSqlServer(connectionString = connectionString )
rxSyncPackages(computeContext=computeContext, verbose=TRUE)
```

### <a name="example-2-restrict-synchronized-packages-by-scope"></a>Exemple 2. Limiter les packages synchronisés par étendue

Les exemples suivants synchronisent uniquement les packages dans l’étendue spécifiée.

```R
#Shared scope
rxSyncPackages(computeContext=computeContext, scope="shared", verbose=TRUE)

#Private scope
rxSyncPackages(computeContext=computeContext, scope="private", verbose=TRUE)
```

### <a name="example-3-restrict-synchronized-packages-by-owner"></a>Exemple 3. Limiter les packages synchronisés par propriétaire

L’exemple suivant montre comment synchroniser uniquement les packages qui ont été installés pour un utilisateur spécifique. Dans cet exemple, l’utilisateur est identifié par l’ID de connexion SQL, *user1*.

```R
rxSyncPackages(computeContext=computeContext, scope="private", owner = "user1", verbose=TRUE))
```

## <a name="related-resources"></a>Ressources associées

[Gestion des packages R pour SQL Server](install-additional-r-packages-on-sql-server.md)