---
title: Activer ou désactiver la fonctionnalité de groupe de disponibilité
description: Découvrez les étapes à suivre pour activer ou désactiver la fonctionnalité de groupe de disponibilité Always On à l’aide de Transact-SQL (T-SQL), PowerShell ou SQL Server Management Studio.
ms.custom: seodec18
ms.date: 08/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], server instance
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], disabling
- Availability Groups [SQL Server], enabling
ms.assetid: 7c326958-5ae9-4761-9c57-905972276a8f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 419684c0b5299d9bdb500b7c1f23fdef8e4bc987
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349292"
---
# <a name="enable-or-disable-always-on-availability-group-feature"></a>Activer ou désactiver la fonctionnalité de groupe de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  L'activation de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] est indispensable pour qu'une instance de serveur utilise des groupes de disponibilité. Avant de pouvoir créer et configurer un groupe de disponibilité, la fonctionnalité [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] doit avoir été activée sur chaque instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui hébergera un réplica de disponibilité pour un ou plusieurs groupes de disponibilité.  
  
> [!IMPORTANT]  
>  Si vous supprimez et recréez un cluster WSFC, vous devez désactiver puis réactiver la fonctionnalité [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sur chaque instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui hébergeait un réplica de disponibilité sur le cluster WSFC d'origine.  
  
  
##  <a name="prerequisites-for-enabling-always-on-availability-groups"></a><a name="Prerequisites"></a> Conditions préalables requises à l’activation des groupes de disponibilité Always On  
  
-   L'instance de serveur doit résider sur un nœud de Clustering de basculement Windows Server (WSFC).  
  
-   L'instance du serveur doit exécuter une édition de SQL Server qui prend en charge [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Pour plus d’informations, consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   Activez les groupes de disponibilité Always On sur une seule instance de serveur à la fois. Après avoir activé les groupes de disponibilité Always On, attendez le redémarrage du service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avant de passer à une autre instance de serveur.  
  
 Pour plus d’informations sur les autres conditions requises pour la création et la configuration de groupes de disponibilité, consultez [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
## <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Lorsque les groupes de disponibilité Always On sont activés sur une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], l’instance de serveur a le contrôle total sur le cluster WSFC.  

 Nécessite l'appartenance au groupe **Administrateur** sur l'ordinateur local et le contrôle total sur le cluster WSFC. Pour activer Always On à l’aide de PowerShell, ouvrez la fenêtre d’invite de commandes en utilisant l’option **Exécuter en tant qu’administrateur** .  
  
 Requiert des autorisations de création et de gestion d'objets Active Directory.  
  
##  <a name="determine-whether-always-on-availability-groups-is-enabled"></a><a name="IsEnabled"></a> Déterminer si l’option Groupes de disponibilité Always On est activée  
  
-   [SQL Server Management Studio](#SSMS1Procedure)  
  
-   [Transact-SQL](#Tsql1Procedure)  
  
-   [PowerShell](#PowerShell1Procedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMS1Procedure"></a> Utilisation de SQL Server Management Studio  
 **Pour déterminer si l’option Groupes de disponibilité Always On est activée**  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur l’instance de serveur, puis sélectionnez **Propriétés**.  
  
2.  Dans la boîte de dialogue **Propriétés du serveur** , cliquez sur la page **Général** . La propriété **Is HADR Enabled** affiche une des valeurs suivantes :  
  
    -   **True**, si l’option Groupes de disponibilité Always On est activée  
  
    -   **False**, si l’option Groupes de disponibilité Always On est désactivée.  
  
###  <a name="using-transact-sql"></a><a name="Tsql1Procedure"></a> Utilisation de Transact-SQL  
 **Pour déterminer si l’option Groupes de disponibilité Always On est activée**  
  
1.  Utilisez l'instruction [SERVERPROPERTY](../../../t-sql/functions/serverproperty-transact-sql.md) suivante :  
  
    ```  
    SELECT SERVERPROPERTY ('IsHadrEnabled');  
    ```  
  
     Le paramètre de la propriété de serveur **IsHadrEnabled** indique si une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est activée pour les groupes de disponibilité Always On, comme suit :  
  
    -   Si **IsHadrEnabled** = 1, l’option Groupes de disponibilité Always On est activée.  
  
    -   Si **IsHadrEnabled** = 0, l’option Groupes de disponibilité Always On est désactivée.  
  
    > [!NOTE]  
    >  Pour plus d’informations sur la propriété de serveur **IsHadrEnabled** , consultez [SERVERPROPERTY &#40;Transact-SQL&#41;](../../../t-sql/functions/serverproperty-transact-sql.md).  
  
###  <a name="using-powershell"></a><a name="PowerShell1Procedure"></a> Utilisation de PowerShell  
 **Pour déterminer si l’option Groupes de disponibilité Always On est activée**  
  
1.  Définissez la valeur par défaut (**cd**) sur l’instance de serveur pour laquelle vous voulez déterminer si les [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sont activés.  
  
2.  Entrez la commande PowerShell **Get-Item** suivante :  
  
    ```  
    PS SQLSERVER:\SQL\NODE1\DEFAULT> get-item . | select IsHadrEnabled  
    ```  
  
    > [!NOTE]  
    >  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="enable-always-on-availability-groups"></a><a name="EnableAOAG"></a> Activer les groupes de disponibilité Always On  
 **Pour activer Always On, utilisez :**  
  
-   [Gestionnaire de configuration SQL Server](#SQLCM2Procedure)  
  
-   [PowerShell](#PScmd2Procedure)  
  
###  <a name="using-sql-server-configuration-manager"></a><a name="SQLCM2Procedure"></a> Utilisation du Gestionnaire de configuration SQL Server  
 **Pour activer les groupes de disponibilité Always On**  
  
1.  Connectez-vous au nœud du cluster de basculement Windows Server qui héberge l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] où vous voulez activer les groupes de disponibilité Always On.  
  
2.  Dans le menu **Démarrer** , pointez sur **Tous les programmes**, sur [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)]et sur **Outils de configuration**, puis cliquez sur **Gestionnaire de configuration SQL Server**.  
  
3.  Dans le **Gestionnaire de configuration SQL Server**, cliquez sur **Services SQL Server**, cliquez avec le bouton droit sur SQL Server ( **\<**_instance name_**>)** , où **\<**_instance name_**>** est le nom d’une instance de serveur local pour laquelle vous souhaitez activer les groupes de disponibilité Always On, puis cliquez sur **Propriétés.**  
  
4.  Sélectionnez l’onglet **Haute disponibilité Always On**.  
  
5.  Vérifiez que le champ **Nom du cluster de basculement Windows** contient le nom du cluster de basculement local. Si ce champ est vide, cette instance de serveur ne prend pas en charge [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]pour le moment. L'ordinateur local n'est pas un nœud de cluster, le cluster WSFC a été arrêté, ou cette édition de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] ne prend pas en charge [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
6.  Cochez la case **Activer les groupes de disponibilité Always On** , puis cliquez sur **OK**.  
  
     [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] enregistre votre modification. Ensuite, vous devez redémarrer manuellement le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Cela vous permet de choisir l'heure de redémarrage la plus adaptée aux besoins de l'entreprise. Lorsque le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] redémarre, Always On est activé, et la propriété de serveur **IsHadrEnabled** a la valeur 1.  
  
###  <a name="using-sql-server-powershell"></a><a name="PScmd2Procedure"></a> Utilisation de PowerShell SQL Server  
 **Pour activer Always On**  
  
1.  Remplacez le répertoire (**cd**) par une instance de serveur que vous voulez activer pour les groupes de disponibilité Always On.  
  
2.  Utilisez l’applet de commande **Enable-SqlAlwaysOn** pour activer les groupes de disponibilité Always On.  
  
     Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
    > [!NOTE]  
    >  Pour plus d’informations sur la manière de contrôler si l’applet de commande **Enable-SqlAlwaysOn** redémarre le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [À quel moment une applet de commande redémarre-t-elle le service SQL Server ?](#WhenCmdletRestartsSQL), plus loin dans cette rubrique.  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
####  <a name="example-enable-sqlalwayson"></a><a name="ExmplEnable-SqlHadrServic"></a> Exemple : Enable-SqlAlwaysOn  
 La commande PowerShell suivante active les [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sur une instance de SQL Server (*Ordinateur*\\*Instance*).  
  
```  
Enable-SqlAlwaysOn -Path SQLSERVER:\SQL\Computer\Instance  
```  
  
##  <a name="disable-always-on-availability-groups"></a><a name="DisableAOAG"></a> Désactiver les groupes de disponibilité Always On  
  
-   **Avant de désactiver Always On :**  
  
     [Recommandations](#Recommendations)  
  
-   **Pour désactiver Always On, utilisez :**  
  
    -   [Gestionnaire de configuration SQL Server](#SQLCM3Procedure)  
  
    -   [PowerShell](#PScmd3Procedure)  
  
-   **Suivi :**  [Après la désactivation d’Always On](#FollowUp)  
  
> [!IMPORTANT]  
>  Désactivez Always On sur une seule instance de serveur à la fois. Après avoir désactivé les groupes de disponibilité Always On, attendez le redémarrage du service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avant de passer à une autre instance de serveur.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
 Avant de désactiver Always On sur une instance de serveur, nous vous recommandons d’effectuer les opérations suivantes :  
  
1.  Si l'instance de serveur héberge actuellement le réplica principal d'un groupe de disponibilité que vous souhaitez conserver, nous recommandons de basculer manuellement le groupe de disponibilité vers un réplica secondaire synchronisé, si possible. Pour plus d’informations, consultez [Effectuer un basculement manuel planifié d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md).  
  
2.  Supprimez tous les réplicas secondaires locaux. Pour plus d’informations, consultez [Supprimer un réplica secondaire d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md).  
  
###  <a name="using-sql-server-configuration-manager"></a><a name="SQLCM3Procedure"></a> Utilisation du Gestionnaire de configuration SQL Server  
 **Pour désactiver Always On**  
  
1.  Connectez-vous au nœud du cluster de basculement Windows Server qui héberge l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] où vous voulez désactiver les groupes de disponibilité Always On.  
  
2.  Dans le menu **Démarrer** , pointez sur **Tous les programmes**, sur [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)]et sur **Outils de configuration**, puis cliquez sur **Gestionnaire de configuration SQL Server**.  
  
3.  Dans le **Gestionnaire de configuration SQL Server**, cliquez sur **Services SQL Server**, cliquez avec le bouton droit sur SQL Server ( **\<**_instance name_**>)** , où **\<**_instance name_**>** est le nom d’une instance de serveur local pour laquelle vous souhaitez désactiver les groupes de disponibilité Always On, puis cliquez sur **Propriétés.**  
  
4.  Sous l’onglet **Haute disponibilité Always On**, décochez la case **Activer les groupes de disponibilité Always On**, puis cliquez sur **OK**.  
  
     [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] enregistre votre modification et redémarre le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Quand le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] redémarre, Always On est désactivé et la propriété de serveur **IsHadrEnabled** a la valeur 0, pour indiquer que l’option Groupes de disponibilité Always On est désactivée.  
  
5.  Nous vous conseillons de lire la section [Suivi : Après la désactivation d’Always On](#FollowUp), plus loin dans cette rubrique.  
  
###  <a name="using-sql-server-powershell"></a><a name="PScmd3Procedure"></a> Utilisation de PowerShell SQL Server  
 **Pour désactiver Always On**  
  
1.  Remplacez le répertoire (**cd**) par une instance de serveur actuellement activée que vous voulez désactiver pour les groupes de disponibilité Always On.  
  
2.  Utilisez l’applet de commande **Disable-SqlAlwaysOn** pour désactiver les groupes de disponibilité Always On.  
  
     Par exemple, la commande suivante désactive les groupes de disponibilité Always On sur une instance de SQL Server (*Ordinateur*\\*Instance*).  Cette commande nécessite le redémarrage de l'instance et vous serez invité à confirmer ce redémarrage.  
  
    ```  
    Disable-SqlAlwaysOn -Path SQLSERVER:\SQL\Computer\Instance  
    ```  
  
    > [!IMPORTANT]  
    >  Pour plus d’informations sur la manière de contrôler si l’applet de commande **Disable-SqlAlwaysOn** redémarre le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [À quel moment une applet de commande redémarre-t-elle le service SQL Server ?](#WhenCmdletRestartsSQL), plus loin dans cette rubrique.  
  
     Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
###  <a name="follow-up-after-disabling-always-on"></a><a name="FollowUp"></a> Suivi : Après la désactivation d’Always On  
 Après avoir désactivé des groupes de disponibilité Always On, l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit être redémarrée. Le Gestionnaire de configuration SQL redémarre l'instance de serveur automatiquement. Toutefois, si vous avez utilisé l’applet de commande **Disable-SqlAlwaysOn** , vous devez redémarrer l’instance de serveur manuellement. Pour plus d’informations, consultez [sqlservr Application](../../../tools/sqlservr-application.md).  
  
 Sur l'instance de serveur redémarrée :  
  
-   Les bases de données de disponibilité ne démarrent pas au démarrage de SQL Server, ce qui les rend inaccessibles.  
  
-   La seule instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] Always On prise en charge est [DROP AVAILABILITY GROUP](../../../t-sql/statements/drop-availability-group-transact-sql.md). Les options CREATE AVAILABILITY GROUP, ALTER AVAILABILITY GROUP et SET HADR de ALTER DATABASE ne sont pas prises en charge.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Les métadonnées et les données de configuration [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] dans WSFC ne sont pas affectées par la désactivation des groupes de disponibilité Always On.  
  
 Si vous désactivez définitivement des groupes de disponibilité Always On sur chaque instance de serveur qui héberge un réplica de disponibilité pour un ou plusieurs groupes de disponibilité, nous recommandons de procéder comme suit :  
  
1.  Si vous ne supprimez pas les réplicas de disponibilité locaux avant de désactiver Always On, supprimez (avec l’instruction Drop) chaque groupe de disponibilité pour lequel l’instance de serveur héberge un réplica de disponibilité. Pour plus d’informations sur la suppression d’un groupe de disponibilité, consultez [Supprimer un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md).  
  
2.  Pour supprimer les métadonnées restantes, supprimez chaque groupe de disponibilité affecté sur une instance de serveur qui fait partie du cluster de basculement Windows Server d’origine.  
  
3.  Toutes les bases de données primaires restent accessibles à toutes les connexions, mais la synchronisation des données entre les bases de données primaires et secondaires est arrêtée.  
  
4.  Les bases de données secondaires passent à l'état RESTORING. Vous pouvez les supprimer ou les restaurer à l'aide de RESTORE WITH RECOVERY. Toutefois, les bases de données restaurées ne participent plus à la synchronisation des données des groupes de disponibilité.  
  
##  <a name="when-does-a-cmdlet-restart-the-sql-server-service"></a><a name="WhenCmdletRestartsSQL"></a> À quel moment une applet de commande redémarre-t-elle le service SQL Server ?  
 Sur une instance de serveur en cours d’exécution, l’utilisation de l’applet de commande **Enable-SqlAlwaysOn** ou **Disable-SqlAlwaysOn** pour changer le paramètre Always On actuel peut entraîner le redémarrage du service SQL Server. Le comportement de redémarrage dépend des conditions suivantes :  
  
|Paramètre -NoServiceRestart spécifié|Paramètre -Force spécifié|Le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a-t-il été redémarré ?|  
|--------------------------------------------|---------------------------------|---------------------------------------------------------|  
|Non|Non|Par défaut. Mais l'applet de commande vous invite à agir comme suit :<br /><br /> **Pour permettre l’achèvement de cette action, le service SQL Server de l’instance de serveur <nom_instance> doit redémarrer. Voulez-vous continuer ? »**<br /><br /> **[Y] Oui [N] Non [S] Suspendre [?] Aide (la valeur par défaut est « Y ») :**<br /><br /> Si vous spécifiez **N** ou **S**, le service n'est pas redémarré.|  
|Non|Oui|Le service est redémarré.|  
|Oui|Non|Le service n'est pas redémarré.|  
|Oui|Oui|Le service n'est pas redémarré.|  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [SERVERPROPERTY &#40;Transact-SQL&#41;](../../../t-sql/functions/serverproperty-transact-sql.md)  
  
