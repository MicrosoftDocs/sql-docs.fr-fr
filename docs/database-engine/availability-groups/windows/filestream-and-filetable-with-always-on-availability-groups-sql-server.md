---
title: Utiliser FILESTREAM et FileTable avec les groupes de disponibilité
description: Étapes à suivre pour utiliser FILESTREAM ou FileTable avec des bases de données membres d’un groupe de disponibilité Always On.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- FileTables [SQL Server], Availability Groups
- FILESTREAM [SQL Server], Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: fdceda9a-a9db-4d1d-8745-345992164a98
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 566c50867511e9e9d02996247c3efecf634b93aa
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644120"
---
# <a name="use-filestream-and-filetable-with-always-on-availability-groups"></a>Utiliser FILESTREAM et FileTable avec les groupes de disponibilité Always On

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  Cette rubrique contient des informations sur l'utilisation des fonctionnalités FILESTREAM et FileTable avec [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
 L'intégralité des fonctionnalités FILESTREAM est prise en charge. Après un basculement, les données FILESTREAM sont accessibles à la fois sur les réplicas secondaires avec accès en lecture et sur le nouveau réplica principal.  
  
 La fonctionnalité FileTable n'est prise en charge que partiellement. Après un basculement, les données FileTable sont accessibles sur le réplica principal, mais pas sur les réplicas secondaires avec accès en lecture.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Avant d'ajouter une base de données qui utilise FILESTREAM, avec ou sans FileTable, à un groupe de disponibilité, vérifiez que FILESTREAM est activé sur chaque instance de serveur qui héberge un réplica de disponibilité pour le groupe de disponibilité. Pour plus d’informations, consultez [Enable and Configure FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md).  
  
##  <a name="using-virtual-network-names-vnns-for-filestream-and-filetable-access"></a><a name="vnn"></a> Utilisation de noms de réseau virtuel (VNN) pour l'accès à FILESTREAM et FileTable  
 Lorsque vous activez FILESTREAM sur une instance du [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], un partage d'instance est créé pour permettre d'accéder aux données FILESTREAM. Vous accédez à ce partage en utilisant le nom d'ordinateur au format suivant :  
  
 `\\<computer_name>\<filestream_share_name>`  
  
 Toutefois, dans un groupe de disponibilité Always On, le nom de l’ordinateur est virtualisé à l’aide d’un nom de réseau virtuel, ou VNN. Lorsque l'ordinateur est le réplica primaire dans un groupe de disponibilité, et les bases de données du groupe de disponibilité contiennent des données FILESTREAM, un partage d'étendue VNN est également créé pour permettre d'accéder aux données FILESTREAM. Cela n'affecte pas l'accès Transact-SQL aux données FILESTREAM. Toutefois, les applications qui utilisent les API du système de fichiers doivent utiliser le partage d'étendue VNN, dont le chemin d'accès a le format suivant :  
  
 `\\<VNN>\<filestream_share_name>`  
  
 Ce partage d'étendue VNN est créé lorsque l'un des événements suivants se produit.  
  
-   Vous ajoutez une base de données qui contient des données FILESTREAM à un groupe de disponibilité Always On sur le réplica primaire. Dans ce cas, le partage `\\<computer_name>\<filestream_share_name>` existe déjà. Le partage `\\<VNN>\<filestream_share_name>` est créé.  
  
-   Vous activez FILESTREAM pour l'accès en continu des E/S de fichier sur un réplica primaire qui possède des groupes de disponibilité. Les partages suivants sont créés :  
  
    1.  `\\<computer_name>\<filestream_share_name>`  
  
    2.  `\\<VNN1>\<filestream_share_name>` pour le groupe de disponibilité 1.  
  
    3.  `\\<VNN2>\<filestream_share_name>` pour le groupe de disponibilité 2.  
  
 Ces partages d'étendue VNN sont également propagées à tous les réplicas secondaires.  
  
 Lorsque la base de données qui contient des données FILESTREAM ou FileTable appartient à un groupe de disponibilité Always On :  
  
-   Les fonctions FILESTREAM et FileTable acceptent ou retournent des noms de réseau virtuel (VNN) à la place de noms d'ordinateur. Pour plus d’informations sur ces fonctions, consultez [Fonctions FileStream et FileTable &#40;Transact-SQL&#41;](../../../relational-databases/system-functions/filestream-and-filetable-functions-transact-sql.md).  
  
-   Tous les accès à FILESTREAM ou aux données FileTable via les API du système de fichiers doivent utiliser des VNN à la place des noms d'ordinateur.  
  
 Si votre application tente d'accéder au partage en utilisant le nom d'ordinateur au format `\\<computer_name>\<filestream_share_name>` lorsque la base de données fait partie d'un groupe de disponibilité, une erreur est générée.  
  
 Si votre application tente d'accéder au partage à l'aide d'un chemin d'accès d'étendue VNN lorsque la base de données ne fait pas partie d'un groupe de disponibilité, la demande peut réussir. Dans ce cas, le nom du réseau virtuel est résolu avec le nom de l'ordinateur. Toutefois, cette utilisation est fortement déconseillée, étant donné que le chemin d'accès d'étendue VNN cesse de fonctionner si le groupe de disponibilité est supprimé.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Activer et configurer FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md)  
  
-   [Activer les conditions préalables pour les FileTables](../../../relational-databases/blob/enable-the-prerequisites-for-filetable.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
 Aucun.  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
