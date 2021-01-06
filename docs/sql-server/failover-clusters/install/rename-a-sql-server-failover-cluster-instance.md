---
title: Renommer une instance de cluster de basculement
description: Cet article explique comment renommer une instance SQL Server qui fait partie d’un cluster de basculement. Le processus n’est pas le même que pour le renommage des instances autonomes.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], virtual servers
- renaming virtual servers
- virtual servers [SQL Server], failover clustering
- failover clustering [SQL Server], virtual servers
ms.assetid: 2a49d417-25fb-4760-8ae5-5871bfb1e6f3
author: cawrites
ms.author: chadam
ms.openlocfilehash: a407ab5bc7f01a91a1b20a9627a61441e62ddcc9
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642825"
---
# <a name="rename-a-sql-server-failover-cluster-instance"></a>Renommer une instance de cluster de basculement SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Lorsqu'une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fait partie d'un cluster de basculement, le processus permettant de renommer un serveur virtuel diffère du processus permettant de renommer une instance autonome. Pour plus d’informations, consultez [Renommer un ordinateur qui héberge une instance autonome de SQL Server](../../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md).  
  
 Le nom du serveur virtuel est toujours identique au nom réseau SQL (le nom réseau du serveur virtuel SQL). Bien que vous puissiez modifier le nom du serveur virtuel, vous ne pouvez pas modifier le nom de l'instance. Par exemple, vous pouvez attribuer un autre nom à un serveur virtuel nommé VS1\instance1, tel que SQL35\instance1, mais la partie instance du nom (instance1) restera inchangée.  
  
 Avant de commencer le processus d'attribution d'un nouveau nom, examinez les éléments ci-dessous.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne prend pas en charge le renommage des serveurs impliqués dans la réplication, sauf en cas d’utilisation de la copie des journaux de transaction avec la réplication. Le serveur secondaire dans l'envoi de journaux peut être renommé si le serveur principal est perdu de manière permanente. Pour plus d’informations, consultez [Copie des journaux de transaction et réplication &#40;SQL Server&#41;](../../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md).  
  
-   Lorsqu'un serveur configuré pour utiliser la mise en miroir de base de données doit être renommé, vous devez au préalable désactiver la mise en miroir de la base de données, puis la réactiver avec le nouveau nom du serveur virtuel. Les métadonnées pour la mise en miroir de la base de données ne seront pas mises à jour automatiquement de façon à refléter le nouveau nom du serveur virtuel.  
  
### <a name="to-rename-a-virtual-server"></a>Pour renommer un serveur virtuel  
  
1.  À l'aide de l'Administrateur de cluster, modifiez le nom réseau SQL.  
  
2.  Faites passer la ressource de nom réseau en mode hors connexion. La ressource [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et les autres ressources dépendantes basculeront également en mode hors connexion.  
  
3.  Faites repasser la ressource [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en mode connecté.  
  
## <a name="verify-the-renaming-operation"></a>Vérification de l'opération d'attribution d'un nom  
 Une fois qu'un serveur virtuel a été renommé, toute connexion qui utilisait l'ancien nom doit maintenant se connecter à l'aide du nouveau nom.  
  
 Pour vérifier que l’opération de changement de nom a abouti, sélectionnez les informations de **@@servername** ou **sys.servers**. La fonction **@@servername** retourne le nouveau nom de serveur virtuel, et la table **sys.servers** affiche le nouveau nom de serveur virtuel. Pour vérifier que le processus de basculement fonctionne correctement avec le nouveau nom, l'utilisateur doit également essayer de faire basculer la ressource [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vers les autres nœuds.  
  
 Pour les connexions établies à partir d'un nœud du cluster, le nouveau nom peut être utilisé presque immédiatement. Toutefois, pour les connexions utilisant le nouveau nom à partir d'un ordinateur client, le nouveau nom ne peut être utilisé pour se connecter au serveur qu'une fois qu'il est visible à cet ordinateur client. La durée nécessaire à la propagation du nouveau nom sur le réseau peut aller de quelques secondes à quelques minutes, selon la configuration du réseau ; il faudra peut-être davantage de temps avant que l'ancien nom du serveur virtuel ne soit plus visible sur le réseau.  
  
 Pour minimiser le délai de propagation réseau lorsque vous renommez un serveur virtuel, procédez comme suit :  
  
#### <a name="to-minimize-network-propagation-delay"></a>Pour minimiser le délai de propagation réseau  
  
1.  Exécutez les commandes suivantes à partir d'une invite de commandes sur le nœud du serveur :  
  
    ```  
    ipconfig /flushdns  
    ipconfig /registerdns  
    nbtstat -RR  
    ```  
  
## <a name="additional-considerations-after-the-renaming-operation"></a>Éléments supplémentaires à prendre en considération après une opération Renommer  
 Après avoir renommé le nom réseau d'un cluster de basculement, nous devons vérifier et suivre les instructions suivantes pour que tous les scénarios dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent et [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]soient opérationnels.  
  
 **Service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent :** Effectuez des vérifications et les actions supplémentaires suivantes pour le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent :  
  
-   Corrigez les paramètres de registre si l'Agent SQL est configuré pour le transfert d'événements. Pour plus d’informations, consultez [Désigner un serveur de transfert d’événements &#40;SQL Server Management Studio&#41;](../../../ssms/agent/designate-an-events-forwarding-server-sql-server-management-studio.md).  
  
-   Corrigez les noms d'instance du serveur maître (MSX) et des serveurs cibles (TSX) lorsque le nom réseau du cluster/des ordinateurs est modifié. Pour plus d'informations, voir les rubriques suivantes :  
  
    -   [Annuler l’inscription de plusieurs serveurs cibles dans un serveur maître](../../../ssms/agent/defect-multiple-target-servers-from-a-master-server.md)  
  
    -   [Créer un environnement multi-serveur](../../../ssms/agent/create-a-multiserver-environment.md)  
  
-   Reconfigurez la copie des journaux de transaction afin que le nom de serveur mis à jour soit utilisé dans les journaux de sauvegarde et de restauration. Pour plus d'informations, voir les rubriques suivantes :  
  
    -   [Configurer la copie des journaux de transaction &#40;Transact-SQL&#41;](../../../database-engine/log-shipping/configure-log-shipping-sql-server.md)  
  
    -   [Supprimer la copie des journaux de transaction &#40;SQL Server&#41;](../../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   Mettez à jour les étapes de travail qui dépendent du nom du serveur. Pour plus d’informations, consultez [Gérer les étapes de travail](../../../ssms/agent/manage-job-steps.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Renommer un ordinateur qui héberge une instance autonome de SQL Server](../../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)  
  
