---
title: Supprimer un groupe de disponibilité
description: 'Explique comment supprimer un groupe de disponibilité à l’aide de SSMS (SQL Server Management Studio), T-SQL (Transact-SQL) ou SQL PowerShell. '
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.deleteag.f1
helpviewer_keywords:
- Availability Groups [SQL Server], removing
- Availability Groups [SQL Server], dropping
ms.assetid: 4b7f7f62-43a3-49db-a72e-22d4d7c2ddbb
author: cawrites
ms.author: chadam
ms.openlocfilehash: bfd907b77633bdecc1af1371c83da4a4d4bff9a9
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783483"
---
# <a name="remove-an-availability-group-sql-server"></a>Supprimer un groupe de disponibilité (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cet article explique comment supprimer un groupe de disponibilité Always On à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou PowerShell dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Si une instance de serveur qui héberge l'un des réplicas de disponibilité est hors connexion lorsque vous supprimez un groupe de disponibilité, une fois de nouveau en ligne, l'instance de serveur supprimera le réplica de disponibilité local. La suppression d'un groupe de disponibilité supprime tout écouteur du groupe de disponibilité associé.  
  
 Notez qu'en cas de besoin, vous pouvez supprimer un groupe de disponibilité de tout nœud de clustering de basculement Windows Server (WSFC) qui possède les informations d'identification de sécurité correctes pour le groupe de disponibilité. Cela vous permet de supprimer un groupe de disponibilité lorsqu'il ne reste aucun de ses réplicas de disponibilité.  
  
> [!IMPORTANT]  
>  si cela est possible, supprimez le groupe de disponibilité uniquement lorsque vous êtes connecté à l'instance de serveur qui héberge le réplica principal. Si le groupe de disponibilité est supprimé du réplica principal, les modifications sont autorisées dans les bases de données primaires précédentes (sans protection haute disponibilité). Le fait de supprimer un groupe de disponibilité d'un réplica secondaire conserve le réplica principal dans l'état RESTORING, et les modifications ne sont pas autorisées sur les bases de données.  

  
## <a name="limitations-and-recommendations"></a><a name="Restrictions"></a> Recommandations et limitations  
  
-   Si le groupe de disponibilité est en ligne, sa suppression d'un réplica secondaire entraîne le passage du réplica principal à l'état RESTORING. Par conséquent, et si cela est possible, ne supprimez le groupe de disponibilité que de l'instance de serveur qui héberge le réplica principal.    
-   Si vous supprimez un groupe de disponibilité d'un ordinateur qui a été supprimé ou évincé du cluster de basculement WSFC, le groupe de disponibilité est supprimé uniquement localement. 
-   Évitez de supprimer un groupe de disponibilité lorsque le cluster de clustering de basculement Windows Server (WSFC) n'a aucun quorum. Si vous devez supprimer un groupe de disponibilité lorsque le cluster ne dispose pas de quorum, les métadonnées du groupe de disponibilité stockées dans le cluster nesont pas supprimées. Après que le cluster a regagné le quorum, vous devez supprimer à nouveau le groupe de disponibilité pour le supprimer du cluster WSFC.    
-   Sur un réplica secondaire, DROP AVAILABILITY GROUP ne doit être utilisé qu'en cas d'urgence. Cela est dû au fait que la suppression d'un groupe de disponibilité met le groupe de disponibilité hors connexion. Si vous supprimez le groupe de disponibilité d'un réplica secondaire, le réplica principal ne peut pas déterminer si l'état OFFLINE se produit en raison de la perte de quorum, d'un basculement forcé ou d'une commande DROP AVAILABILITY GROUP. Le réplica principal passe à l'état RESTORING pour éviter un fractionnement possible des partitions. Pour plus d’informations, consultez [Fonctionnement : comportements de DROP AVAILABILITY GROUP](/archive/blogs/psssql/how-it-works-drop-availability-group-behaviors) (blog des ingénieurs du Service clientèle et du Support technique de SQL Server).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER. Pour supprimer un groupe de disponibilité qui n'est pas hébergé par l'instance de serveur local, vous avez besoin de l'autorisation CONTROL SERVER ou CONTROL sur ce groupe de disponibilité.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour supprimer un groupe de disponibilité**  
  
1.  Dans l’Explorateur d’objets, connectez-vous à l’instance de serveur qui héberge le réplica principal, si possible, ou connectez-vous à une autre instance de serveur qui est activée pour les groupes de disponibilité Always On sur un nœud WSFC comportant les informations d’identification de sécurité appropriées pour le groupe de disponibilité. Développez l'arborescence du serveur.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Cette étape varie selon que vous souhaitez supprimer plusieurs groupes de disponibilité ou un seul, comme suit :  
  
    -   Pour supprimer plusieurs groupes de disponibilité (dont les réplicas principaux figurent sur l’instance de serveur connectée), utilisez le volet **Détails de l’Explorateur d’objets** pour afficher et sélectionner tous les groupes de disponibilité à supprimer. Pour plus d’informations, consultez [Utiliser les détails de l’Explorateur d’objets pour surveiller les groupes de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md).  
  
    -   Pour supprimer un seul groupe de disponibilité, sélectionnez-le dans le volet **Explorateur d'objets** ou le volet **Détails de l'Explorateur d'objets** .  
  
4.  Cliquez avec le bouton droit sur le ou les groupes de disponibilité sélectionnés, puis sélectionnez la commande **Supprimer** .  
  
5.  Dans la boîte de dialogue **Supprimer le groupe de disponibilité** , pour supprimer tous les groupes de disponibilité répertoriés, cliquez sur **OK**. Si vous ne souhaitez pas supprimer tous les groupes de disponibilité répertoriés, cliquez sur **Annuler**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour supprimer un groupe de disponibilité**  
  
1.  Connectez-vous à l’instance de serveur qui héberge le réplica principal, si possible, ou connectez-vous à une autre instance de serveur qui est activée pour les groupes de disponibilité Always On sur un nœud WSFC comportant les informations d’identification de sécurité appropriées pour le groupe de disponibilité.  
  
2.  Utilisez l'instruction [DROP AVAILABILITY GROUP](../../../t-sql/statements/drop-availability-group-transact-sql.md) , comme suit :  
  
     DROP AVAILABILITY GROUP *nom_groupe*  
  
     où *nom_groupe* est le nom du groupe de disponibilité à supprimer.  
  
     L'exemple suivant supprime le groupe de disponibilité `MyAG` .  
  
    ```  
    DROP AVAILABILITY GROUP MyAG;  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell  
 **Pour supprimer un groupe de disponibilité**  
  
 Dans le fournisseur PowerShell [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
1.  Remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica principal, si possible, ou connectez-vous à une autre instance de serveur qui est activée pour les groupes de disponibilité Always On sur un nœud WSFC comportant les informations d’identification de sécurité appropriées pour le groupe de disponibilité.  
  
2.  Utilisez l’applet de commande **Remove-SqlAvailabilityGroup** .  
  
     Par exemple, la commande suivante supprime le groupe de disponibilité nommé `MyAg`. Cette commande peut être exécutée sur n'importe quelle instance de serveur qui héberge un réplica de disponibilité pour le groupe de disponibilité.  
  
    ```  
    Remove-SqlAvailabilityGroup `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg  
    ```  
  
    > [!NOTE]  
    >  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
  
-   [Fonctionnement : comportements de DROP AVAILABILITY GROUP](/archive/blogs/psssql/how-it-works-drop-availability-group-behaviors) (blog des ingénieurs du Service clientèle et du Support technique de SQL Server)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Création et configuration des groupes de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)  
  
