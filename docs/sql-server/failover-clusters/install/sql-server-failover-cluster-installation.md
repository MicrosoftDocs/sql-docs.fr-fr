---
title: Installer une instance de cluster de basculement
description: Découvrez comment installer un cluster de basculement SQL Server. Créez et configurez une instance de cluster de basculement en exécutant SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
ms.assetid: c0e75a7c-85c5-423c-a218-77247bf071aa
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4e46540d75518b4da2a11337e35dfe22a335af60
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346638"
---
# <a name="sql-server-failover-cluster-installation"></a>Installation d'un cluster de basculement SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Pour installer un cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , vous devez créer et configurer une instance de cluster de basculement en exécutant le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
## <a name="installing-a-failover-cluster"></a>Installation d'un cluster de basculement  
 Pour installer un cluster de basculement, vous devez utiliser un compte de domaine avec des droits d'administrateur local pour vous connecter en tant que service et pour agir dans le cadre du système d'exploitation sur tous les nœuds du cluster de basculement. Pour installer un cluster de basculement à l'aide du programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , procédez comme suit :  
  
1.  Pour installer, configurer et gérer un cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , utilisez le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
    -   Identifiez les informations dont vous aurez besoin pour créer votre instance de cluster de basculement (par exemple, une ressource disque de cluster, des adresses IP et un nom réseau) et les nœuds disponibles pour le basculement. Pour plus d'informations :  
  
        -   [Avant l'installation du clustering de basculement](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)  
  
        -   [Considérations sur la sécurité pour une installation SQL Server](../../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
  
    -   Les étapes de configuration doivent se dérouler avant l'exécution du programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ; pour cela, utilisez l'Administrateur de cluster Windows. Vous avez besoin d'un groupe WSFC pour chaque instance de cluster de basculement que vous souhaitez configurer.  
  
    -   Vous devez vous assurer que votre système satisfait la configuration minimale requise. Pour plus d’informations sur la configuration requise pour un cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , consultez [Avant l’installation du clustering de basculement](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md).  
  
2.  Ajoutez ou retirez des nœuds d'une configuration de cluster de basculement sans aucune incidence sur les autres nœuds du cluster. Pour plus d’informations, consultez [Ajouter ou supprimer des nœuds dans un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
    -   Tous les nœuds d'un cluster de basculement doivent être de la même plateforme (32 bits ou 64 bits) et exécuter le même système d'exploitation et la même version. En outre, les éditions 64 bits de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doivent être installées sur un matériel 64 bits utilisant les versions 64 bits des systèmes d'exploitation Windows. Il n'existe pas de prise en charge WOW64 pour le clustering de basculement dans cette version.  
  
3.  Spécifiez plusieurs adresses IP pour chaque instance de cluster de basculement. Vous pouvez spécifier plusieurs adresses IP pour chaque sous-réseau. Si les adresses IP sont toutes sur le même sous-réseau, le programme d’installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] définit la dépendance sur AND. Si vous mettez des nœuds en cluster sur plusieurs sous-réseaux, le programme d'installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] définit la dépendance sur OR.  

4.  L’instance de cluster de basculement (FCI) SQL Server exige que les nœuds de cluster soient joints à un domaine. Les configurations suivantes ne sont **pas prises en charge** :
    - instance de cluster de basculement SQL sur des clusters de groupe de travail ; 
    - instance de cluster de basculement SQL sur un cluster multidomaine ;   
    - instance de cluster de basculement SQL sur des clusters de domaine + de groupe de travail. 

## <a name="ssnoversion-failover-cluster-installation-options"></a>[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Options d’installation d’un cluster de basculement  
  
##### <a name="option-1-integrated-installation-with-add-node"></a>Option 1 : Installation intégrée avec ajout de nœud  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] comprend deux étapes :  
  
1.  Créez et configurez une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à un seul nœud. Une fois la configuration du nœud réussie, vous disposez d'une instance de cluster de basculement entièrement fonctionnelle. Celle-ci n'offre pas une haute disponibilité pour l'instant, car il n'y a qu'un seul nœud dans le cluster de basculement.  
  
2.  Sur chaque nœud à ajouter au cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , exécutez le programme d'installation en utilisant la fonctionnalité d'ajout de nœud pour ajouter le nœud correspondant.  
  
##### <a name="option-2-advancedenterprise-installation"></a>Option n°2 : Installation avancée/entreprise  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] L’installation avancée/entreprise d’un cluster de basculement comprend deux étapes :  
  
1.  Sur chaque nœud devant faire partie du cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , exécutez le programme d'installation en utilisant la fonctionnalité de préparation de cluster de basculement. Cette étape prépare les nœuds pour le clustering ; toutefois, aucune instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] n'est opérationnelle à la fin de cette étape.  
  
2.  Une fois les nœuds préparés pour le clustering, lancez le programme d'installation sur le nœud propriétaire du disque partagé en utilisant la fonctionnalité de création de cluster de basculement. Cette étape permet de configurer et de rendre opérationnelle l'instance de cluster de basculement. À la fin de cette étape, vous disposez d'une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] opérationnelle.  
  
    > [!NOTE]  
    >  Les deux options d'installation permettent d'effectuer une installation de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à plusieurs nœuds. Quelle que soit l'option d'installation choisie, la fonctionnalité d'ajout de nœud peut être utilisée pour ajouter des nœuds supplémentaires après qu'un cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a été créé.  
  
    > [!IMPORTANT]  
    >  La lettre de lecteur du système d'exploitation pour les emplacements d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit correspondre sur tous les nœuds ajoutés au cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
#### <a name="ip-address-configuration-during-setup"></a>Configuration d'adresse IP pendant l'installation  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Le programme d'installation vous permet de définir ou de modifier les paramètres de dépendance de ressource IP pendant les actions suivantes :  
  
-   Installation intégrée – [Créer un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)  
  
-   CompleteFailoverCluster (installation avancée) – [Créer un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)  
  
-   Ajouter un nœud – [Ajouter ou supprimer des nœuds dans un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)  
  
-   Supprimer un nœud – [Ajouter ou supprimer des nœuds dans un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)  
  
 **Remarque** Les adresses IP IPV6 sont prises en charge.  Si vous configurez à la fois IPV4 et IPV6, les adresses sont traitées comme des sous-réseaux différents, et IPV6 devrait être mis en ligne en premier.  
  
##### <a name="ssnoversion-multi-subnet-failover-cluster"></a>[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Cluster de basculement de sous-réseaux multiples  
 Vous pouvez définir des dépendances OR lorsque les nœuds du cluster se trouvent sur des sous-réseaux différents. Toutefois, chaque nœud du cluster de basculement de sous-réseaux multiples [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit être un propriétaire possible d'au moins une d'adresse IP spécifiée.  
  
## <a name="see-also"></a>Voir aussi  
 [Avant l'installation du clustering de basculement](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)   
 [Créer un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)   
 [Installer SQL Server 2016 à partir de l’invite de commandes](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)   
 [Mettre à niveau une instance de cluster de basculement SQL Server](../../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md)  
  
