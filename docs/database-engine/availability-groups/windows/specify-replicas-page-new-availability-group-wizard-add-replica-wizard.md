---
title: 'Assistant Groupe de disponibilité : Page Spécifier les réplicas'
description: Décrit les options de la page Spécifier les réplicas de l’Assistant Nouveau groupe de disponibilité dans SSMS (SQL Server Management Studio).
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.newagwizard.listeners.f1
- sql13.swb.addreplicawizard.specifyreplicas.f1
- sql13.swb.newagwizard.specifyreplicas.f1
ms.assetid: 2d90fc12-a67b-4bd0-b0ab-899b73017196
author: cawrites
ms.author: chadam
ms.openlocfilehash: fc0fd2d74ceea77b0c577f7bf3315695385274eb
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97639197"
---
# <a name="specify-replicas-page-new-availability-group-wizard-add-replica-wizard"></a>Page Spécifier les réplicas (Assistant Nouveau groupe de disponibilité : Assistant Ajouter un réplica)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique décrit les options de la page **Spécifier les réplicas** . Cette page s'applique à l' **[!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)]** et à l' **[!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)]** . Utilisez la page **Spécifier les réplicas** pour spécifier et configurer un ou plusieurs réplicas de disponibilité afin d'ajouter le groupe de disponibilité. Cette page contient quatre onglets, qui sont présentés dans le tableau suivant. Cliquez sur le nom d'un onglet du tableau pour atteindre la section correspondante, plus loin dans cette rubrique.  
  
|Onglet|Brève description|  
|---------|-----------------------|  
|[Réplicas](#ReplicasTab)|Cet onglet vous permet de spécifier chaque instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui hébergera ou héberge actuellement un réplica secondaire. Notez que l'instance de serveur à laquelle vous êtes actuellement connecté doit héberger le réplica principal.<br /><br /> Terminez la spécification de tous les réplicas sur l’onglet **Réplicas** avant de passer aux autres onglets.<br/><br/> Notez que le **basculement automatique** est désactivé si le type de cluster est **NONE**. SQL Server prend en charge seulement le basculement manuel quand un groupe de disponibilité n’est pas dans un cluster. <br/><br/> Quand le type de cluster est EXTERNAL, le mode de basculement est **Externe**. <br/><br/> Quand vous ajoutez un réplica, tous les nouveaux réplicas doivent être hébergés sur le même type de système d’exploitation que les réplicas existants. <br/><br/>Quand vous ajoutez un réplica, si le réplica principal se trouve sur un cluster WSFC, les réplicas secondaires doivent être dans le même cluster.|
|[Points de terminaison](#EndpointsTab)|Utilisez cet onglet pour vérifier tous les points de terminaison de mise en miroir de bases de données existants et également, si ce point de terminaison manque sur une instance de serveur dont les comptes de service utilisent l'authentification Windows, pour créer le point de terminaison automatiquement.|  
|[Préférences de sauvegarde](#BackupPreferencesTab)|Utilisez cet onglet pour spécifier vos préférences de sauvegarde pour le groupe de disponibilité dans son ensemble, ainsi que les priorités de sauvegarde pour les différents réplicas de disponibilité.|  
|[Port d'écoute](#Listener)|Utilisez cet onglet, s'il est disponible, pour créer un écouteur de groupe de disponibilité. Par défaut, aucun écouteur n'est créé.<br /><br /> Cet onglet est disponible uniquement si vous exécutez l' [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)].<br/><br/>DHCP est désactivé quand le type de cluster est EXTERNAL ou NONE. |  
  
##  <a name="replicas-tab"></a><a name="ReplicasTab"></a> Onglet Réplicas  
 **Instance de serveur**  
 Affiche le nom de l'instance du serveur qui hébergera le réplica de disponibilité.  
  
 Si une instance de serveur que vous prévoyez d’utiliser pour héberger un réplica secondaire n’est pas listée dans la grille **Réplicas de disponibilité**, cliquez sur le bouton **Ajouter un réplica**. Si vous configurez un groupe de disponibilité dans un environnement informatique hybride (consultez [Haute disponibilité et récupération d’urgence pour SQL Server dans les machines virtuelles Azure](/previous-versions/azure/jj870962(v=azure.100))), cliquez sur le bouton **Ajouter un réplica Azure** pour créer des machines virtuelles avec des réplicas secondaires dans Azure.  
  
 **Rôle initial**  
 Indique le rôle que le nouveau réplica jouera initialement : **Principal** ou **Secondaire**.  
  
 **Basculement automatique (jusqu’à 3)**  
 Activez cette case à cocher uniquement si vous souhaitez que ce réplica de disponibilité soit un partenaire de basculement automatique. Pour configurer le basculement automatique, vous devez choisir cette option pour le réplica principal initial et pour un réplica secondaire. Les deux réplicas utilisent le mode de disponibilité avec validation synchrone. Seuls trois réplicas peuvent prendre en charge le basculement automatique.  
  
 Pour plus d’informations sur le mode de disponibilité avec validation synchrone, consultez [Modes de disponibilité &#40;Groupes de disponibilité AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md). Pour plus d’informations sur le basculement automatique, consultez [Basculement et modes de basculement &#40;Groupes de disponibilité AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
 **Validation synchrone (jusqu'à 3)**  
 Si vous avez sélectionné **Basculement automatique (jusqu’à 3)** pour le réplica, l’option **Validation asynchrone (jusqu’à 3)** est également sélectionnée. Si la case à cocher est vide, sélectionnez-la uniquement si vous souhaitez que ce réplica utilise le mode de validation synchrone avec le basculement manuel planifié uniquement. Seuls trois réplicas peuvent utiliser le mode de validation synchrone.  
  
 Si vous souhaitez que ce réplica utilise le mode de disponibilité avec validation asynchrone, n'activez pas cette case à cocher. Le réplica ne prendra en charge que le basculement manuel forcé (avec possible perte de données). Pour plus d’informations sur le mode de disponibilité avec validation asynchrone, consultez [Modes de disponibilité &#40;Groupes de disponibilité AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md). Pour plus d’informations sur le basculement manuel planifié et le basculement manuel forcé, consultez [Basculement et modes de basculement &#40;Groupes de disponibilité AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
 **Rôle secondaire accessible en lecture**  
 Sélectionnez une valeur dans la liste déroulante **Rôle secondaire accessible en lecture** , comme suit :  
  
 **Non**  
 Aucune connexion directe n'est autorisée aux bases de données secondaires de ce réplica. Elles ne sont pas disponibles pour l'accès en lecture. Il s'agit du paramètre par défaut.  
  
 **Tentative de lecture uniquement**  
 Seules les connexions en lecture seule directes sont autorisées aux bases de données secondaires de ce réplica. La ou les bases de données secondaires sont toutes disponibles pour l'accès en lecture.  
  
 **Oui**  
 Toutes les connexions sont autorisées aux bases de données secondaires de ce réplica, mais uniquement pour l'accès en lecture. La ou les bases de données secondaires sont toutes disponibles pour l'accès en lecture.  
  
 **Ajouter un réplica**  
 Cliquez pour ajouter un réplica secondaire au groupe de disponibilité.  
  
 **Ajouter un réplica Azure**  
 Cliquez pour créer une machine virtuelle Azure qui exécute un réplica secondaire dans le groupe de disponibilité. Cette option s’applique uniquement à un groupe de disponibilité dans un environnement hybride qui contient des réplicas locaux. Pour plus d’informations, consultez [Haute disponibilité et récupération d’urgence pour SQL Server dans les machines virtuelles Azure](/previous-versions/azure/jj870962(v=azure.100)).  
  
 **Supprimer le réplica**  
 Cliquez pour supprimer le réplica secondaire sélectionné du groupe de disponibilité.  
  
##  <a name="endpoints-tab"></a><a name="EndpointsTab"></a> Onglet Points de terminaison  
 Pour chaque instance de serveur qui hébergera un réplica de disponibilité, l'onglet **Points de terminaison** affiche des valeurs réelles du point de terminaison de mise en miroir de bases de données existant, le cas échéant, ou des valeurs suggérées pour un nouveau point de terminaison potentiel qui utiliserait l'authentification Windows. Pour les points de terminaison existant et potentiel, la grille des valeurs de point de terminaison affiche les informations suivantes :  
  
 **Nom de serveur**  
 Affiche le nom d'une instance de serveur qui hébergera un réplica de disponibilité.  
  
 **URL de point de terminaison**  
 Affiche l'URL réelle ou proposée du point de terminaison de mise en miroir de bases de données. Pour un nouveau point de terminaison proposé, vous pouvez modifier cette valeur. Pour plus d’informations sur le format de ces URL, consultez [Spécifier l’URL de point de terminaison lors de l’ajout ou lors de la modification d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md).  
  
 **Numéro de port**  
 Affiche le numéro de port réel ou proposé du point de terminaison. Pour un nouveau point de terminaison proposé, vous pouvez modifier cette valeur.  
  
 **Nom du point de terminaison**  
 Affiche le nom réel ou proposé du point de terminaison. Pour un nouveau point de terminaison proposé, vous pouvez modifier cette valeur.  
  
 **Chiffrer les données**  
 Indique si les données envoyées sur ce point de terminaison sont chiffrées. Pour un nouveau point de terminaison proposé, vous pouvez modifier ce paramètre.  
  
 **Compte de service SQL Server**  
 Nom d'utilisateur du compte de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 Pour qu'une instance de serveur utilise un point de terminaison qui utilise l'authentification Windows, son compte de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit être un compte de domaine.  
  
 Cette condition requise détermine votre étape de configuration suivante, comme suit :  
  
-   Si chaque instance de serveur s'exécute sous un compte de service de domaine, c'est-à-dire, si la colonne **Compte de service SQL Server** affiche un compte de service de domaine pour chaque instance de serveur, cliquez sur **Suivant**.  
  
-   Si une instance de serveur s'exécute sous un compte de service qui n'appartient pas au domaine, vous devez apporter une modification manuelle à votre instance de serveur avant de pouvoir continuer dans l'Assistant. Dans ce cas, si vous cliquez sur **Suivant** , une boîte de dialogue d'avertissement s'affiche ; vous devez cliquer sur **Non**, ce qui vous renvoie à l'onglet **Points de terminaison** . En laissant l'Assistant sur la page **Spécifier les réplicas** , apportez l'une des modifications suivantes à chaque instance de serveur pour laquelle la colonne **Compte de service SQL Server** affiche un compte de service qui n'appartient pas au domaine :  
  
    -   Utilisez le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour changer le compte de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en compte de domaine. Pour plus d’informations, consultez [Modifier le compte de démarrage du service pour SQL Server &#40;Gestionnaire de configuration SQL Server&#41;](../../../database-engine/configure-windows/scm-services-change-the-service-startup-account.md).  
  
    -   Utilisez [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou PowerShell pour créer manuellement un point de terminaison de mise en miroir de bases de données qui utilise un certificat. Pour plus d'informations, consultez [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-endpoint-transact-sql.md) ou [Créer un point de terminaison de mise en miroir de bases de données pour les groupes de disponibilité Always On &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md).  
  
     Si vous laissez la page **Spécifier les réplicas de disponibilité** ouverte lorsque vous configurez des points de terminaison, retournez à l'onglet **Points de terminaison** et cliquez sur **Actualiser** pour mettre à jour la grille **Valeurs de point de terminaison** .  
  
##  <a name="backup-preferences-tab"></a><a name="BackupPreferencesTab"></a> Onglet Préférences de sauvegarde  
 Pour spécifier l'emplacement des sauvegardes, choisissez l'une des options suivantes :  
  
 **Préférer secondaire**  
 Spécifie que les sauvegardes doivent se produire sur un réplica secondaire sauf lorsque le réplica principal est le seul réplica en ligne. Dans ce cas, la sauvegarde doit se produire sur le réplica principal. Il s'agit de l'option par défaut.  
  
 **Secondaire uniquement**  
 Spécifie que les sauvegardes ne doivent jamais être effectuées sur le réplica principal. Si le réplica principal est le seul réplica en ligne, la sauvegarde ne doit pas avoir lieu.  
  
 **Primaire**  
 Spécifie que les sauvegardes doivent toujours avoir lieu sur le réplica principal. Cette option est utile si vous avez besoin de fonctionnalités de sauvegarde, telles que la création de sauvegardes différentielles, qui ne sont pas prises en charge lorsque la sauvegarde est exécutée sur un réplica secondaire.  
  
 **Tout réplica**  
 Spécifie que vous préférez que les travaux de sauvegarde ignorent le rôle des réplicas de disponibilité lorsque vous choisissez le réplica pour effectuer les sauvegardes. Notez que les travaux de sauvegarde peuvent évaluer d'autres facteurs tels que la priorité de sauvegarde de chaque réplica de disponibilité en association avec son état opérationnel et son état connecté.  
  
> [!IMPORTANT]  
>  Il n'y a aucune contrainte du paramètre de préférence de sauvegarde. La traduction de cette préférence dépend de la logique, le cas échéant, que vous avez écrite dans les travaux de sauvegarde pour les bases de données dans un groupe de disponibilité donné. Pour plus d’informations, consultez [Secondaires actifs : Sauvegarde sur des réplicas secondaires &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).  
  
### <a name="replica-backup-priorities-grid"></a>Grille des priorités de sauvegarde de réplica  
 Utilisez la grille **Priorités de sauvegarde de réplica** pour spécifier les priorités de sauvegarde pour chacun des réplicas du groupe de disponibilité. Cette grille comporte les colonnes suivantes :  
  
 **Instance de serveur**  
 Affiche le nom de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui héberge le réplica de disponibilité.  
  
 **Priorité de sauvegarde (Minimale=1, Maximale=100)**  
 Attribuez la priorité d'exécution des sauvegardes sur ce réplica par rapport aux autres réplicas dans le même groupe de disponibilité. La valeur par défaut est 50. Vous pouvez sélectionner n'importe quel autre entier dans la plage 0..100. 1 indique la priorité la plus faible, 100 la priorité la plus élevée. Si vous avez défini **Priorité de sauvegarde** sur 1, le réplica de disponibilité est choisi pour l’exécution des sauvegardes uniquement si aucun réplica de disponibilité ayant une priorité plus élevée n’est actuellement disponible.  
  
 **Exclure le réplica**  
 Pour que ce réplica de disponibilité ne soit jamais choisi pour effectuer des sauvegardes. Cela est utile, par exemple, pour un réplica de disponibilité distant sur lequel vous ne souhaitez jamais basculer de sauvegardes.  
  
##  <a name="listener-tab"></a><a name="Listener"></a> Onglet Écouteur  
 Spécifiez votre préférence pour un[écouteur de groupe de disponibilité](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)qui fournira un point de connexion du client, à savoir :  
  
 **Ne créez pas d'écouteur de groupe de disponibilité maintenant.**  
 Sélectionnez cette option pour ignorer cette étape. Vous pouvez créer un écouteur ultérieurement. Pour plus d'informations, consultez [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
 **Créez un écouteur de groupe de disponibilité.**  
 Spécifiez vos préférences d'écouteur pour ce groupe de disponibilité, comme suit :  
  
 **Nom DNS de l'écouteur**  
 Indiquez le nom réseau de l'écouteur. Ce nom doit être unique sur le domaine et ne peut contenir que des caractères alphanumériques, des tirets ( **-** ) et des traits d’union ( **_** ), dans n’importe quel ordre. Lorsqu'il est spécifié à l'aide de l'onglet **Écouteur** , le nom DNS peut contenir jusqu'à 15 caractères.  
  
> [!IMPORTANT]  
>  Si vous entrez un nom d’écouteur DNS (ou un numéro de port) non valide dans l’onglet **Écouteur** , le bouton **Suivant** est désactivé dans la page **Spécifier les réplicas** .  
  
 **Port**  
 Spécifiez le port TPC utilisé par cet écouteur.  
  
> [!NOTE]  
>  Si vous entrez un numéro de port (ou un nom d’écouteur DNS) non valide dans l’onglet **Écouteur** , le bouton **Suivant** est désactivé dans la page **Spécifier les réplicas** .  
  
 **Mode réseau**  
 Utilisez la liste déroulante pour sélectionner le mode réseau à utiliser par cet écouteur, à savoir :  
  
 **Adresse IP statique**  
 Indiquez si vous souhaitez que l'écouteur écoute sur plusieurs sous-réseaux. Pour utiliser le mode réseau IP statique, un écouteur de groupe de disponibilité doit écouter sur chaque sous-réseau qui héberge un réplica de disponibilité pour le groupe de disponibilité. Pour chaque sous-réseau, cliquez sur **Ajouter** pour sélectionner une adresse de sous-réseau et spécifier une adresse IP.  
  
 Si l’option **Adresse IP statique** est sélectionnée comme mode réseau (il s’agit de la sélection par défaut), une grille affiche les colonnes **Sous-réseau** et **Adresse IP** , et les boutons associés **Ajouter** et **Supprimer** sont affichés. La grille est vide tant que vous n’avez pas ajouté le premier sous-réseau.  
  
 Colonne **Sous-réseau**  
 Affiche l'adresse de sous-réseau que vous avez sélectionnée pour chaque sous-réseau ajouté pour l'écouteur.  
  
 Colonne **Adresse IP**  
 Affiche l'adresse IPv4 ou IPv6 que vous avez spécifiée pour un sous-réseau donné.  
  
 **Ajouter**  
 Cliquez pour ajouter un sous-réseau à cet écouteur. Cela ouvre la boîte de dialogue **Ajouter une adresse IP** . Pour plus d’informations, consultez la rubrique d’aide [Boîte de dialogue Ajouter une adresse IP &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/add-ip-address-dialog-box-sql-server-management-studio.md).  
  
 **Remove**  
 Cliquez pour supprimer le sous-réseau actuellement sélectionné dans la grille.  
  
 **DHCP**  
 Indiquez si vous souhaitez que l'écouteur écoute sur un seul sous-réseau et utilise une adresse IPv4 dynamique affectée par un serveur exécutant le protocole DHCP (Dynamic Host Configuration Protocol). DHCP est limité à un seul sous-réseau commun à chaque instance de serveur qui héberge un réplica de disponibilité pour le groupe de disponibilité. DHCP n’est pas disponible pour le type de cluster EXTERNAL ou NONE.  
  
> [!IMPORTANT]  
>  Nous vous déconseillons d'utiliser DHCP dans un environnement de production. Lorsqu'un temps mort se produit et que le bail IP DHCP arrive à expiration, une période de temps supplémentaire est requise pour enregistrer la nouvelle adresse IP de réseau DHCP associée au nom DNS de l'écouteur et cela a un impact sur la connectivité client. Toutefois, DHCP peut tout à fait être utilisé pour configurer un environnement de développement et de test dans le but de vérifier les fonctions de base des groupes de disponibilité et l'intégration avec vos applications.  
  
 Lorsque **DHCP** est sélectionné, le champ **Sous-réseau** est affiché.  
  
 **Sous-réseau**  
 Si vous avez sélectionné **DHCP** comme mode réseau, utilisez la liste déroulante **Sous-réseau** pour sélectionner une adresse du sous-réseau qui héberge les réplicas de disponibilité du groupe de disponibilité.  
  
> [!IMPORTANT]
>  Après avoir défini un écouteur du groupe de disponibilité, nous vous recommandons fortement de procéder comme suit :  
> 
>  -   Demandez à votre administrateur réseau de réserver l'adresse IP de l'écouteur pour son utilisation exclusive. Fournissez le nom d'hôte DNS de l'écouteur aux développeurs d'applications pour qu'ils l'utilisent dans les chaînes de connexion lorsqu'ils demandent des connexions clientes vers ce groupe de disponibilité.  
> -   Fournissez le nom d'hôte DNS de l'écouteur aux développeurs d'applications pour qu'ils l'utilisent dans les chaînes de connexion lorsqu'ils demandent des connexions clientes vers ce groupe de disponibilité.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Utiliser l’Assistant Groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Utiliser l’Assistant Ajouter un réplica au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Utiliser des certificats pour un point de terminaison de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-endpoint-transact-sql.md)  
  
-   [Créer un point de terminaison de mise en miroir de bases de données pour les groupes de disponibilité Always On &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md)   
 [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
