---
title: Autorisations de hiérarchisation HDFS pour les clusters Big Data SQL Server
titleSuffix: Manage HDFS tiering permissions for SQL Server Big Data Clusters
description: Gérez la sécurité de la hiérarchisation HDFS sur les clusters Big Data SQL Server comme les autorisations sur d’autres systèmes Linux.
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 0f791cd64dd177e01dc086dd08f9216bda4a55b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046681"
---
# <a name="manage-hdfs-permissions-for-big-data-clusters-2019"></a>Gérer les autorisations HDFS pour [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

HDFS, en tant que système de fichiers, est similaire aux systèmes de fichiers basés sur Linux qui utilisent POSIX pour les autorisations de fichier. En plus du modèle d’autorisations POSIX traditionnel, HDFS prend aussi en charge les listes de contrôle d’accès (ACL) POSIX. Pour plus d’informations, consultez l’[article Apache Hadoop sur les listes de contrôle d’accès](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_.28Access_Control_Lists.29).

Les sections suivantes fournissent des exemples d’utilisation de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] pour la gestion des autorisations de fichier et de répertoire HDFS.

## <a name="prerequisites"></a>Conditions préalables requises

- [Cluster Big Data déployé](deployment-guidance.md)
- [Outils Big Data](deploy-big-data-tools.md)
  
## <a name="hdfs-shell"></a>Interpréteur de commandes HDFS

La fonctionnalité d’interpréteur de commandes `hdfs` dans [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] vous permet d’émettre des commandes directement dans un interpréteur de commandes pour gérer les autorisations HDFS sur les fichiers et les répertoires. Le mécanisme sous-jacent utilise les appels WebHdfs pour émettre les commandes.

La commande suivante ouvre l’interpréteur de commandes.

```bash
azdata bdc hdfs shell
```

Pour accéder à l’aide de l’interpréteur de commandes `hdfs` et comprendre comment émettre des commandes, exécutez la commande suivante une fois que l’interpréteur de commandes est actif.

```bash
[hdfs] ?
```

L’exemple suivant montre comment créer un répertoire, lister les répertoires, modifier les autorisations sur un répertoire et accorder à un utilisateur nommé `bob` l’accès en lecture, en écriture et en exécution au répertoire `sales`.

```bash
[hdfs] mkdir sales
[hdfs] ls
rwxr-xr-x  hdfs bdcadmins        0 Oct 09 18:02 system/
rwxrwxr-x admin bdcadmins        0 Oct 10 16:47 sales/
--xrwxrwxrwx  hdfs bdcadmins        0 Oct 09 18:03 tmp/
rwxrwxrwx  hdfs bdcadmins        0 Oct 09 17:59 user/

[hdfs] acl modify  '/sales/' 'user:bob:rwx'
acl modify: Change completed.
[hdfs] acl status  '/sales/'
{
  `AclStatus`: {
    `entries`: [
      `user:bob:rwx`,
      `group::r-x`
    ],
    `group`: `bdcadmins`,
    `owner`: `admin`,
    `permission`: `775`,
    `stickyBit`: false
  }
}
```

## <a name="create-a-directory-in-hdfs-using-azure-data-cli-azdata"></a>Créer un répertoire dans HDFS en utilisant [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]

Créez un répertoire appelé `data` dans `/sales`.

```bash
azdata bdc hdfs mkdir --path '/sales/data'
```

## <a name="change-owner-of-a-directory-or-file"></a>Changer le propriétaire d’un répertoire ou d’un fichier

Changez l’utilisateur propriétaire du répertoire `data` dans HDFS et définissez *`alice`* comme utilisateur propriétaire et *`salesgroup`* comme groupe d’appartenance. Pour pouvoir changer le propriétaire, vous devez être propriétaire vous-même.

```bash
azdata bdc hdfs chown --owner alice --group 'salesgroup' --path '/sales/data'
```

## <a name="change-permissions-of-a-file-or-directory-with-chmod"></a>Modifier les autorisations d’un fichier ou d’un répertoire avec `chmod`

Utilisez `chmod` pour modifier les autorisations sur les fichiers et les répertoires (pour un propriétaire, un groupe d’appartenance ou autres). Pour plus d’informations, reportez-vous à la [modification des autorisations sur un système de fichiers Linux](https://www.lifewire.com/uses-of-command-chmod-2201064). Dans HDFS, le modèle est le même. Par exemple :

```bash
azdata bdc hdfs chmod --permission 750 --path /sales/data
```

```bash
azdata bdc hdfs chmod --permission 775 --path /sales/data/file.txt
```

## <a name="set-sticky-bit-on-directories"></a>Définir un sticky bit sur des répertoires

Définissez le sticky bit sur des répertoires pour empêcher la suppression ou le déplacement non intentionnels des fichiers. Le sticky bit limite l’autorisation de suppression ou de déplacement d’un fichier au superutilisateur, au propriétaire du répertoire ou au propriétaire du fichier. Ce paramètre n’affecte pas le fichier. L’exemple ci-dessous définit un sticky bit sur le répertoire `users` en préfixant les autorisations avec un `1`.

```bash
azdata bdc hdfs chmod --path /sales/users --permission 1750
```

## <a name="setting-acls-on-files-and-directories"></a>Définition de listes de contrôle d’accès sur des fichiers et des répertoires

Pour définir des listes de contrôle d’accès sur des fichiers et des répertoires dans HDFS, utilisez les commandes [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].

Définition de listes de contrôle d’accès sur un répertoire et attribution à un utilisateur nommé *`tom`* un accès en lecture, en écriture et en exécution au répertoire *`data`* . 

> [!NOTE]
> Quand vous utilisez la commande `set`, assurez-vous de fournir la spécification de liste ACL complète, y compris la spécification de liste ACL pour l’utilisateur propriétaire, le groupe d’appartenance et les autres.

```bash
azdata bdc hdfs acl set --path '/sales' --aclspec  'user::rw-,user:tom:rwx,group::rw-,other::rw-'
```

## <a name="default-acl-on-directories"></a>Liste de contrôle d’accès par défaut sur des répertoires

La liste de contrôle d’accès par défaut permet à des sous-répertoires d’hériter des autorisations du répertoire parent. Seuls des répertoires peuvent avoir une liste de contrôle d’accès par défaut. Lorsqu’un nouveau fichier ou sous-répertoire est créé, il hérite automatiquement de la liste de contrôle d’accès par défaut de son parent dans sa propre liste de contrôle d’accès. De cette façon, la liste de contrôle d’accès par défaut est héritée via des niveaux de répertoire d’une profondeur arbitraire au fur et à mesure que de nouveaux sous-répertoires sont créés.

Voici un exemple illustrant comment définir la liste de contrôle d’accès par défaut en utilisant azdata.

```bash
azdata bdc hdfs acl set --path '/sale' --aclspec  'user::rw-,user:tom:rwx,group::rw-,other::rw-,default:group::rw-,default:user::rw-,default:other::rw-'
```

## <a name="next-steps"></a>Étapes suivantes

- [Référence [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/reference/reference-azdata.md)

- [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md)