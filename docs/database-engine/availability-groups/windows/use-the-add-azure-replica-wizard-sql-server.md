---
title: Configurer une machine virtuelle Azure comme réplica secondaire dans un groupe de disponibilité
description: Utilisez l’Assistant Ajout d’un réplica Azure pour créer une machine virtuelle Azure dans un environnement informatique hybride et la configurer comme réplica secondaire pour un groupe de disponibilité Always On nouveau ou existant.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.addreplicawizard.azurereplica.f1
ms.assetid: b89cc41b-07b4-49f3-82cc-bc42b2e793ae
author: cawrites
ms.author: chadam
ms.openlocfilehash: 91f906b996c44cd9798d9754ca0345ca8892db10
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076361"
---
# <a name="configure-azure-vm-as-a-secondary-replica-in-an-availability-group"></a>Configurer une machine virtuelle Azure comme réplica secondaire dans un groupe de disponibilité
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Utilisez l’Assistant Ajout d’un réplica Azure pour vous aider à créer une machine virtuelle Azure dans un environnement informatique hybride et à la configurer comme réplica secondaire pour un groupe de disponibilité Always On nouveau ou existant.  

>  [!IMPORTANT]  
>  Azure dispose de deux modèles de déploiement différents pour créer et utiliser des ressources : Resource Manager et classique. Cet article traite du modèle de déploiement classique. Pour la plupart des nouveaux déploiements, Microsoft recommande d’utiliser le modèle Resource Manager. La procédure donnée dans cet article ne s’applique en cas de déploiement de la machine virtuelle Azure suivant le modèle Resource Manager.   

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
 Si vous n’avez jamais ajouté de réplica de disponibilité à un groupe de disponibilité, consultez les sections « Instances de serveur » et « Groupes de disponibilité et réplicas » dans [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Vous devez être connecté à l'instance de serveur qui héberge le réplica principal actuel.  
  
-   Vous devez travailler dans un environnement informatique hybride où votre sous-réseau local dispose d’un réseau privé virtuel (VPN) de site à site avec Azure. Pour plus d’informations, consultez [Configurer un réseau privé virtuel (VPN) de site à site dans le portail de gestion](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal).  
  
-   Votre groupe de disponibilité doit contenir des réplicas de disponibilité locaux.  
  
-   Les clients de l’écouteur de groupe de disponibilité doivent disposer d’une connectivité à Internet s’ils souhaitent maintenir la connectivité avec l’écouteur lorsque le groupe de disponibilité est basculé vers un réplica Azure.  
  
-   **Conditions préalables requises pour utiliser la synchronisation de données initiale complète** Vous devez spécifier un partage réseau pour que l'assistant crée des sauvegardes et puisse y accéder. Pour le réplica principal, le compte utilisé pour démarrer le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] doit disposer d'autorisations de système de fichiers en lecture et en écriture sur un partage réseau. Pour les réplicas secondaires, le compte doit disposer d'une autorisation en lecture sur le partage réseau.  
  
     Si vous ne pouvez pas utiliser l'Assistant pour effectuer la synchronisation des données initiale complète, vous devez préparer vos bases de données secondaires manuellement. Vous pouvez le faire avant ou après l'exécution de l'Assistant. Pour plus d’informations, consultez [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  
  
 Nécessite également l'autorisation CONTROL ON ENDPOINT si vous souhaitez autoriser l'Assistant Ajouter un réplica au groupe de disponibilité à gérer le point de terminaison de mise en miroir de bases de données.  
  
##  <a name="using-the-add-azure-replica-wizard-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de l'Assistant Ajouter un réplica Windows Azure (SQL Server Management Studio)  

>  [!IMPORTANT]  
>  L’Assistant Ajout d’un réplica Azure prend en charge seulement les machines virtuelles créées avec le modèle de déploiement classique. Les nouveaux déploiements de machines virtuelles doivent suivre le modèle Resource Manager, plus récent. Si vous utilisez des machines virtuelles avec Resource Manager, vous devez ajouter manuellement le réplica Azure secondaire à l’aide de commandes Transact-SQL (non indiquées ici). Cet Assistant ne fonctionne pas dans le scénario Resource Manager. 
>
>  L’Assistant Ajout d’un réplica Azure n’est pas disponible dans les dernières versions (18.x et 17.x) de SQL Server Management Studio.
        
 L'Assistant Ajouter un réplica Windows Azure peut être lancé depuis la page [Spécifier les réplicas](../../../database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard.md). Il existe deux moyens d'atteindre cette page :  
  
-   [Utiliser l’Assistant Groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Utiliser l’Assistant Ajouter un réplica au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
 Une fois que vous avez lancé l'Assistant Ajouter un réplica Windows Azure, suivez les étapes ci-dessous :  
  
1.  Commencez par télécharger un certificat de gestion pour votre abonnement Azure. Cliquez sur **Télécharger** pour ouvrir la page de connexion.  
  
2.  Connectez-vous à Microsoft Azure avec votre compte Microsoft ou votre compte professionnel ou scolaire. Votre compte Microsoft ou votre compte d’organisation  a le format d’une adresse e-mail, comme HYPERLINK "mailto:patc@contoso.com" patc@contoso.com. Pour en savoir plus sur les informations d’identification Azure, consultez les rubriques suivantes : [Microsoft Account for Organizations FAQ (FAQ sur les comptes Microsoft pour les organisations)](/previous-versions/jj592903(v=msdn.10)) et [Résolution des problèmes de connexion avec votre compte d’organisation](http://web.archive.org/web/20121016005434/http://support.microsoft.com:80/kb/2756852).  
  
3.  Connectez-vous ensuite à votre abonnement en cliquant sur **Connexion**. Une fois que vous êtes connecté, les listes déroulantes sont remplies avec vos paramètres Azure, tels que **Réseau virtuel** et **Sous-réseau de réseau virtuel**.  
  
4.  Spécifiez les paramètres de la machine virtuelle Azure qui hébergera le nouveau réplica secondaire :  
  
     Image  
     Nom de l’image SQL Server à utiliser pour la machine virtuelle Azure  
  
     Taille de la machine virtuelle  
     Taille de la machine virtuelle Azure  
  
     Nom de la machine virtuelle  
     Nom DNS de la machine virtuelle Azure  
  
     Nom d'utilisateur de l'ordinateur virtuel  
     Nom d’utilisateur de l’administrateur par défaut de la machine virtuelle Azure  
  
     Mot de passe d'administrateur de l'ordinateur virtuel (et Confirmer le mot de passe)  
     Mot de passe de l’administrateur par défaut de la machine virtuelle Azure  
  
     Réseau virtuel  
     Réseau virtuel dans lequel placer la machine virtuelle Azure  
  
     Sous-réseau de réseau virtuel  
     Sous-réseau de réseau virtuel dans lequel placer la machine virtuelle Azure  
  
     Domain  
     Domaine Active Directory (AD) auquel joindre la machine virtuelle Azure  
  
     Domain Username  
     Nom d’utilisateur AD utilisé pour joindre la machine virtuelle Azure au domaine  
  
     Mot de passe  
     Mot de passe utilisé pour joindre la machine virtuelle Azure au domaine  
  
5.  Cliquez sur **OK** pour valider vos paramètres et quitter l'Assistant Ajouter un réplica Windows Azure.  
  
6.  Effectuez les autres étapes de configuration décrites à la page [Spécifier les réplicas](../../../database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard.md) , comme vous le feriez pour tout nouveau réplica.  
  
     Une fois que vous avez terminé avec l’Assistant Groupe de disponibilité ou l’Assistant Ajout d’un réplica au groupe de disponibilité, le processus de configuration effectue toutes les opérations dans Azure afin de créer la machine virtuelle, de la joindre au domaine AD, de l’ajouter au cluster Windows, d’activer la haute disponibilité Always On et d’ajouter le nouveau réplica au groupe de disponibilité.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Ajouter un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Ajouter un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
