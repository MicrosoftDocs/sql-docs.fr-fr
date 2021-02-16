---
title: Modifier le délai d’expiration de session pour un réplica de groupe de disponibilité
description: Explique comment configurer la période d’expiration de session d’un réplica dans un groupe de disponibilité Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], session timeout
- session timeout [SQL Server]
ms.assetid: e23c6e06-1cd1-4d4a-9bc2-e3e06ab2933d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8aebb45eb25338a2303e607f1a37a877af999ed8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343797"
---
# <a name="modify-the-session-timeout-period-for-an-availability-group-replica"></a>Modifier la période d’expiration de session pour un réplica de groupe de disponibilité
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment configurer la période d’expiration de session d’un réplica de disponibilité Always On à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou de PowerShell dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. La période d'expiration de session est une propriété de réplica qui contrôle le nombre de secondes (en secondes) pendant lequel un réplica de disponibilité attend une réponse ping d'un réplica connecté avant de considérer que la connexion a échoué. Par défaut, un réplica attend une réponse ping pendant 10 secondes. Cette propriété de réplica applique uniquement la connexion entre un réplica secondaire donné et le réplica principal du groupe de disponibilité. Pour plus d’informations sur le délai d’expiration de session, consultez [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
   
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Vous devez être connecté à l'instance de serveur qui héberge le réplica principal.  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
 Le temps d'attente recommandé est de 10 secondes minimum. En définissant une valeur inférieure à 10 secondes, vous créez la possibilité qu'un système surchargé soit à court de PING et qu'il déclare à tort une défaillance.  
  
  
## <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 **Pour modifier la période d'expiration de session pour un réplica de disponibilité**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica principal et développez l'arborescence du serveur.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Cliquez sur le groupe de disponibilité dont vous souhaitez configurer le réplica de disponibilité.  
  
4.  Cliquez avec le bouton droit sur le réplica à configurer, puis sélectionnez **Propriétés**.  
  
5.  Dans la boîte de dialogue **Propriétés du réplica de disponibilité** , utilisez le champ **Délai d’expiration de session (secondes)** pour modifier le nombre de secondes pour la période d’expiration de session sur ce réplica.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour modifier la période d'expiration de session pour un réplica de disponibilité**  
  
1.  Connectez-vous à l'instance de serveur qui héberge le réplica principal.  
  
2.  Utilisez l'instruction [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , comme suit :  
  
     ALTER AVAILABILITY GROUP *nom_groupe*  
  
     MODIFY REPLICA ON «*nom_instance*» WITH ( SESSION_TIMEOUT =*secondes* )  
  
     où *nom_groupe* est le nom du groupe de disponibilité, *nom_instance* est le nom de l’instance de serveur qui héberge le réplica de disponibilité à modifier, et *secondes* spécifie le nombre minimal de secondes pendant lesquelles le réplica doit attendre avant d’appliquer un journal aux bases de données lorsqu’il joue le rôle de réplica secondaire. La valeur par défaut est de 0 seconde, ce qui indique qu'il n'existe pas de délai d'application.  
  
     L'exemple suivant, écrit sur le réplica principal du groupe de disponibilité `AccountsAG` modifie la valeur d'expiration de session sur `15` secondes pour le réplica situé sur l'instance de serveur `INSTANCE09` .  
  
    ```  
    ALTER AVAILABILITY GROUP AccountsAG   
       MODIFY REPLICA ON 'INSTANCE09' WITH (SESSION_TIMEOUT = 15);  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell  
 **Pour modifier la période d'expiration de session pour un réplica de disponibilité**  
  
1.  Remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica principal.  
  
2.  Utilisez l’applet de commande **Set-SqlAvailabilityReplica** avec le paramètre **SessionTimeout** pour modifier le nombre de secondes pour la période d’expiration de session sur un réplica de disponibilité spécifié.  
  
     Par exemple, la commande suivante définit le délai d'expiration de session sur 15 secondes.  
  
    ```  
    Set-SqlAvailabilityReplica -SessionTimeout 15 `   
    -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
    > [!NOTE]  
    >  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
