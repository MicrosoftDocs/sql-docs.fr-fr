---
title: Vue d’ensemble du moniteur de mise en miroir de bases de données | Microsoft Docs
description: Découvrez comment utiliser le Moniteur de mise en miroir de bases de données pour surveiller les bases de données mises en miroir afin de vérifier le flux de données dans la session de mise en miroir de bases de données et résoudre les problèmes.
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.main.f1
helpviewer_keywords:
- Database Mirroring Monitor [SQL Server], interface
ms.assetid: 8ebbdcd6-565a-498f-b674-289c84b985eb
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: a5c0a12ba95a6f0248ac6ba0c073e0d5428205f7
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642032"
---
# <a name="database-mirroring-monitor-overview"></a>Vue d'ensemble du moniteur de mise en miroir de bases de données
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Si vous disposez des autorisations appropriées, vous pouvez utiliser le moniteur de mise en miroir de bases de données pour surveiller un sous-ensemble quelconque des bases de données mises en miroir sur une instance de serveur. La surveillance permet de vérifier si et comment les données circulent dans la session de mise en miroir de bases de données. Le moniteur de mise en miroir de bases de données permet de déterminer la cause d'une réduction du flux de données.  
  
 Vous pouvez inscrire vos bases de données mises en miroir pour une surveillance individuelle sur chaque partenaire de basculement. Lorsque vous inscrivez une base de données, le moniteur de mise en miroir de bases de données met en cache les informations suivantes relatives à la base de données :  
  
-   Nom de la base de données  
  
-   les noms des instances de serveur des deux partenaires ;  
  
-   les derniers rôles connus de chaque partenaire (principal ou miroir).  
  
## <a name="permissions"></a>Autorisations  
 Pour surveiller la mise en miroir de bases de données, vous devez être membre du rôle de serveur fixe **sysadmin** ou membre du rôle de base de données fixe **dbm_monitor** dans la base de données **msdb** sur l’instance de serveur. Si vous êtes membre de **sysadmin** ou **dbm_monitor** sur une seule des instances de serveur partenaire, le moniteur peut se connecter uniquement à ce partenaire ; il ne peut pas extraire d’informations de l’autre partenaire.  
  
 Si vous êtes uniquement membre de **dbm_monitor** sur une instance de serveur, vous disposez d’autorisations limitées sur cette instance de serveur. Vous pourrez afficher uniquement la ligne du dernier état. Si vous vous connectez à une instance de serveur en utilisant les autorisations **dbm_monitor** , le moniteur de mise en miroir de bases de données vous informe que vous disposez d’autorisations limitées.  
  
> [!IMPORTANT]  
>  Le rôle de base de données fixe **dbm_monitor** est créé dans la base de données **msdb** lorsque la première base de données est inscrite dans le moniteur de mise en miroir de bases de données. Le nouveau rôle **dbm_monitor** ne comporte aucun membre tant qu’un administrateur système n’affecte pas d’utilisateurs au rôle.  
  
## <a name="navigation-tree"></a>Arborescence de navigation  
 Si une base de données a été inscrite pour la surveillance par le moniteur de mise en miroir de bases de données, une liste des bases de données inscrites s'affiche dans l'arborescence de navigation. L'arborescence est actualisée automatiquement toutes les 30 secondes. Pour afficher l'état d'une base de données inscrite, sélectionnez-la. Pour plus d'informations, consultez « Volet d'informations » plus loin dans cette rubrique.  
  
 Pour chaque base de données inscrite, les informations suivantes s'affichent :  
  
 _<nom_base_de_données>_ **(** _\<Status>_ **,** _<SERVEUR_PRINCIPAL>_ **->** _<SERVEUR_MIROIR>_ **)**  
  
 *<nom_base_de_données>*  
 Nom d'une base de données mise en miroir inscrite auprès du moniteur de mise en miroir de bases de données.  
  
 *\<Status>*  
 Les états possibles et les icônes associées sont les suivants :  
  
|Icône|Statut|Description|  
|----------|------------|-----------------|  
|Icône d'avertissement|**Unknown**|Le moniteur n'est connecté à aucun partenaire. Les seules informations disponibles sont celles qui ont été mises en cache par le moniteur.|  
|Icône d'avertissement|**Synchronisation**|Le contenu de la base de données en miroir est décalé par rapport à celui de la base de données principale. L'instance de serveur principal envoie des enregistrements de journal à l'instance de serveur miroir, laquelle applique les modifications à la base de données miroir pour la restaurer par progression.<br /><br /> Lors du démarrage d'une session de mise en miroir de bases de données, les bases de données miroir et principale se trouvent dans cet état.|  
|Cylindre de base de données standard|**Synchronisée**|Lorsque le serveur miroir a rattrapé suffisamment de retard par rapport au serveur principal, l'état de la base de données devient **Synchronisé**. La base de données reste dans cet état aussi longtemps que le serveur principal continue d'envoyer des modifications au serveur miroir et que le serveur miroir continue d'appliquer les modifications à la base de données miroir.<br /><br /> En mode haute sécurité, les deux méthodes de basculement (automatique et manuel) sont possibles, sans perte de données.<br /><br /> En mode haute performance, la perte de données peut se produire, même si l’état est **Synchronisé** .|  
|Icône d'avertissement|**Suspendu**|La base de données principale est disponible mais n'envoie pas de journaux au serveur miroir.|  
|Icône d'erreur|**Déconnecté**|L'instance de serveur ne peut pas se connecter à son partenaire.|  
  
 *<PRINCIPAL_SERVER>*  
 Nom du partenaire qui est actuellement l'instance de serveur principal. Ce nom respecte le format suivant :  
  
 *<NOM_SYSTÈME>* [ **\\** _<nom_instance>_ ]  
  
 où *<NOM_SYSTÈME>* est le nom du système sur lequel l’instance de serveur réside. Pour une instance de serveur autre que l’instance par défaut, le nom de l’instance s’affiche également : _<NOM_SYSTÈME>_ **\\** _<nom_instance>_ .  
  
 *<MIRROR_SERVER>*  
 Nom du partenaire qui est actuellement l'instance de serveur miroir. Le format est identique à celui du serveur principal.  
  
## <a name="detail-pane"></a>Volet d'informations  
 L'apparence du moniteur varie selon qu'une base de données est sélectionnée ou non. Lorsque vous ouvrez le moniteur, le volet d’informations affiche un lien **Inscrire la base de données mise en miroir** . Cliquez sur ce lien pour inscrire une base de données. Les bases de données inscrites s’affichent sous le nœud **Moniteur de mise en miroir de bases de données** dans l’arborescence de navigation. Le moniteur de mise en miroir de bases de données tente toujours de se connecter à chaque instance de serveur pour laquelle il possède des informations d'identification stockées.  
  
 Quand vous sélectionnez une base de données, son état s’affiche dans la page à onglets **État** dans le volet d’informations. Le contenu de cette page provient des instances du principal et du serveur miroir. La page est remplie de manière asynchrone au fur et à mesure que l'état est recueilli par le biais de connexions distinctes aux instances du principal et du serveur miroir. L'état est actualisé automatiquement toutes les 30 secondes.  
  
> [!NOTE]  
>  Vous ne pouvez pas modifier la fréquence d’actualisation du moniteur, mais vous pouvez actualiser la table d’état à partir de la boîte de dialogue **Historique de la mise en miroir de bases de données** .  
  
 Un administrateur système peut afficher la configuration actuelle des avertissements pour la base de données en sélectionnant la page à onglets **Avertissements** . À partir de là, l’administrateur peut lancer la boîte de dialogue **Définir les seuils d’avertissement** pour activer et configurer un ou plusieurs seuils d’avertissement.  
  
 Dans la bannière située au-dessus des onglets, le volet d’informations affiche l’heure de la dernière actualisation des informations d’état par le moniteur dans le champ **Dernière actualisation :** _\<date>\<time>_ . En général, le moniteur de mise en miroir de bases de données extrait des informations d'état des instances de principal et de serveur miroir à des moments différents. L'heure d'actualisation la plus ancienne s'affiche.  
  
## <a name="action-menu"></a>Menu Action  
 Le menu **Action** comprend toujours les commandes suivantes :  
  
|Commande|Description|  
|-------------|-----------------|  
|**Inscrire la base de données mise en miroir**|Ouvre la boîte de dialogue **Inscrire la base de données mise en miroir** . Utilisez cette boîte de dialogue pour inscrire une ou plusieurs bases de données mises en miroir sur une instance de serveur donnée en ajoutant la ou les bases de données au moniteur de mise en miroir de bases de données. Lorsqu'une base de données est ajoutée, le moniteur de mise en miroir de bases de données met en cache localement des informations sur la base de données, ses partenaires et la connexion aux partenaires.|  
|**Gérer les connexions des instances serveur...**|Lorsque vous sélectionnez cette commande, la boîte de dialogue **Gérer les connexions serveur** s’ouvre. Vous pouvez alors choisir une instance de serveur pour laquelle vous souhaitez spécifier des informations d'identification qui seront utilisées par le moniteur lors de la connexion à un partenaire donné.<br /><br /> Pour modifier les informations d’identification pour un partenaire, recherchez l’entrée correspondante dans la grille **Instances de serveur** et cliquez sur **Modifier** dans cette ligne. La boîte de dialogue **Se connecter au serveur** s’ouvre avec le nom d’instance de serveur fixe, les contrôles d’informations d’identification étant initialisés sur la valeur actuelle mise en cache. Modifiez les informations d’authentification comme souhaité et cliquez sur **Se connecter**. Si les informations d’identification disposent des privilèges suffisants, la colonne **Se connecter avec** est mise à jour avec les nouvelles informations d’identification.|  
  
 Si vous sélectionnez une base de données, le menu **Action** comprend également les commandes suivantes.  
  
|Commande|Description|  
|-------------|-----------------|  
|**Annuler l'inscription de cette base de données**|Supprime la base de données sélectionnée du moniteur de mise en miroir de bases de données.|  
|**Définir les seuils d’avertissement...**|Ouvre la boîte de dialogue **Définir les seuils d’avertissement** . Un administrateur système peut activer ou désactiver des avertissements pour la base de données sur chaque partenaire et modifier le seuil de chaque avertissement. Nous vous recommandons de définir un seuil pour un avertissement spécifique sur les deux partenaires, afin de vous assurer que l'avertissement persiste lors du basculement de la base de données. Le seuil approprié pour chaque partenaire dépend des capacités de performance du système du partenaire.<br /><br /> Un événement est écrit dans le journal des événements pour une performance uniquement si sa valeur est identique ou supérieure au seuil au moment de la mise à jour de la table d'état. Si une valeur maximale atteint momentanément le seuil entre des mises à jour d'état, cette valeur est ignorée.|  
  
 **Pour contrôler la mise en miroir de bases de données à l'aide de SQL Server Management Studio**  
  
-   [Démarrer le moniteur de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Surveillance de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Démarrer l’Assistant Configuration de la sécurité de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
