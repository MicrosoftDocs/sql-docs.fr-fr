---
title: Activer et configurer FILESTREAM | Microsoft Docs
description: Pour utiliser FILESTREAM, vous devez d’abord l’activer dans l’instance du moteur de base de données SQL Server. Découvrez comment activer FILESTREAM à l’aide du Gestionnaire de configuration SQL Server.
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- FILESTREAM [SQL Server], enabling
ms.assetid: 78737e19-c65b-48d9-8fa9-aa6f1e1bce73
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: ea678a62f4453232fe8442cf2053c3b951e361ef
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250658"
---
# <a name="enable-and-configure-filestream"></a>Activer et configurer FILESTREAM

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Pour pouvoir commencer à utiliser FILESTREAM, vous devez l’activer sur l’instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Cette rubrique décrit comment activer FILESTREAM à l'aide du Gestionnaire de configuration SQL Server.  
  
##  <a name="enabling-filestream"></a><a name="enabling"></a> Activation de FILESTREAM  
  
#### <a name="to-enable-and-change-filestream-settings"></a>Pour activer et modifier des paramètres FILESTREAM  
  
1.  Dans le menu **Démarrer** , pointez sur **Tous les programmes**, sur [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]et sur **Outils de configuration**, puis cliquez sur **Gestionnaire de configuration SQL Server**.  
  
2.  Dans la liste de services, cliquez avec le bouton droit sur **Services SQL Server**, puis cliquez sur **Ouvrir**.  
  
3.  Dans le composant logiciel enfichable **Gestionnaire de configuration SQL Server** , recherchez l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur laquelle vous souhaitez activer FILESTREAM.  
  
4.  Cliquez avec le bouton droit sur l’instance, puis cliquez sur **Propriétés**.  
  
5.  Dans la boîte de dialogue **Propriétés de SQL Server** , cliquez sur l'onglet **FILESTREAM** .  
  
6.  Cochez la case **Activer FILESTREAM pour l’accès Transact-SQL** .  
  
7.  Si vous souhaitez lire et écrire des données FILESTREAM à partir de Windows, cliquez sur **Activer FILESTREAM pour l’accès en continu aux E/S de fichier**. Entrez le nom du partage Windows dans la zone **Nom de partage Windows** .  
  
8.  Si des clients distants doivent accéder aux données FILESTREAM stockées sur ce partage, sélectionnez **Autoriser les clients distants à avoir un accès en continu aux données FILESTREAM**.  
  
9. Cliquez sur **Appliquer**.  
  
10. Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], cliquez sur **Nouvelle requête** pour afficher l'Éditeur de requête.  
  
11. Dans l'Éditeur de requête, entrez le code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant :  
  
    ```sql  
    EXEC sp_configure filestream_access_level, 2  
    RECONFIGURE  
    ```  
  
12. Cliquez sur **Exécuter**.  
  
13. Redémarrez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

##  <a name="best-practices"></a><a name="best"></a> Bonnes pratiques  
  
###  <a name="physical-configuration-and-maintenance"></a><a name="config"></a> Configuration et maintenance physiques  
 Lorsque vous configurez des volumes de stockage FILESTREAM, prenez en compte les recommandations suivantes :  
  
-   Désactivez les noms de fichiers courts sur les systèmes informatiques FILESTREAM. La création de noms de fichiers courts demande beaucoup plus de temps. Pour désactiver les noms de fichiers courts, servez-vous de l’utilitaire Windows **fsutil** .  
  
-   Défragmentez régulièrement les systèmes informatiques FILESTREAM.  
  
-   Utilisez des clusters NTFS de 64 Ko. Les volumes compressés doivent être définis à l'aide de clusters NTFS de 4 Ko.  
  
-   Désactivez l’indexation sur les volumes FILESTREAM et définissez **disablelastaccess**. Pour définir **disablelastaccess**, utilisez l’utilitaire **fsutil** de Windows.  
  
-   Désactivez l’analyse antivirus des volumes FILESTREAM quand elle n’est pas nécessaire. Si l'analyse antivirus est nécessaire, ne définissez pas de stratégies qui suppriment automatiquement les fichiers incriminés.  
  
-   Configurez et paramétrez le niveau RAID pour la tolérance de panne et en fonction des performances requises par une application.  
  
|Niveau RAID|Performances en écriture|Performances en lecture|Tolérance de panne|Notes|  
|-|-|-|-|-|   
|RAID 5|Normal|Normal|Excellent|Les performances sont meilleures qu'avec un seul disque ou une simple concaténation de disques, mais elles sont moins bonnes qu'avec le niveau RAID 0 ou le niveau RAID 5 utilisant l'agrégation par bandes.|  
|RAID 0|Excellent|Excellent|None||  
|RAID 5 + agrégation par bandes|Excellent|Excellent|Excellent|Option la plus chère.|  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |
  
  
###  <a name="physical-database-design"></a><a name="database"></a> Conception d’une base de données physique  
 Lorsque vous concevez une base de données FILESTREAM, prenez en compte les recommandations suivantes:  
  
-   Les colonnes FILESTREAM doivent être accompagnées d’une colonne **uniqueidentifier** ROWGUID correspondante. Ces types de tables doivent également être accompagnés d'un index unique. En règle générale, cet index n'est pas un index cluster. Si la logique métier des bases de données requiert un index cluster, vous devez vous assurer que les valeurs stockées dans l'index ne sont pas aléatoires. Les valeurs aléatoires entraînent une réorganisation de l'index chaque fois qu'une ligne est ajoutée ou supprimée dans la table.  
  
-   Pour des raisons de performances, les groupes de fichiers et conteneurs FILESTREAM doivent résider sur d'autres volumes que ceux du système d'exploitation, de la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , du journal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , de la base de données tempdb ou du fichier de pagination.  
  
-   La gestion de l'espace et les stratégies ne sont pas prises en charge directement par FILESTREAM. Toutefois, vous pouvez gérer l'espace et appliquer des stratégies de manière indirecte en affectant chaque groupe de fichiers FILESTREAM à un volume distinct et en utilisant les fonctionnalités de gestion du volume.  
  
  
