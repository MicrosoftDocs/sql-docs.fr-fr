---
title: Afficher les paramètres NodeWeight pour le quorum de cluster | Microsoft Docs
description: Découvrez comment afficher les paramètres NodeWeight pour chaque nœud membre dans un cluster de clustering de basculement Windows Server. Ces paramètres sont utilisés pendant le vote du quorum.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- quorum [SQL Server], AlwaysOn and WSFC quorum
ms.assetid: b845e73a-bb01-4de2-aac2-8ac12abebc95
author: cawrites
ms.author: chadam
ms.openlocfilehash: f4a6b8f335cbc52a6c7a91bc02f68fe0561bf87b
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642686"
---
# <a name="view-cluster-quorum-nodeweight-settings"></a>Afficher les paramètres NodeWeight pour le quorum de cluster
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment afficher les paramètres NodeWeight pour chaque nœud membre dans un cluster de clustering de basculement Windows Server (WSFC). Les paramètres NodeWeight sont utilisés pendant le vote du quorum pour prendre en charge les scénarios de récupération d'urgence et de sous-réseaux multiples pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] et les instances de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   **Avant de commencer :**  [Prérequis](#Prerequisites), [Sécurité](#Security)  
  
-   **Pour afficher les paramètres NodeWeight du quorum avec :** [Utilisation de Transact-SQL](#TsqlProcedure), [Utilisation de PowerShell](#PowerShellProcedure), [Utilisation de Cluster.exe](#CommandPromptProcedure)  
  
##  <a name="before-you-start"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
 Cette fonctionnalité est prise en charge uniquement dans [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] ou versions ultérieures.  
  
> [!IMPORTANT]  
>  Pour utiliser les paramètres NodeWeight, le correctif logiciel suivant doit être appliqué à tous les serveurs dans le cluster WSFC :  
>   
>  [KB2494036](https://support.microsoft.com/kb/2494036) : un correctif est disponible pour vous permettre de configurer un nœud de cluster qui n'a pas de votes de quorum dans [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] et dans [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)]  
  
> [!TIP]  
>  Si ce correctif logiciel n'est pas installé, les exemples de cette rubrique retournent des valeurs vides ou NULL pour NodeWeight.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
 L'utilisateur doit être un compte de domaine qui est membre du groupe Administrateurs local sur chaque nœud du cluster WSFC.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
##### <a name="to-view-nodeweight-settings"></a>Pour consulter les paramètres NodeWeight  
  
1.  Connectez-vous à une instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans le cluster.  
  
2.  Interrogez la vue [sys].[dm_hadr_cluster_members].  
  
### <a name="example-transact-sql"></a>Exemple (Transact-SQL)  
 L’exemple suivant interroge une vue système pour retourner les valeurs de tous les nœuds du cluster de cette instance.  
  
```sql  
SELECT  member_name, member_state_desc, number_of_quorum_votes  
 FROM   sys.dm_hadr_cluster_members;  
```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell  
  
##### <a name="to-view-nodeweight-settings"></a>Pour consulter les paramètres NodeWeight  
  
1.  Démarrez Windows PowerShell avec élévation de privilèges via **Exécuter en tant qu'administrateur**.  
  
2.  Importez le module `FailoverClusters` pour activer les applets de commande de cluster.  
  
3.  Utilisez l'objet `Get-ClusterNode` pour retourner une collection d'objets du nœud de cluster.  
  
4.  Générez la sortie des propriétés du nœud de cluster dans un format lisible.  
  
### <a name="example-powershell"></a>Exemple (PowerShell)  
 L’exemple suivant génère certaines des propriétés de nœud du cluster « Cluster001 ».  
  
```powershell  
Import-Module FailoverClusters  
  
$cluster = "Cluster001"  
$nodes = Get-ClusterNode -Cluster $cluster  
  
$nodes | Format-Table -property NodeName, State, NodeWeight  
```  
  
##  <a name="using-clusterexe"></a><a name="CommandPromptProcedure"></a> Utilisation de Cluster.exe  
  
> [!NOTE]  
>  L'utilitaire cluster.exe est déconseillé dans [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] .  Utilisez PowerShell avec le clustering de basculement pour le développement futur.  L'utilitaire cluster.exe sera supprimé dans la prochaine version de Windows Server. Pour plus d'informations, consultez [Mappage des commandes Cluster.exe aux applets de commande Windows PowerShell pour les clusters de basculement](https://technet.microsoft.com/library/ee619744\(WS.10\).aspx).  
  
##### <a name="to-view-nodeweight-settings"></a>Pour consulter les paramètres NodeWeight  
  
1.  Démarrez une invite de commandes avec élévation de privilèges via **Exécuter en tant qu'administrateur**.  
  
2.  Utilisez **cluster.exe** pour retourner l'état du nœud et les valeurs de NodeWeight  
  
### <a name="example-clusterexe"></a>Exemple (Cluster.exe)  
 L’exemple suivant génère certaines des propriétés de nœud du cluster « Cluster001 ».  
  
```ms-dos  
cluster.exe Cluster001 node /status /properties  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Modes de quorum WSFC et configuration de vote &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md)   
 [Configurer les paramètres NodeWeight pour un quorum de cluster](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)   
 [sys.dm_hadr_cluster_members &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)   
 [Applets de commande de cluster de basculement dans Windows PowerShell répertoriées par tâche](https://technet.microsoft.com/library/ee619761\(WS.10\).aspx)  
  
  
