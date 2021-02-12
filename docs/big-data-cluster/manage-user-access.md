---
title: Gérer l’accès à un cluster Big Data en mode Active Directory
description: Gérer l’accès au cluster Big Data
author: NelGson
ms.author: negust
ms.reviewer: mikeray
ms.date: 08/04/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7c40a136f748126bd7baa6892d5cdb5317363ad2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046329"
---
# <a name="manage-big-data-cluster-access-in-active-directory-mode"></a>Gérer l’accès à un cluster Big Data en mode Active Directory

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article explique comment ajouter de nouveaux groupes Active Directory avec des rôles *bdcUser* en plus de ceux fournis lors du déploiement via le paramètre de configuration *clusterUsers*.

>[!IMPORTANT]
>N’utilisez pas cette procédure pour ajouter de nouveaux groupes Active Directory avec le rôle *bdcAdmin*. Les composants Hadoop, tels que HDFS et Spark, n’autorisent qu’un seul groupe Active Directory en tant que groupe superutilisateur : l’équivalent du rôle *bdcAdmin* dans un BDC. Pour octroyer des autorisations de groupes Active Directory avec *bdcAdmin* supplémentaires au cluster Big Data après le déploiement, ajoutez des utilisateurs et des groupes supplémentaires aux groupes déjà désignés au cours du déploiement. Vous pouvez suivre la même procédure pour mettre à jour l’appartenance au groupe qui a le rôle *bdcUsers*.

## <a name="two-overarching-roles-in-the-big-data-cluster"></a>Deux rôles principaux dans le cluster Big Data

Les groupes Active Directory peuvent être fournis dans la section sécurité du profil de déploiement comme faisant partie des rôles principaux de l’autorisation dans le cluster Big Data :

* `clusterAdmins`: Ce paramètre prend un groupe Active Directory. Les membres de ce groupe ont le rôle *bdcAdmin*, ce qui signifie qu’ils obtiennent des autorisations d’administrateur pour l’ensemble du cluster. Ils possèdent des autorisations *sysadmin* dans SQL Server, des autorisations *superutilisateur* dans Hadoop Distributed File System (HDFS) et Spark et des droits *administrateur* dans le contrôleur.

* `clusterUsers`: Ces groupes Active Directory sont mappés au rôle *bdcUsers* dans le BDC. Ce sont des utilisateurs réguliers sans autorisations administrateur dans le cluster. Ils disposent d’autorisations pour se connecter à l’instance maître SQL Server mais, par défaut, ils n’ont pas d’autorisation aux objets ni aux données. Il s’agit d’utilisateurs standard pour HDFS et Spark, sans autorisations de *superutilisateur*. Lors de la connexion au point de terminaison du contrôleur, ces utilisateurs peuvent uniquement interroger les points de terminaison (à l’aide de *azdata bdc endpoints list*).

Pour octroyer des autorisations *bdcUser* à des groupes Active Directory supplémentaires sans modifier les appartenances aux groupes dans Active Directory, effectuez les procédures décrites dans les sections suivantes.

## <a name="grant-bdcuser-permissions-to-additional-active-directory-groups"></a>Octoyer des autorisations *bdcUser* à des groupes Active Directory supplémentaires

### <a name="create-a-login-for-the-active-directory-user-or-group-in-the-sql-server-master-instance"></a>Créer une connexion pour l’utilisateur ou le groupe Active Directory dans l’instance maître SQL Server

1. Connectez-vous au point de terminaison SQL maître à l’aide de votre client SQL préféré. Utilisez n’importe quelle connexion d’administrateur (par exemple, `AZDATA_USERNAME`, fournie pendant le déploiement). Il peut également s’agir d’un compte Active Directory qui appartient au groupe Active Directory fourni comme `clusterAdmins` dans la configuration de la sécurité.

1. Pour créer une connexion pour l’utilisateur ou le groupe Active Directory, exécutez la commande TSQL suivante :

   ```sql
   CREATE LOGIN [<domain>\<principal>] FROM WINDOWS;
   ```

   Accordez les autorisations souhaitées dans l’instance SQL Server :

   ```sql
   ALTER SERVER ROLE <server role> ADD MEMBER [<domain>\<principal>];
   GO
   ```

Pour obtenir la liste complète des rôles de serveur, consultez la rubrique de sécurité SQL Server correspondante [ici](../relational-databases/security/authentication-access/server-level-roles.md).

### <a name="add-the-active-directory-user-or-group-to-the-roles-table-in-the-controller-database"></a>Ajouter l’utilisateur ou le groupe Active Directory à la table des rôles dans la base de données du contrôleur

1. Obtenez les informations d’identification SQL Server du contrôleur en exécutant les commandes suivantes :

   a. Exécutez cette commande en tant qu’administrateur Kubernetes :

   ```bash
   kubectl get secret controller-sa-secret -n <cluster name> -o yaml | grep password
   ```

   b. Décoder le secret au format Base64 :

   ```bash
   echo <password from kubectl command>  | base64 --decode && echo
   ```

1. Dans une fenêtre de commande distincte, exposez le port du serveur de base de données du contrôleur :

   ```bash
   kubectl port-forward controldb-0 1433:1433 --address 0.0.0.0 -n <cluster name>
   ```

1. Utilisez la connexion précédente pour insérer une nouvelle ligne dans les tables *roles* et *active_directory_principals*. Saisissez la valeur *REALM* en majuscules.

   ```sql
   USE controller;
   GO

   INSERT INTO [controller].[auth].[roles] VALUES (N'<user or group name>@<REALM>', 'bdcUser')
   GO

   INSERT INTO [controller].[auth].[active_directory_principals] VALUES (N'<user or group name>@<REALM>', N'<SID>')
   GO
   ```

   Pour rechercher le SID de l’utilisateur ou du groupe en cours d’ajout, vous pouvez utiliser les commandes PowerShell [Get-ADUser](/powershell/module/addsadministration/get-aduser/) ou [Get-ADGroup](/powershell/module/addsadministration/get-adgroup/).

2. Vérifiez que les membres du groupe que vous avez ajouté disposent des autorisations *bdcUser* en vous connectant au point de terminaison du contrôleur ou en vous authentifiant auprès de l’instance maître de SQL Server. Par exemple :

   ```bash
   azdata login
   azdata bdc endpoints list
   ```

## <a name="next-steps"></a>Étapes suivantes

- [Concepts de sécurité pour les clusters Big Data SQL Server 2019](concept-security.md)
