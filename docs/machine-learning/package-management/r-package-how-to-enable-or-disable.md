---
title: Activer ou désactiver la gestion des packages R distants
description: Activer la gestion des packages distants sur SQL Server 2016 R Services ou Machine Learning Services SQL Server (dans la base de données)
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 06/13/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: 728dc656d3fdb94abae0a5c2c0e2665af2c4807c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073903"
---
# <a name="enable-or-disable-remote-package-management-for-sql-server"></a>Activer ou désactiver la gestion des packages distants pour SQL Server
[!INCLUDE [SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

Cet article explique comment activer la gestion à distance des packages R à partir d’une station de travail cliente ou d’un autre Machine Learning Server. Une fois la fonctionnalité de gestion des packages activée sur SQL Server, vous pouvez utiliser les commandes RevoScaleR sur un client pour installer des packages sur SQL Server.

Par défaut, la fonctionnalité de gestion des packages externes pour SQL Server est désactivée. Vous devez exécuter un script distinct pour activer la fonctionnalité, comme décrit dans la section suivante.

## <a name="overview-of-process-and-tools"></a>Vue d’ensemble du processus et outils

Pour activer ou désactiver la gestion des packages sur SQL Server, utilisez l’utilitaire de ligne de commande **RegisterRExt.exe**, qui est inclus dans le package **RevoScaleR**.

[L’activation](#bkmk_enable) de cette fonctionnalité est un processus en deux étapes qui requiert un administrateur de base de données : vous activez la gestion des packages sur l’instance (une fois par instance), puis activez la gestion des packages sur la base de données SQL (une fois par base de données SQL Server).

[La désactivation](#bkmk_disable) de la fonctionnalité de gestion des packages requiert également plusieurs étapes : supprimez les autorisations et les packages au niveau de la base de données (une fois par base de données), puis supprimez les rôles du serveur (une fois par instance).

## <a name="enable-package-management"></a><a name="bkmk_enable"></a>Activer la gestion des packages

1. Sur SQL Server, ouvrez une invite de commandes élevée, puis accédez au dossier contenant l’utilitaire, RegisterRExt.exe. L’emplacement par défaut est : `<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExe.exe`.

2. Exécutez la commande suivante, en fournissant les arguments appropriés pour votre environnement :

    `RegisterRExt.exe /install pkgmgmt [/instance:name] [/user:username] [/password:*|password]`

    Cette commande crée sur l’ordinateur SQL Server des objets au niveau de l’instance qui sont nécessaires pour la gestion des packages. Elle permet également de redémarrer Launchpad pour l’instance.

    Si vous ne spécifiez pas d’instance, l’instance par défaut est utilisée. Si vous ne spécifiez pas d’utilisateur, le contexte de sécurité actuel est utilisé. Par exemple, la commande suivante active la gestion des packages sur l’instance par défaut, à l’aide des informations d’identification de l’utilisateur qui a ouvert l’invite de commandes :

    `REgisterRExt.exe /install pkgmgmt`

3. À partir d’une invite de commandes élevée, exécutez la commande suivante pour ajouter la gestion des packages à une base de données spécifique :

    `RegisterRExt.exe /install pkgmgmt /database:databasename [/instance:name] [/user:username] [/password:*|password]`
   
    Cette commande crée certains artefacts de base de données, notamment les rôles de base de données suivants nécessaires pour le contrôle des autorisations d’utilisateur : `rpkgs-users`, `rpkgs-private` et `rpkgs-shared`.

    Par exemple, la commande suivante active la gestion des packages sur la base de données, sur l’instance par défaut. Si vous ne spécifiez pas d’utilisateur, le contexte de sécurité actuel est utilisé.

    `RegisterRExt.exe /install pkgmgmt /database:TestDB`

4. Répétez la commande pour chaque base de données sur laquelle les packages doivent être installés.

5. Pour vérifier que les nouveaux rôles ont été créés avec succès, dans SQL Server Management Studio, cliquez sur la base de données, développez **Sécurité**, puis développez **Rôles de base de données**.

    Vous pouvez également exécuter une requête sur sys.database_principals comme suit :

    ```sql
    SELECT pr.principal_id, pr.name, pr.type_desc,   
        pr.authentication_type_desc, pe.state_desc,   
        pe.permission_name, s.name + '.' + o.name AS ObjectName  
    FROM sys.database_principals AS pr  
    JOIN sys.database_permissions AS pe  
        ON pe.grantee_principal_id = pr.principal_id  
    JOIN sys.objects AS o  
        ON pe.major_id = o.object_id  
    JOIN sys.schemas AS s  
        ON o.schema_id = s.schema_id;
    ```

Une fois cette fonctionnalité activée, vous pouvez utiliser la fonction RevoScaleR pour installer ou désinstaller des packages à partir d’un client R distant.

## <a name="disable-package-management"></a><a name="bkmk_disable"></a> Désactiver la gestion des packages

1. À partir d’une invite de commandes élevée, exécutez à nouveau l’utilitaire RegisterRExt pour désactiver la gestion des packages au niveau de la base de données :

    `RegisterRExt.exe /uninstall pkgmgmt /database:databasename [/instance:name] [/user:username] [/password:*|password]`

    Cette commande supprime de la base de données spécifiée des objets de la base de données relatifs à la gestion des packages. Elle supprime également de l’emplacement du système de fichiers sécurisé sur l’ordinateur SQL Server tous les packages qui ont été installés.

2. Répétez cette commande sur chaque base de données où la gestion des packages a été utilisée.

3.  (Facultatif) Une fois toutes les bases de données effacées des packages lors de l’étape précédente, exécutez la commande suivante à partir d’une invite de commandes élevée :

    `RegisterRExt.exe /uninstall pkgmgmt [/instance:name] [/user:username] [/password:*|password]`

    Cette commande supprime la fonctionnalité de gestion des packages de l’instance. Vous devrez peut-être redémarrer manuellement le service Launchpad une fois de plus pour voir les modifications.

## <a name="next-steps"></a>Étapes suivantes

+ [Utiliser RevoScaleR pour installer des packages R](install-r-packages-with-revoscaler.md)
+ [Obtenir des informations sur les packages R](r-package-information.md)
+ [Conseils pour l’utilisation de packages R](tips-for-using-r-packages.md)
