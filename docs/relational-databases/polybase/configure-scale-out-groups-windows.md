---
title: Configurer des groupes de scale-out PolyBase sur Windows | Microsoft Docs
description: Configurez un groupe de scale-out PolyBase pour créer un cluster d’instances SQL Server. Vous améliorez ainsi les performances des requêtes pour les grands groupes de données provenant de sources externes.
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: tutorial
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 99129e6b52be36b1eb729bfda6056ec1010e7198
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351864"
---
# <a name="configure-polybase-scale-out-groups-on-windows"></a>Configurer des groupes de scale-out PolyBase sur Windows

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Cet article explique comment configurer un [groupe de scale-out PolyBase](polybase-scale-out-groups.md) sur Windows. Ceci permet de créer un cluster d’instances de SQL Server pour traiter de grands volumes de jeux de données à partir de sources de données externes telles que Hadoop ou le stockage d’objets Blob Azure, sous forme de montée en puissance (scale-out) parallèle pour des performances de requête optimisées.

## <a name="prerequisites"></a>Prérequis
  
- Plusieurs machines dans le même domaine  
  
- Un compte d’utilisateur de domaine pour exécuter les services PolyBase  
  
## <a name="process-overview"></a>Vue d’ensemble du processus

Les étapes suivantes résument le processus de création d’un groupe de scale-out PolyBase. La section suivante fournit une présentation plus détaillée de chaque étape.
  
1. Installez la même version de SQL Server avec PolyBase sur N machines.
  
2. Sélectionnez une instance de SQL Server en tant que nœud principal. 
  
3. Ajoutez les autres instances SQL Server comme nœuds de calcul à l’aide de [sp_polybase_join_group](../../relational-databases/system-stored-procedures/polybase-stored-procedures-sp-polybase-join-group.md).

4. Surveillez les nœuds du groupe à l’aide de [sys.dm_exec_compute_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).

5. facultatif. Supprimez un nœud de calcul à l’aide de [sp_polybase_leave_group &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/polybase-stored-procedures-sp-polybase-leave-group.md).

## <a name="example-walk-through"></a>Exemple de procédure

Cet exemple présente les étapes de configuration d’un groupe PolyBase à l’aide :  
  
1. de deux machines dans le domaine *PQTH4A* qui ont pour nom :  
  
   - PQTH4A-CMP01  
  
   - PQTH4A-CMP02  
  
2. Compte de domaine : *PQTH4A\PolyBaseUse* r  

## <a name="install-sql-server-with-polybase-on-all-machines"></a>Installer SQL Server avec PolyBase sur toutes les machines

1. Exécutez setup.exe.
  
2. Sur la page de sélection de fonctionnalités, sélectionnez **Service de requête PolyBase pour données externes**.
  
3. Dans la page Configuration du serveur, utilisez le **compte de domaine** PQTH4A\PolyBaseUser pour le moteur SQL Server PolyBase et le service de mouvement de données PolyBase SQL Server.
  
4. Dans la page Configuration de PolyBase, sélectionnez l’option **Utiliser ce serveur SQL Server comme composant du groupe de scale-out PolyBase**. Elle ouvre le pare-feu pour autoriser les connexions entrantes aux services PolyBase. Si le nœud principal est une instance nommée, vous devez ajouter manuellement le port SQL Server au pare-feu Windows sur le nœud principal, et également démarrer SQL Browser sur le nœud principal.
  
5. Une fois l’installation terminée, exécutez **services.msc**. Vérifiez que SQL Server, le moteur PolyBase et le service de déplacement de données PolyBase sont en cours d’exécution.
  
   ![Services PolyBase](../../relational-databases/polybase/media/polybase-services.png "Services PolyBase")  
  
## <a name="select-one-sql-server-as-head-node"></a>Sélectionnez un serveur SQL Server comme nœud principal  
  
Une fois l’installation terminée, les deux machines peuvent fonctionner en tant que nœuds principaux d’un groupe PolyBase. Dans cet exemple, nous choisissons « MSSQLSERVER » sur PQTH4A-CMP01 en tant que nœud principal.
  
## <a name="add-other-sql-server-instances-as-compute-nodes"></a>Ajoutez d’autres instances SQL Server comme nœuds de calcul  
  
1. Connectez-vous à SQL Server sur PQTH4A-CMP02.
  
2. Exécutez la procédure stockée [sp_polybase_join_group](../../relational-databases/system-stored-procedures/polybase-stored-procedures-sp-polybase-join-group.md).

   ```sql
   -- Enter head node details:
   -- head node machine name, head node dms control channel port, head node sql server name  
    EXEC sp_polybase_join_group 'PQTH4A-CMP01', 16450, 'MSSQLSERVER';
   ```  

3. Exécutez services.msc sur le nœud de calcul (PQTH4A-CMP02).
  
4. Arrêtez le moteur PolyBase et redémarrez le service de déplacement des données PolyBase.

> [!NOTE] 
> Quand le service du moteur Polybase est redémarré ou arrêté dans le nœud principal, le service DMS (Data Movement Service) s’arrête dès que le canal de communication est fermé entre DMS et le service du moteur Polybase (DW). Si le moteur DW est redémarré plus de 2 fois, le service DMS entre dans une période silencieuse de 90 minutes et doit attendre 90 minutes avant la prochaine tentative de démarrage automatique. Dans une telle situation, vous devez démarrer ce service manuellement sur tous les nœuds.

## <a name="optional-remove-a-compute-node"></a>Facultatif : supprimez un nœud de calcul  
  
1. Connectez-vous au nœud de calcul SQL Server (PQTH4A-CMP02).
  
2. Exécutez la procédure stockée sp_polybase_leave_group.
  
    ```sql  
    EXEC sp_polybase_leave_group;  
    ```  
  
3. Exécutez services.msc sur le nœud de calcul en cours de suppression (PQTH4A-CMP02).
  
4. Démarrez le moteur PolyBase. Redémarrez le service de déplacement de données PolyBase.
  
5. Vérifiez que le nœud a été supprimé en exécutant la DMV sys.dm_exec_compute_nodes sur PQTH4A-CMP01. À présent, PQTH4A-CMP02 fonctionne en tant que nœud principal autonome  
  
## <a name="next-steps"></a>Étapes suivantes  

Pour le dépannage, consultez [PolyBase troubleshooting with dynamic management views](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130)).
  
Pour plus d’informations sur PolyBase, consultez [Vue d'ensemble de PolyBase](../../relational-databases/polybase/polybase-guide.md).