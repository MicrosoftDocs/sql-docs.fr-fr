---
title: Moniteur de mise en miroir de bases de données (Page État) | Microsoft Docs
description: Cette page en lecture seule du Moniteur de mise en miroir de bases de données vous permet d’afficher l’état de la mise en miroir pour les instances de serveur principal et miroir d’une base de données sélectionnée.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.status.f1
ms.assetid: 4f64b4e1-89e9-4827-98fa-b92c3dc73b48
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 2d912623497cc6dc0baac1c4fbb4c8e794363ce8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353109"
---
# <a name="database-mirroring-monitor-status-page"></a>Moniteur de mise en miroir de bases de données (Page État)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette page accessible en lecture seule affiche le dernier état de mise en miroir pour les instances de serveur miroir et de principal de la base de données actuellement sélectionnée dans l'arborescence de navigation. Si les informations d'une instance ne sont pas actuellement disponibles, certaines des cellules de la grille **État** correspondant à cette instance sont grisées et affichent le texte **Inconnu**.  
  
 **Pour utiliser SQL Server Management Studio pour contrôler la mise en miroir de base de données**  
  
-   [Démarrer le moniteur de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="options"></a>Options  
 **État**  
 Affiche une grille présentant le dernier état de la mise en miroir à un haut niveau pour chaque instance de principal et de serveur miroir. Les lignes de la grille **État** s'affichent dans l'ordre suivant :  
  
-   Instance de serveur principal  
  
-   Instance de serveur miroir  
  
 Les colonnes sont les suivantes :  
  
|Nom de la colonne|Description|  
|-----------------|-----------------|  
|**Instance de serveur**|Nom de l'instance de serveur dont l'état est affiché dans la ligne **État** .|  
|**Rôle actuel**|Rôle actuel de l'instance de serveur : **Principal** ou **Miroir**.|  
|**État de la mise en miroir**|État de la mise en miroir reportée par l'instance de serveur et icône indiquant la gravité de l'état. Les états possibles et les icônes associées sont les suivants :<br /><br /> Icône : -, état **Inconnu**. Le moniteur n'est connecté à aucun partenaire. Les seules informations disponibles sont celles qui ont été mises en cache par le moniteur.<br /><br /> Icône : icône d’avertissement, état **Synchronisation**. Le contenu de la base de données en miroir est décalé par rapport à celui de la base de données principale. L'instance de serveur principal envoie des enregistrements de journal à l'instance de serveur miroir, laquelle applique les modifications à la base de données miroir pour la restaurer par progression. Lors du démarrage d'une session de mise en miroir de bases de données, les bases de données miroir et principale se trouvent dans cet état.<br /><br /> Icône : cylindre de base de données standard, état **Synchronisé**. Lorsque le serveur miroir a rattrapé suffisamment de retard par rapport au serveur principal, l'état de la base de données devient **Synchronisé**. La base de données reste dans cet état aussi longtemps que le serveur principal envoie des modifications au serveur miroir et que le serveur miroir applique les modifications à la base de données miroir.  En mode haute sécurité, les deux méthodes de basculement (automatique et manuel) sont possibles, sans perte de données.  En mode haute performance, la perte de données peut se produire, même si l’état est **Synchronisé** .<br /><br /> Icône : icône d’avertissement, état **Suspendu**. <br />                            La base de données principale est disponible mais n'envoie pas de journaux au serveur miroir.<br /><br /> Icône : icône d’erreur, état **Déconnecté**. L'instance de serveur ne peut pas se connecter à son partenaire.|  
|**Connexion témoin**|État de la connexion du témoin, précédé d'une icône d'état **Inconnu**, **Connecté** ou **Déconnecté**.|  
|**History**|Cliquez sur cette colonne pour afficher l'historique de mise en miroir sur l'instance de serveur. La boîte de dialogue **Historique de la mise en miroir de bases de données** s'ouvre, ce qui affiche l'historique d'état de la mise en miroir ainsi que des statistiques pour une base de données mise en miroir sur une instance de serveur donnée.<br /><br /> Le bouton **Historique** est grisé si le moniteur n'est pas connecté à l'instance de serveur.|  
  
 **Journal principal (** *\<time>* **)**  
 État du journal sur l’instance de serveur principal à l’heure locale de l’instance de serveur, indiquée par *\<time>* . Les paramètres suivants s'affichent :  
  
 **Journal non envoyé**  
 Quantité de journal en attente dans la file d'attente d'envoi, en Ko.  
  
 **Transaction non envoyée la plus ancienne**  
 Âge de la transaction non envoyée la plus ancienne dans la file d'attente d'envoi. La durée de vie de cette transaction indique la quantité de transactions en minutes qui n'ont pas encore été envoyées à l'instance de serveur miroir. Cette valeur permet de mesurer la perte de données potentielle en termes de temps.  
  
 **Durée (estimée) d'envoi du journal**  
 Durée approximative nécessaire à l’instance de serveur principal pour envoyer le journal qui se trouve actuellement dans la file d’attente d’envoi vers l’instance de serveur miroir ( *taux d’envoi*). Étant donné que le taux de transactions entrantes peut varier sensiblement, la durée d'envoi du journal est une estimation. Cependant, le taux d'envoi peut être utile pour obtenir une estimation approximative de la durée requise pour effectuer un basculement manuel.  
  
 **Taux d'envoi actuel**  
 Taux d'envoi des transactions à l'instance du serveur miroir, en Ko par seconde.  
  
 **Taux actuel de nouvelles transactions**  
 Débit auquel les transactions entrantes sont consignées dans le journal du principal en Kilo-octets par seconde. Pour déterminer si la mise en miroir prend du retard, reste à jour ou rattrape le retard, comparez cette valeur à la valeur **Durée (estimée) d'envoi du journal** .  
  
 **Journal miroir (** *\<time>* **)**  
 État du journal sur l’instance de serveur miroir à l’heure locale de l’instance de serveur, indiquée par *\<time>* . Les paramètres suivants s'affichent :  
  
 **Journal non restauré**  
 Quantité de journal en attente dans la file d'attente de restauration par progression, en Ko.  
  
 **Durée (estimée) de restauration du journal**  
 Nombre approximatif de minutes requises pour que le journal actuellement dans la file d'attente de restauration par progression soit appliqué à la base de données miroir.  
  
 **Taux de restauration actuel**  
 Débit auquel les transactions sont restaurées dans la base de données miroir (en Kilo-octets par seconde).  
  
 **Temps de traitement de validation de miroir**  
 Délai moyen en millisecondes par transaction toléré avant qu'un avertissement ne soit généré sur le serveur principal. Ce délai correspond au temps de traitement pendant lequel l'instance de serveur principal attend que l'instance de serveur miroir écrive l'enregistrement du journal de transaction dans la file d'attente de restauration par progression. Cette valeur est utile uniquement en mode haute sécurité.  
  
 **Durée (estimée) d'envoi et de restauration de la totalité du journal en cours**  
 Durée requise pour envoyer et restaurer la totalité du journal qui a été validée sur le principal à l'heure actuelle. Cette durée peut être inférieure à la somme des valeurs des champs **Durée (estimée) d’envoi du journal** et **Durée (estimée) de restauration du journal** , car l’envoi et la restauration peuvent se produire simultanément. Cette estimation ne prédit pas la durée requise pour envoyer et restaurer de nouvelles transactions validées sur le principal lors du travail sur des retards dans la file d'attente d'envoi.  
  
 **Adresse témoin**  
 Adresse réseau de l'instance du serveur témoin. Pour plus d’informations sur le format de cette adresse, consultez [Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
 **Mode d'opération**  
 Mode d'opération de la session de mise en miroir de bases de données :  
  
-   **Haute performance (asynchrone)**  
  
-   **Haute sécurité sans basculement automatique (synchrone)**  
  
-   **Haute sécurité avec basculement automatique (synchrone)**  
  
## <a name="remarks"></a>Notes  
 Les membres du rôle de base de données fixe **dbm_monitor** peuvent consulter l’état de la mise en miroir existant à l’aide du moniteur de mise en miroir de bases de données ou de la procédure stockée **sp_dbmmonitorresults** . Cependant, ces utilisateurs ne peuvent pas mettre à jour la table d'état. Ils dépendent du **Travail du moniteur de mise en miroir de bases de données** pour la mise à jour de la table d’état à intervalles réguliers. Pour connaître la durée de vie de l’état affiché, un utilisateur peut regarder les heures des étiquettes **Journal principal (** _\<time>_ **)** et **Journal miroir (** _\<time>_ **)** .  
  
 Si ce travail n'existe pas ou que l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est arrêté, l'état devient rapidement obsolète et risque de ne plus refléter la configuration de la session de mise en miroir. Par exemple, après un basculement, les partenaires peuvent sembler partager le même rôle (principal ou miroir), ou le serveur principal actuel peut être affiché comme serveur miroir, alors que le serveur miroir actuel est affiché comme principal.  
  
## <a name="see-also"></a>Voir aussi  
 [Démarrer le moniteur de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Surveillance de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Démarrer l’Assistant Configuration de la sécurité de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
