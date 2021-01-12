---
title: Configurer un écouteur de groupe de disponibilité
description: 'Explique comment configurer un écouteur pour un groupe de disponibilité Always On à l’aide de PowerShell ou SQL Server Management Studio. '
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
f1_keywords:
- sql13.swb.availabilitygroup.newaglistener.general.f1
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- Availability Groups [SQL Server], client connectivity
ms.assetid: 2bc294f6-2312-4b6b-9478-2fb8a656e645
author: cawrites
ms.author: chadam
manager: erikre
ms.openlocfilehash: 6e6b362f9e121f10515753d46ee098387348f272
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091733"
---
# <a name="configure-a-listener-for-an-always-on-availability-group"></a>Configurer un écouteur Always On pour un groupe de disponibilité
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment créer ou configurer un *écouteur de groupe de disponibilité* unique pour un groupe de disponibilité Always On à l’aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou de PowerShell dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
> [!IMPORTANT]  
>  Pour créer le premier écouteur d’un groupe de disponibilité, nous vous recommandons vivement d’utiliser [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Évitez de créer un écouteur directement dans le cluster WSFC, sauf si cela s'avère nécessaire, par exemple, pour créer un écouteur supplémentaire.  
  
 
##  <a name="does-a-listener-exist-for-this-availability-group-already"></a><a name="DoesListenerExist"></a> Existe-t-il déjà un écouteur pour ce groupe de disponibilité ?  

 **Pour déterminer si un écouteur existe déjà pour le groupe de disponibilité**  
  
-   [Afficher les propriétés d’écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)  
  
> [!NOTE]  
>  Si un écouteur existe déjà et que vous souhaitez créer un écouteur supplémentaire, consultez [Pour créer un écouteur supplémentaire pour un groupe de disponibilité (facultatif)](#CreateAdditionalListener), plus loin dans cette rubrique.  
  
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Vous ne pouvez créer qu'un seul écouteur par groupe de disponibilité via [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. En général, chaque groupe de disponibilité nécessite un seul écouteur. Toutefois, certains scénarios de client requièrent plusieurs écouteurs pour un groupe de disponibilité.   Après la création d'un écouteur par SQL Server, vous pouvez utiliser Windows PowerShell pour les clusters de basculement ou le gestionnaire de cluster de basculement WSFC pour crée des écouteurs supplémentaires. Pour plus d’informations, consultez [Pour créer un écouteur supplémentaire pour un groupe de disponibilité (facultatif)](#CreateAdditionalListener), plus loin dans cette rubrique.  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
 L'utilisation d'une adresse IP statique est recommandée, mais n'est pas obligatoire, en cas de configurations de plusieurs sous-réseaux.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Vous devez être connecté à l'instance de serveur qui héberge le réplica principal.  
  
-   Si vous configurez un écouteur de groupe de disponibilité sur plusieurs sous-réseaux et si vous planifiez d'utiliser des adresses IP statiques, vous devez obtenir l'adresse IP statique de chaque sous-réseau qui héberge un réplica de disponibilité pour le groupe de disponibilité pour lequel vous créez l'écouteur. Généralement, vous devez demander à votre administrateur réseau de vous communiquer les adresses IP statiques.  
  
> [!IMPORTANT]  
>  Avant de créer votre premier écouteur, nous vous recommandons fortement de lire [Connectivité client Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
###  <a name="requirements-for-the-dns-name-of-an-availability-group-listener"></a><a name="DNSnameReqs"></a> Conditions requises pour le nom DNS d'un écouteur de groupe de disponibilité  
 Chaque écouteur du groupe de disponibilité a besoin d'un nom d'hôte DNS unique dans le domaine et dans NetBIOS. Le nom DNS est une valeur de chaîne. Ce nom ne peut contenir que des caractères alphanumériques, des tirets/traits d’union (-) et des traits de soulignement (_), quel que soit l’ordre. Les noms d'hôte DNS ne respectent pas la casse. La longueur maximale est de 63 caractères, toutefois, dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], la longueur maximale que vous pouvez spécifier est 15 caractères.  
  
 Nous vous recommandons de spécifier une chaîne explicite. Par exemple, pour un groupe de disponibilité nommé `AG1`, un nom d'hôte DNS explicite est `ag1-listener`.  
  
> [!IMPORTANT]  
>  NetBIOS identifie les 15 premiers caractères du nom_dns. Si vous avez deux clusters WSFC qui sont contrôlés par le même annuaire Active Directory et que vous tentez de créer des écouteurs de groupe de disponibilité dans les deux clusters à l'aide de noms contenant plus de 15 caractères et un préfixe identique de 15 caractères, vous obtenez une erreur signalant que la ressource de nom de réseau virtuel ne peut pas être mise en ligne. Pour plus d'informations sur les règles de préfixe des noms DNS, consultez [Attribution de noms de domaine](https://technet.microsoft.com/library/cc731265\(WS.10\).aspx).  
  
##  <a name="windows-permissions"></a><a name="WinPermissions"></a> Autorisations Windows  
  
|Autorisations|Lien|  
|-----------------|----------|  
|Le nom d’objet cluster (CNO) du cluster WSFC qui héberge le groupe de disponibilité doit disposer de l’autorisation de **création d’objets ordinateur** .<br /><br /> Dans Active Directory, un CNO ne dispose pas par défaut explicitement de l’autorisation de **création d’objets ordinateur** et peut créer 10 objets ordinateur virtuel (VCO). Une fois les 10 VCO créés, la création de VCO supplémentaires échoue. Vous pouvez éviter cela en accordant l’autorisation explicite au CNO du cluster WSFC. Notez que les VCO des groupes de disponibilité que vous avez supprimé ne sont pas supprimés automatiquement dans Active Directory et sont pris en compte dans le nombre maximal par défaut de 10 VCO, sauf s'ils sont supprimés manuellement.<br /><br /> Remarque : Dans certaines organisations, la stratégie de sécurité interdit d’accorder l’autorisation de **création d’objets ordinateur** aux comptes d’utilisateur individuels.|*Étapes pour configurer le compte de la personne qui installe le cluster* dans le [Guide pas à pas du cluster de basculement : Configuration des comptes dans Active Directory](https://technet.microsoft.com/library/cc731002\(WS.10\).aspx#BKMK_steps_installer)<br /><br /> *Étapes de préconfiguration du nom du compte de cluster* dans le [Guide pas à pas du cluster de basculement : Configuration des comptes dans Active Directory](https://technet.microsoft.com/library/cc731002\(WS.10\).aspx#BKMK_steps_precreating)|  
|Si votre organisation requiert la préconfiguration du compte d’ordinateur pour un nom de réseau virtuel d’écouteur, vous devrez être membre du groupe **Opérateur de compte** ou vous aurez besoin de l’aide de l’administrateur de domaine.|*Étapes de préconfiguration d’un compte pour un service cluster ou une application*  dans le [Guide pas à pas du cluster de basculement : Configuration des comptes dans Active Directory](https://technet.microsoft.com/library/cc731002\(WS.10\).aspx#BKMK_steps_precreating2).|  
  
> [!TIP]  
>  En général, il est plus simple de ne pas préconfigurer le compte d'ordinateur pour un nom de réseau virtuel d'écouteur. Si vous le pouvez, laissez le compte être créé et configuré automatiquement lorsque vous exécutez l'Assistant WSFC haute disponibilité.  
  
##  <a name="sql-server-permissions"></a><a name="SqlPermissions"></a> Autorisations SQL Server  
  
|Tâche|Autorisations|  
|----------|-----------------|  
|Pour créer un écouteur de groupe de disponibilité|Requiert l’appartenance au rôle serveur fixe **sysadmin** et l’autorisation de serveur CREATE AVAILABILITY GROUP, l’autorisation ALTER ANY AVAILABILITY GROUP ou l’autorisation CONTROL SERVER.|  
|Pour modifier un écouteur de groupe de disponibilité existant|Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.|  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
> [!TIP]  
>  L’ [Assistant Nouveau groupe de disponibilité](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md) prend en charge la création de l’écouteur pour un nouveau groupe de disponibilité.  
  
 **Pour créer ou configurer un écouteur de groupe de disponibilité**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica principal du groupe de disponibilité, puis cliquez sur le nom du serveur pour développer l'arborescence du serveur.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Cliquez sur le groupe de disponibilité dont vous souhaitez configurer l'écouteur, puis choisissez l'une des méthodes suivantes :  
  
    -   Pour créer un écouteur, cliquez avec le bouton droit sur le nœud **Écouteurs de groupe de disponibilité** , puis sélectionnez la commande **Nouvel écouteur** . Cela ouvre la boîte de dialogue **Nouvel écouteur du groupe de disponibilité** . Pour plus d’informations, consultez [Ajouter l’écouteur du groupe de disponibilité (boîte de dialogue)](#AddAgListenerDialog), plus loin dans cette rubrique.  
  
    -   Pour modifier le numéro de port d’un écouteur existant, développez le nœud **Écouteurs de groupe de disponibilité** , cliquez avec le bouton droit sur l’écouteur, puis sélectionnez la commande **Propriétés** . Entrez le nouveau numéro de port dans le champ **Port** , puis cliquez sur **OK**.  
  
###  <a name="new-availability-group-listener-dialog-box"></a><a name="AddAgListenerDialog"></a> Nouvel écouteur du groupe de disponibilité (boîte de dialogue)  
 **Nom DNS de l'écouteur**  
 Spécifie le nom d'hôte DNS de l'écouteur du groupe de disponibilité. Le nom DNS est une chaîne et doit être unique dans le domaine et dans NetBIOS. Ce nom ne peut contenir que des caractères alphanumériques, des tirets (-) et des caractères de soulignement (_), dans n'importe quel ordre. Les noms d'hôte DNS ne respectent pas la casse. La longueur maximale autorisée s'élève à 15 caractères.  
  
 Pour plus d'informations, consultez [Conditions requises pour le nom DNS d'un écouteur de groupe de disponibilité](#DNSnameReqs), plus haut dans cette rubrique.  
  
 **Port**  
 Port TCP utilisé par cet écouteur.  
  
 **Mode réseau**  
 Indique le protocole TCP utilisé par l'écouteur, à savoir :  
  
 **DHCP**  
 L'écouteur utilise une adresse IP dynamique affectée par un serveur exécutant le protocole DHCP (Dynamic Host Configuration Protocol). DHCP est limité à un sous-réseau.  
  
> [!IMPORTANT]  
>  Nous vous déconseillons d'utiliser DHCP dans un environnement de production. Lorsqu'un temps mort se produit et que le bail IP DHCP arrive à expiration, une période de temps supplémentaire est requise pour enregistrer la nouvelle adresse IP de réseau DHCP associée au nom DNS de l'écouteur et cela a un impact sur la connectivité client. Toutefois, DHCP peut tout à fait être utilisé pour configurer un environnement de développement et de test dans le but de vérifier les fonctions de base des groupes de disponibilité et l'intégration avec vos applications.  
  
 **Adresse IP statique**  
 L'écouteur utilise une ou plusieurs adresses IP statiques. Les adresses IP supplémentaires sont facultatives. Pour créer un écouteur de groupe de disponibilité sur plusieurs sous-réseaux, vous devez spécifier, pour chaque sous-réseau, une adresse IP statique dans la configuration de l'écouteur. Contactez votre administrateur réseau pour obtenir ces adresses IP statiques.  
  
 Si vous sélectionnez **Adresse IP statique** , une grille de sous-réseau apparaît sous le champ **Mode réseau** . Cette grille affiche des informations sur chaque sous-réseau accessible par cet écouteur de groupe de disponibilité. Cette grille est vide tant que vous n'avez pas ajouté une adresse IP statique en cliquant sur **Ajouter**.  
  
 Les colonnes sont les suivantes :  
  
 **Sous-réseau**  
 Affiche l'identificateur de chaque sous-réseau que vous ajoutez à l'écouteur du groupe de disponibilité.  
  
 **Adresse IP**  
 Affiche l'adresse IP d'un sous-réseau donné.  Pour un sous-réseau donné, l'adresse IP est une adresse IPv4 ou une adresse IPv6.  
  
 **Ajouter**  
 Cliquez pour ajouter une adresse IP statique dans un sous-réseau sélectionné ou un autre sous-réseau pour cet écouteur. Cela ouvre la boîte de dialogue **Ajouter une adresse IP** . Pour plus d’informations, consultez la rubrique d’aide [Boîte de dialogue Ajouter une adresse IP &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/add-ip-address-dialog-box-sql-server-management-studio.md).  
  
 **Remove**  
 Cliquez pour supprimer le sous-réseau sélectionné de cet écouteur.  
  
 **OK**  
 Cliquez pour créer l'écouteur de groupe de disponibilité spécifié.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour créer ou configurer un écouteur de groupe de disponibilité**  
  
1.  Connectez-vous à l'instance de serveur qui héberge le réplica principal.  
  
2.  Utilisez l'option LISTENER de l'instruction [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md) ou de l'option ADD LISTENER de l'instruction [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) .  
  
     L'exemple suivant ajoute un écouteur de groupe de disponibilité à un groupe de disponibilité existant nommé `MyAg2`. Un seul nom DNS, `MyAg2ListenerIvP6`, est spécifié pour cet écouteur. Les deux réplicas sont sur des sous-réseaux différents, par conséquent, selon les recommandations, l'écouteur doit utiliser des adresses IP statiques. Pour les deux réplicas de disponibilité, la clause WITH IP spécifie une adresse IP statique, `2001:4898:f0:f00f::cf3c and 2001:4898:e0:f213::4ce2`, qui utilisent le format IPv6. Cet exemple utilise également l'argument facultatif PORT pour spécifier le port `60173` comme port d'écoute.  
  
    ```  
    ALTER AVAILABILITY GROUP MyAg2   
          ADD LISTENER 'MyAg2ListenerIvP6' ( WITH IP ( ('2001:db88:f0:f00f::cf3c'),('2001:4898:e0:f213::4ce2') ) , PORT = 60173 );   
    GO  
  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell  
 **Pour créer ou configurer un écouteur de groupe de disponibilité**  
  
1.  Remplacez le répertoire (**cd**) par l’instance de serveur qui héberge le réplica principal.  
  
2.  Pour créer ou modifier un écouteur de groupe de disponibilité, utilisez l'une des applets de commande suivantes :  
  
     **New-SqlAvailabilityGroupListener**  
     Crée un écouteur de groupe de disponibilité et l'attache à un groupe de disponibilité existant.  
  
     Par exemple, la commande **New-SqlAvailabilityGroupListener** suivante crée un écouteur de groupe de disponibilité nommé `MyListener` pour le groupe de disponibilité `MyAg`. Cet écouteur utilise l’adresse IPv4 passée au paramètre **-StaticIp** comme adresse IP virtuelle.  
  
    ```  
    New-SqlAvailabilityGroupListener -Name MyListener `   
    -StaticIp '192.168.3.1/255.255.252.0' `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg  
  
    ```  
  
     **Set-SqlAvailabilityGroupListener**  
     Modifie le paramètre de port sur un écouteur de groupe de disponibilité existant.  
  
     Par exemple, la commande **Set-SqlAvailabilityGroupListener** suivante affecte le numéro de port `MyListener` pour l’écouteur de groupe de disponibilité nommé `1535`. Ce port est utilisé pour écouter les connexions à l'écouteur.  
  
    ```  
    Set-SqlAvailabilityGroupListener -Port 1535 `   
    -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AGListeners\MyListener  
  
    ```  
  
     **Add-SqlAGListenerstaticIp**  
     Ajoute une adresse IP statique à une configuration existante d'écouteur de groupe de disponibilité. L'adresse IP peut être une adresse IPv4 avec sous-réseau ou une adresse IPv6.  
  
     Par exemple, la commande **Add-SqlAGListenerstaticIp** suivante ajoute une adresse IPv4 statique à l’écouteur de groupe de disponibilité `MyListener` sur le groupe de disponibilité `MyAg`. Cette adresse IPv6 sert d'adresse IP virtuelle de l'écouteur sur le sous-réseau `255.255.252.0`. Si le groupe de disponibilité s'étend sur plusieurs sous-réseaux, vous devez ajouter une adresse IP statique pour chaque sous-réseau connecté à l'écouteur.  
  
    ```  
    $path = "SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAg\AGListeners\ MyListener" `   
    Add-SqlAGListenerstaticIp -Path $path `   
    -StaticIp "2001:0db8:85a3:0000:0000:8a2e:0370:7334"  
    ```  
  
    > [!NOTE]  
    >  Pour afficher la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help**  dans l’environnement PowerShell [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
## <a name="troubleshooting"></a>Dépannage  
  
###  <a name="failure-to-create-an-availability-group-listener-because-of-active-directory-quotas"></a><a name="ADQuotas"></a> Échec de création d'un écouteur de groupe de disponibilité en raison de quotas Active Directory  
 La création d'un nouvel écouteur de groupe de disponibilité peut échouer parce que vous avez atteint un quota Active Directory pour le compte d'ordinateur participant du nœud de cluster.  Pour plus d’informations, consultez les articles suivants :  
  
-   [Procédure de dépannage du compte de service de cluster lorsqu'il modifie des objets ordinateur](https://support.microsoft.com/kb/307532)  
  
-   [Quotas Active Directory](https://technet.microsoft.com/library/cc904295\(WS.10\).aspx)  
  
##  <a name="follow-up-after-creating-an-availability-group-listener"></a><a name="FollowUp"></a> Suivi : Après la création d’un écouteur de groupe de disponibilité  
  
###  <a name="multisubnetfailover-keyword-and-associated-features"></a><a name="MultiSubnetFailover"></a> Mot clé MultiSubnetFailover et fonctionnalités associées  
 **MultiSubnetFailover** est un nouveau mot clé de chaîne de connexion utilisé pour permettre un basculement plus rapide avec les groupes de disponibilité Always On et les instances de cluster de basculement Always On dans SQL Server 2012. Les trois sous-fonctionnalités suivantes sont activées lorsque `MultiSubnetFailover=True` est défini dans la chaîne de connexion :  
  
-   Basculement plus rapide de plusieurs sous-réseaux vers un écouteur de plusieurs sous-réseaux pour un groupe de disponibilité Always On ou des instances de cluster de basculement.  
  
-   Basculement plus rapide d’un sous-réseau unique vers un écouteur de sous-réseau unique pour un groupe de disponibilité Always On ou des instances de cluster de basculement.  
  
    -   Cette fonctionnalité est utilisée pour la connexion à un écouteur comportant une IP unique dans un seul sous-réseau. Elle permet d'exécuter des tentatives de connexion TCP plus agressives afin d'accélérer les basculements au sein d'un sous-réseau unique.  
  
-   Résolution d’instance nommée pour une instance de cluster de basculement Always On de plusieurs sous-réseaux.  
  
    -   Elle permet de prendre en charge la résolution d’instance nommée pour des instances de cluster de basculement Always On avec des points de terminaison de plusieurs sous-réseaux.  
  
 **MultiSubnetFailover=True non pris en charge par NET Framework 3.5 ou OLEDB**  
  
 **Problème :** Si votre groupe de disponibilité ou votre instance de cluster de basculement comporte un nom d’écouteur (également appelé « nom de réseau » ou « point d’accès client » dans le gestionnaire de cluster WSFC) qui dépend de plusieurs adresses IP de plusieurs sous-réseaux, et si vous utilisez soit ADO.NET avec .NET Framework 3.5SP1, soit SQL Native Client 11.0 OLEDB, 50 % de vos demandes de connexion client à l’écouteur du groupe de disponibilité sont susceptibles de dépasser le délai de connexion.  
  
 **Solutions de contournement :** nous vous recommandons d’effectuer l’une des tâches suivantes.  
  
-   Si vous n'êtes pas autorisé à manipuler les ressources de cluster, définissez le délai de connexion à 30 secondes (cette valeur correspond au délai TCP de 20 secondes plus un tampon de 10 secondes).  
  
     **Avantages** : en cas de basculement de sous-réseaux croisé, la durée de récupération du client est courte.  
  
     **Inconvénients** : la moitié des connexions du client prendront plus de 20 secondes.  
  
-   Si vous disposez d'une autorisation vous permettant de manipuler les ressources du cluster, l'approche privilégiée consiste à définir le nom du réseau de votre écouteur de groupe de disponibilité en tant que `RegisterAllProvidersIP=0`. Pour plus d’informations, consultez « Paramètre RegisterAllProvidersIP » plus loin dans cette section.  
  
     **Avantages** : vous n’avez pas besoin d’augmenter la valeur du délai de connexion du client.  
  
     **Inconvénients :** en cas de basculement entre sous-réseaux, la durée de récupération du client peut être de 15 minutes ou plus, en fonction de votre paramètre **HostRecordTTL** et du paramètre de votre planification de la réplication DNS/AD intersites.  
  
###  <a name="registerallprovidersip-setting"></a><a name="RegisterAllProvidersIP"></a> Paramètre RegisterAllProvidersIP  
 Quand vous utilisez [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou PowerShell pour créer un écouteur de groupe de disponibilité, le point d’accès client est créé dans WSFC et la valeur 1 (True) est affectée à la propriété **RegisterAllProvidersIP** . L'effet de cette valeur de propriété dépend de la chaîne de connexion du client, comme suit :  
  
-   Chaînes de connexion qui affectent la valeur True à **MultiSubnetFailover**  
  
     [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] affecte la valeur 1 à la propriété  **RegisterAllProvidersIP** pour réduire le temps de reconnexion après un basculement pour les clients dont les chaînes de connexion du client spécifient `MultiSubnetFailover = True`, comme cela est recommandé. Notez que, pour tirer parti des fonctionnalités de plusieurs sous-réseaux d’écouteur, les clients peuvent nécessiter un fournisseur de données qui prend en charge le mot clé **MultiSubnetFailover** . Pour plus d’informations sur la prise en charge du pilote pour le basculement de plusieurs sous-réseaux, consultez [Connectivité client Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
     Pour plus d’informations sur le clustering de sous-réseaux multiples, consultez [Clustering de sous-réseaux multiples SQL Server &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md).  
  
    > [!TIP]  
    >  Lorsque `RegisterAllProvidersIP = 1`, si vous exécutez l'Assistant WSFC Validation d'une configuration sur le cluster WSFC, l'Assistant génère le message d'avertissement suivant :  
    >   
    >  « La propriété RegisterAllProviderIP du nom réseau 'Name:<nom_réseau>' est définie sur 1. Pour la configuration de cluster actuelle, cette valeur doit être définie sur 0. »  
    >   
    >  Veuillez ignorer ce message.  
  
-   Chaînes de connexion qui n'affectent pas la valeur True à **MultiSubnetFailover**  
  
     Lorsque `RegisterAllProvidersIP = 1`, tous les clients dont les chaînes de connexion n'utilisent pas `MultiSubnetFailover = True`rencontreront des connexions à latence élevée. Cela est dû au fait que les clients essaient de se connecter à toutes les adresses IP de manière séquentielle. En revanche, si **RegisterAllProvidersIP** est changé en 0, l'adresse IP active est enregistrée dans le point d'accès client du cluster WSFC, ce qui réduit la latence pour les clients hérités. Par conséquent, si vous possédez des clients hérités qui doivent se connecter à un écouteur du groupe de disponibilité et ne peuvent pas utiliser la propriété **MultiSubnetFailover** , nous vous recommandons de changer **RegisterAllProvidersIP** en 0.  
  
    > [!IMPORTANT]  
    >  Quand vous créez un écouteur de groupe de disponibilité via le cluster WSFC (interface utilisateur graphique du gestionnaire du cluster de basculement), **RegisterAllProvidersIP** a la valeur 0 (False) par défaut.  
  
###  <a name="hostrecordttl-setting"></a><a name="HostRecordTTL"></a> Paramètre HostRecordTTL  
 Par défaut, le service DNS en cluster du cache des clients enregistre pendant 20 minutes.  En réduisant **HostRecordTTL**, autrement dit la durée de vie (TTL) de l’enregistrement mis en cache, les clients hérités peuvent se reconnecter plus rapidement.  Cependant, la réduction du paramètre **HostRecordTTL** peut également entraîner une augmentation du trafic vers les serveurs DNS.  
  
###  <a name="sample-powershell-script-to-disable-registerallprovidersip-and-reduce-ttl"></a><a name="SampleScript"></a> Exemple de script PowerShell pour désactiver RegisterAllProvidersIP et réduire TTL  
 L'exemple PowerShell suivant explique comment configurer les paramètres de cluster **RegisterAllProvidersIP** et **HostRecordTTL** pour la ressource d'écouteur.  L'enregistrement DNS sera mis en cache pendant 5 minutes plutôt que pendant 20 minutes (valeur par défaut).  La modification des deux paramètres de cluster peut réduire le temps de connexion à l'adresse IP correcte après un basculement pour les clients hérités qui ne peuvent pas utiliser le paramètre **MultiSubnetFailover** .  Remplacez `yourListenerName` par le nom de l'écouteur que vous modifiez.  
  
```  
Import-Module FailoverClusters  
Get-ClusterResource yourListenerName | Set-ClusterParameter RegisterAllProvidersIP 0   
Get-ClusterResource yourListenerName | Set-ClusterParameter HostRecordTTL 300  
Stop-ClusterResource yourListenerName  
Start-ClusterResource yourListenerName  
Start-Clustergroup yourListenerGroupName
```  
  
 Pour plus d'informations sur les temps de récupération au cours d'un basculement, consultez [Client Recovery Latency During Failover](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md#DNS).  
  
###  <a name="follow-up-recommendations"></a><a name="FollowUpRecommendations"></a> Recommandations de suivi  
 Après avoir créé un écouteur de groupe de disponibilité :  
  
-   Demandez à votre administrateur réseau de réserver l'adresse IP de l'écouteur pour son utilisation exclusive.  
  
-   Fournissez le nom d'hôte DNS de l'écouteur aux développeurs d'applications pour qu'ils l'utilisent dans les chaînes de connexion lorsqu'ils demandent des connexions clientes vers ce groupe de disponibilité.  
  
-   Encouragez les développeurs à mettre à jour les chaînes de connexion du client pour spécifier `MultiSubnetFailover = True`, si possible. Pour plus d’informations sur la prise en charge du pilote pour le basculement de plusieurs sous-réseaux, consultez [Connectivité client Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
###  <a name="create-an-additional-listener-for-an-availability-group-optional"></a><a name="CreateAdditionalListener"></a> Créer un écouteur supplémentaire pour un groupe de disponibilité (facultatif)  
 Après avoir créé un écouteur via SQL Server, vous pouvez ajouter un écouteur supplémentaire, comme suit :  
  
1.  Créez l'écouteur à l'aide de l'un des outils suivants :  
  
    -   **Utilisation du Gestionnaire de cluster de basculement WSFC :**  
  
        1.  Ajoutez un point d'accès client et configurez l'adresse IP.  
  
        2.  Mettez l'écouteur en ligne.  
  
        3.  Ajoutez une dépendance à la ressource du groupe de disponibilité WSFC.  
  
         Pour plus d’informations sur les boîtes de dialogue et les onglets du gestionnaire de cluster de basculement, consultez [Interface utilisateur : composant logiciel enfichable du gestionnaire de cluster de basculement](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772502(v=ws.11)).  
  
    -   **Utilisation de Windows PowerShell pour les clusters de basculement :**  
  
        1.  Utilisez [Add-ClusterResource](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee460983(v=technet.10)) pour créer les ressources de nom réseau et d’adresse IP.  
  
        2.  Utilisez [Start-ClusterResource](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461056(v=technet.10)) pour démarrer la ressource de nom réseau.  
  
        3.  Utilisez [Add-ClusterResourceDependency](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461014(v=technet.10)) pour définir la dépendance entre le nom réseau et la ressource existante du groupe de disponibilité SQL Server.  
  
         Pour plus d'informations sur l'utilisation de Windows PowerShell pour les clusters de basculement, consultez [Présentation des commandes du Gestionnaire de serveurs](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732757(v=ws.11)#BKMK_wps).  
  
2.  Démarrez l'écoute [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur le nouvel écouteur. Après avoir créé l'écouteur supplémentaire, connectez-vous à l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui héberge le réplica principal du groupe de disponibilité et utilisez [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou PowerShell pour modifier le port d'écoute.  
  
 Pour plus d’informations, consultez [Créer plusieurs écouteurs pour le même groupe de disponibilité](/archive/blogs/sqlalwayson/how-to-create-multiple-listeners-for-same-availability-group-goden-yao) (blog de l’équipe de SQL Server Always On).  
  
 
  
## <a name="next-steps"></a>Étapes suivantes

Maintenant que vous avez créé l’écouteur, configurez votre application pour la [connecter à l’écouteur](listeners-client-connectivity-application-failover.md). Vous pouvez également examiner différentes [stratégies de supervision des groupes de disponibilité](monitoring-of-availability-groups-sql-server.md) pour garantir l’intégrité de votre groupe de disponibilité.

Vous pouvez aussi [afficher les propriétés d’un écouteur](view-availability-group-listener-properties-sql-server.md) ou découvrir comment [supprimer l’écouteur](remove-an-availability-group-listener-sql-server.md), si nécessaire.