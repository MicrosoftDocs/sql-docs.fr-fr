---
title: Placer un groupe de disponibilité hors connexion (SQL Server) | Microsoft Docs
description: Découvrez comment faire passer un groupe de disponibilité Always On de l’état ONLINE à l’état OFFLINE à l’aide de Transact-SQL dans SQL Server.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], take offline
ms.assetid: 50f5aad8-0dff-45ef-8350-f9596d3db898
author: cawrites
ms.author: chadam
ms.openlocfilehash: 361fdb639201fe69c9f31e233dca51a910ed10a7
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641541"
---
# <a name="take-an-availability-group-offline-sql-server"></a>Placer un groupe de disponibilité hors connexion (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment faire passer un groupe de disponibilité Always On de l’état ONLINE à l’état OFFLINE à l’aide de [!INCLUDE[tsql](../../../includes/tsql-md.md)] dans [!INCLUDE[ssSQL11SP1](../../../includes/sssql11sp1-md.md)] et versions ultérieures. Il n'y a aucune perte de données des bases de données de validation synchrone, car si aucun réplica avec validation synchrone n'est synchronisé, l'opération OFFLINE génère une erreur et conserve le groupe de disponibilité dans l'état ONLINE. Conserver le groupe de disponibilité en ligne protège les bases de données non synchronisées avec validation synchrone contre la perte de données. Après qu'un groupe de disponibilité a été mis hors connexion, ses bases de données deviennent indisponibles pour les clients et vous ne pouvez pas remettre le groupe de disponibilité en ligne. Par conséquent, mettez un groupe de disponibilité hors connexion uniquement pour migrer les ressources du groupe de disponibilité d'un cluster WSFC à un autre.  
  
 Pendant une migration entre clusters de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], si les applications se connectent directement au réplica principal d'un groupe de disponibilité, le groupe de disponibilité doit être mis hors connexion. La migration entre clusters de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] prend en charge la mise à niveau du système d'exploitation avec un temps mort minimal des groupes de disponibilité. Le scénario classique consiste à utiliser la migration entre clusters de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] pour la mise à niveau du système d'exploitation vers [!INCLUDE[win8](../../../includes/win8-md.md)] ou [!INCLUDE[win8srv](../../../includes/win8srv-md.md)]. Pour plus d’informations, consultez [Migration entre clusters de groupes de disponibilité Always On pour la mise à niveau du système d’exploitation](/previous-versions/sql/sql-server-2012/jj873730(v=msdn.10)).  
  
  
> [!CAUTION]  
>  Utilisez l’option OFFLINE pour la migration entre clusters des ressources d’un groupe de disponibilité ou pour le basculement d’un groupe de disponibilité accessible en lecture.
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   L'instance de serveur sur laquelle vous sélectionnez la commande OFFLINE doit exécuter [!INCLUDE[ssSQL11SP1](../../../includes/sssql11sp1-md.md)] ou version ultérieure (édition Entreprise ou ultérieure).    
-   Le groupe de disponibilité doit être en ligne.  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
 Avant de mettre le groupe de disponibilité hors connexion, supprimez le ou les écouteurs du groupe de disponibilité. Pour plus d’informations, consultez [Supprimer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour placer le groupe de disponibilité hors connexion**  
  
1.  Connectez-vous à une instance de serveur qui héberge un réplica de disponibilité pour le groupe de disponibilité. Ce réplica peut être le réplica principal ou un réplica secondaire.  
  
2.  Utilisez l'instruction [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , comme suit :  
  
     ALTER AVAILABILITY GROUP *nom_groupe* OFFLINE  
  
     où *nom_groupe* correspond au nom du groupe de disponibilité.  
  
### <a name="example"></a>Exemple  
 L'exemple suivant place le groupe de disponibilité `AccountsAG` hors connexion.  
  
```  
ALTER AVAILABILITY GROUP AccountsAG OFFLINE;  
```  
  
##  <a name="follow-up-after-the-availability-group-goes-offline"></a><a name="FollowUp"></a> Suivi : Après que le groupe de disponibilité a été mis hors connexion  
  
-   **Enregistrement de l’opération OFFLINE :**  l’identité du nœud WSFC sur lequel l’opération OFFLINE a été initialisée est stockée dans le journal du cluster WSFC et dans le journal des erreurs SQL.  
  
-   **Si vous n’avez pas supprimé l’écouteur de groupe de disponibilité avant de mettre le groupe hors connexion :**  Si vous migrez le groupe de disponibilité vers un autre cluster WSFC, supprimez le VNN et le VIP de l’écouteur. Vous pouvez les supprimer à l’aide de la console de gestion du cluster de basculement, de l’applet de commande PowerShell [Remove-ClusterResource](https://technet.microsoft.com/library/ee461015\(WS.10\).aspx) ou de [cluster.exe](https://technet.microsoft.com/library/ee461015\(WS.10\).aspx). Notez que cluster.exe est déconseillé sur Windows 8.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Supprimer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md)  
  
-   [Changer le contexte de cluster HADR de l’instance de serveur &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
  
-   [Articles techniques SQL Server 2012](https://msdn.microsoft.com/library/bb418445\(SQL.10\).aspx)  
  
-   [Blog de l’équipe SQL Server Always On : Blog officiel de l’équipe SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>Voir aussi  
 [Groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
