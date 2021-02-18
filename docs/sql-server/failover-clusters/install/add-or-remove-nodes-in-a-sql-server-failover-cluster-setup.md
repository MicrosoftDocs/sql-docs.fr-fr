---
title: Ajouter, supprimer des nœuds dans une instance cluster de basculement
description: Cet article explique comment ajouter ou supprimer des nœuds dans une instance de cluster de basculement SQL Server Always On existante.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- adding nodes
- nodes [Faillover Clustering], removing
- nodes [Faillover Clustering], adding
- failover clustering [SQL Server], nodes
- deleting nodes
- cluster maintenance [SQL Server]
- removing nodes
ms.assetid: fe20dca9-a4c1-4d32-813d-42f1782dfdd3
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2d1bbc562b90804a7478ce7d5b866cd17dfbc1df
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346153"
---
# <a name="add-or-remove-nodes-in-a-failover-cluster-instance-setup"></a>Ajouter ou supprimer des nœuds dans une instance de cluster de basculement (installation)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

 Utilisez cette procédure pour gérer les nœuds d'une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] existante.  
  
 Pour mettre à jour ou supprimer une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vous devez être un administrateur local autorisé à se connecter en tant que service sur tous les nœuds du cluster de basculement Windows Server (WSFC) sous-jacent. Pour des installations locales, vous devez exécuter le programme d'installation en tant qu'administrateur. Si vous installez [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à partir d'un partage distant, vous devez utiliser un compte de domaine qui a les autorisations de lecture et d'exécution sur le partage distant.  
  
 Pour ajouter un nœud à une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] existante, vous devez exécuter la Configuration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur le nœud destiné à être ajouté à l'instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. N'exécutez pas le programme d'installation sur le nœud actif.  
  
 Pour supprimer un nœud d'une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] existante, vous devez exécuter la Configuration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur le nœud à supprimer dans l'instance de cluster de basculement de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Pour afficher les étapes de procédure pour ajouter ou supprimer des nœuds, sélectionnez une des opérations suivantes :  
  
-   [Ajouter un nœud à une instance de cluster de basculement Always On existante](#Add)  
  
-   [Supprimer un nœud à une instance de cluster de basculement Always On existante](#Remove)  
  
> [!IMPORTANT]  
>  La lettre de lecteur du système d'exploitation pour les emplacements d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit correspondre sur tous les nœuds ajoutés à l’instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
##  <a name="add-node"></a><a name="Add"></a> Ajouter un nœud  
  
#### <a name="to-add-a-node-to-an-existing-ssnoversion-failover-cluster-instance"></a>Pour ajouter un nœud à une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] existante  
  
1.  Insérez le support d'installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et, dans le dossier racine, double-cliquez sur Setup.exe. Pour effectuer l'installation à partir d'un partage réseau, recherchez le dossier racine sur le partage, puis double-cliquez sur Setup.exe.  
  
2.  L'Assistant Installation lancera le Centre d'installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour ajouter un nœud à une instance de cluster de basculement existante, cliquez sur **Installation** dans le volet gauche. Sélectionnez ensuite **Ajouter un nœud à un cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]** .  
  
3.  L'Outil d'analyse de configuration système effectue une opération de découverte sur votre ordinateur. Pour continuer, [!INCLUDE[clickOK](../../../includes/clickok-md.md)].  
  
4.  Dans la page Sélection de la langue, vous pouvez spécifier la langue de votre instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] si vous effectuez l'installation sur un système d'exploitation localisé et que le support d'installation inclut des modules linguistiques pour l'anglais et pour la langue qui correspond au système d'exploitation. Pour plus d’informations sur la prise en charge des langues multiples et sur les considérations relatives à l’installation, consultez [Versions linguistiques locales dans SQL Server](../../../sql-server/install/local-language-versions-in-sql-server.md).  
  
     Pour continuer, cliquez sur **Suivant**.  
  
5.  Dans la page Clé de produit (Product Key), spécifiez la clé PID pour une version de production du produit. Notez que la clé de produit que vous entrez pour cette installation doit se rapporter à la même édition de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que celle installée sur le nœud actif.  
  
6.  Dans la page Termes du contrat de licence, prenez connaissance du contrat de licence et activez la case à cocher indiquant que vous en acceptez les termes et conditions. Pour aider à améliorer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vous pouvez également activer l'option d'utilisation des fonctionnalités et envoyer des rapports à [!INCLUDE[msCoName](../../../includes/msconame-md.md)]. Pour continuer, cliquez sur **Suivant**. Pour mettre fin au programme d'installation, cliquez sur **Annuler**.  
  
7.  L'outil d'analyse de configuration système vérifiera l'état système de votre ordinateur avant que le programme d'installation ne se poursuive. Lorsque la vérification est terminée, cliquez sur **Suivant** pour poursuivre.  
  
8.  Dans la page Configuration du nœud de clusters, utilisez la zone de liste déroulante pour spécifier le nom de l'instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui sera modifiée pendant cette opération d'installation.  
  
9. Dans la page Configuration du serveur - Comptes de service, spécifiez les comptes de connexion des services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les services réels configurés dans cette page dépendent des fonctionnalités que vous avez sélectionnées. Pour les installations d’instance de cluster de basculement, les informations de nom de compte et de type de démarrage seront préremplies dans cette page, en fonction des paramètres indiqués pour le nœud actif. Vous devez fournir des mots de passe pour chaque compte. Pour plus d’informations, consultez [Configuration du serveur - Comptes de service](../../../database-engine/install-windows/install-sql-server.md) et [Configurer les comptes de service Windows et les autorisations](../../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
     **Note de sécurité** [!INCLUDE[ssNoteStrongPass](../../../includes/ssnotestrongpass-md.md)]  
  
     Lorsque vous avez terminé de spécifier les informations de connexion pour les services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , cliquez sur **Suivant**.  
  
10. Dans la page Création de rapports, spécifiez les informations que vous aimeriez envoyer à [!INCLUDE[msCoName](../../../includes/msconame-md.md)] pour améliorer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. L'option de création de rapports d'erreurs est activée par défaut.  
  
11. L'outil d'analyse de configuration système exécutera un autre ensemble de règles pour valider la configuration de votre ordinateur avec les fonctionnalités [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que vous avez spécifiées.  
  
12. La page Prêt à ajouter le nœud affiche une arborescence des options d'installation spécifiées durant l'exécution du programme d'installation.  
  
13. La page Progression de l'ajout de nœud fournit des informations sur l'état pour que vous puissiez contrôler la progression de l'installation au fil de l'avancement de l'exécution du programme d'installation.  
  
14. Après l'installation, la page Terminé fournit un lien vers le fichier journal résumé pour l'installation et d'autres remarques importantes. Pour terminer le processus d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , cliquez sur **Fermer**.  
  
15. Redémarrez l'ordinateur si vous êtes invité à le faire. Il est important de lire le message affiché par l'Assistant Installation à la fin de l'installation. Pour plus d’informations sur les fichiers journaux d’installation, consultez [Afficher et lire les fichiers journaux d’installation de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
##  <a name="remove-node"></a><a name="Remove"></a> Supprimer un nœud  
  
#### <a name="to-remove-a-node-from-an-existing-ssnoversion-failover-cluster-instance"></a>Pour supprimer un nœud d'une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] existante  
  
1.  Insérez le support d'installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Dans le dossier racine, double-cliquez sur setup.exe. Pour effectuer l'installation à partir d'un partage réseau, recherchez le dossier racine sur le partage, puis double-cliquez sur Setup.exe.  
  
2.  L'Assistant Installation exécute le Centre d'installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour supprimer un nœud dans une instance de cluster de basculement existante, cliquez sur **Maintenance** dans le volet gauche, puis sélectionnez **Supprimer un nœud d’un cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]**.  
  
3.  L'Outil d'analyse de configuration système effectue une opération de découverte sur votre ordinateur. Pour continuer, [!INCLUDE[clickOK](../../../includes/clickok-md.md)].  
  
4.  Après avoir cliqué sur Installer dans la page Fichiers de support du programme d'installation, l'Outil d'analyse de configuration système vérifie l'état du système de votre ordinateur avant de poursuivre l'installation. Lorsque la vérification est terminée, cliquez sur **Suivant** pour poursuivre.  
  
5.  Dans la page Configuration du nœud de cluster, utilisez la zone de liste déroulante pour spécifier le nom de l'instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à modifier pendant cette opération d'installation. Le nœud à supprimer est répertorié dans le champ **Nom de ce nœud** .  
  
6.  La page Prêt à supprimer le nœud affiche une arborescence des options spécifiées durant l'exécution du programme d'installation. Pour continuer, cliquez sur **Supprimer**.  
  
7.  Pendant l'opération de suppression, la page Progression de la suppression de nœud fournit des informations sur l'état.  
  
8.  La page Terminé fournit un lien vers le fichier journal résumé pour l'opération de suppression de nœud et d'autres remarques importantes. Pour terminer la suppression du nœud [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , cliquez sur **Fermer**. Pour plus d’informations sur les fichiers journaux d’installation, consultez [Afficher et lire les fichiers journaux d’installation de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et lire les fichiers journaux d'installation de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
