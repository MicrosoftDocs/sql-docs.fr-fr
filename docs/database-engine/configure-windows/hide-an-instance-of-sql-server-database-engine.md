---
title: Masquer une instance du moteur de base de données SQL Server | Microsoft Docs
description: Découvrez comment masquer une instance du moteur de base de données SQL Server. Les ordinateurs clients ne peuvent pas utiliser le service SQL Server Browser pour localiser les instances masquées.
ms.custom: ''
ms.date: 08/19/2015
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], hiding instances
- hiding instances of Database Engine
ms.assetid: 392de21a-57fa-4a69-8237-ced8ca86ed1d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f3f275d1f2c5ce4142bb357a1c1b314b694ef589
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783022"
---
# <a name="hide-an-instance-of-sql-server-database-engine"></a>Masquer une instance du moteur de base de données SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment masquer une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide du Gestionnaire de configuration SQL Server. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser pour énumérer les instances du [!INCLUDE[ssDE](../../includes/ssde-md.md)] installées sur l'ordinateur. Cela permet aux applications clientes de rechercher un serveur et aide les clients à effectuer la distinction entre plusieurs instances du [!INCLUDE[ssDE](../../includes/ssde-md.md)] sur le même ordinateur. Vous pouvez utiliser la procédure suivante pour empêcher le service SQL Server Browser d'exposer une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] aux ordinateurs clients dont les utilisateurs tentent de localiser l'instance à l'aide du bouton **Parcourir** .  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Utilisation du Gestionnaire de configuration SQL Server  
  
#### <a name="to-hide-an-instance-of-the-sql-server-database-engine"></a>Pour masquer une instance du Moteur de base de données SQL Server  
  
1.  Dans le **Gestionnaire de configuration SQL Server**, développez **Configuration du réseau SQL Server**, cliquez avec le bouton droit sur **Protocoles pour** *\<server instance>* , puis sélectionnez **Propriétés**.  
  
2.  Sous l'onglet **Indicateurs** , dans la zone **HideInstance** , sélectionnez **Oui**, puis cliquez sur **OK** pour fermer la boîte de dialogue. La modification prend effet immédiatement pour les nouvelles connexions.  
  
## <a name="remarks"></a>Notes  
 Si vous masquez une instance nommée, vous devez fournir le numéro de port dans la chaîne de connexion pour la connexion à l'instance masquée, même si le service de navigateur est en cours d'exécution. Nous vous recommandons d'utiliser un port statique au lieu d'un port dynamique pour l'instance masquée nommée.  
  Pour plus d’informations, consultez [Configurer un serveur pour écouter un port TCP spécifique &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md).  
  
### <a name="clustering"></a>Clustering  
 Si vous masquez le nom d’une instance en cluster ou d’un groupe de disponibilité, il se peut que le service de cluster ne soit pas en mesure de se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cela entraîne l’échec de la vérification **IsAlive** de l’instance de cluster et la mise hors connexion de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
 
Pour éviter cela, créez un alias dans tous les nœuds de l’instance en cluster ou dans toutes les instances qui hébergent des réplicas de groupe de disponibilité afin de refléter le port statique que vous avez configuré pour l’instance.  Par exemple, sur un groupe de disponibilité avec deux réplicas, sur le premier nœud (node-one), créez un alias pour l’instance du deuxième nœud (node-two), par exemple `node-two\instancename`. Sur le deuxième nœud (node-two), créez un alias appelé `node-one\instancename`. Les alias sont nécessaires pour la réussite du basculement. 
 
 Pour plus d’informations, consultez [Créer ou supprimer un alias de serveur devant être utilisé par un client &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/create-or-delete-a-server-alias-for-use-by-a-client.md).  
  
 Si vous masquez une instance nommée en cluster, il se peut que le service de cluster ne soit pas en mesure de se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si la clé de Registre **LastConnect** (**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SNI11.0\LastConnect**) a un autre port que le port écouté par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si le service de cluster ne peut pas établir une connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez recevoir un message d’erreur similaire à ce qui suit :  
**ID de l’événement : 1001 : Nom de l’événement : Blocage des ressources du clustering de basculement.**  
  
## <a name="see-also"></a>Voir aussi  
 [Configuration réseau du serveur](../../database-engine/configure-windows/server-network-configuration.md)   
 [Description des connexions clientes du serveur virtuel SQL](https://support.microsoft.com/kb/273673)   
 [Affectation d’un port statique à une instance nommée SQL Server pour éviter un piège courant](https://deep.data.blog/2012/09/08/how-to-assign-a-static-port-to-a-sql-server-named-instance-and-avoid-a-common-pitfall/)  
  
  
