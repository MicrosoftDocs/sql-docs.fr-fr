---
title: Moniteur de mise en miroir de bases de données (Page Avertissements) | Microsoft Docs
description: Utilisez cette page en lecture seule du Moniteur de mise en miroir de bases de données pour voir les avertissements pris en charge sur les événements de mise en miroir de bases de données et les valeurs de seuil d'avertissement, si disponibles.
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.warningsandalerts.f1
ms.assetid: 01936122-961d-436b-ba3c-5f79fefe5469
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: e55294b2bbba96ebe9dda253852d3f1b8280220f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353098"
---
# <a name="database-mirroring-monitor-warnings-page"></a>Moniteur de mise en miroir de bases de données (Page Avertissements)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Affiche une liste, accessible en lecture seule, d'avertissements pris en charge lors d'événements de mise en miroir de bases de données, ainsi que les valeurs de seuil spécifiées pour les avertissements, si disponible.  
  
 **Pour utiliser SQL Server Management Studio pour contrôler la mise en miroir de base de données**  
  
-   [Démarrer le moniteur de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="columns"></a>Colonnes  
 **Avertissement**  
 Les avertissements pour lesquels vous pouvez définir un seuil sont les suivants :  
  
|Avertissement|Seuil|  
|-------------|---------------|  
|**Avertir si le journal non envoyé dépasse le seuil**|Spécifie le nombre de kilo-octets (Ko) de journal non envoyé qui génère un avertissement sur l'instance de serveur principal. Cet avertissement permet de mesurer le risque de perte de données en termes de Ko et s'avère particulièrement approprié en mode hautes performances. Toutefois, l'avertissement est également approprié en mode haute sécurité lorsque la mise en miroir est interrompue ou suspendue en raison de la déconnexion des partenaires.|  
|**Avertir si le journal non restauré dépasse le seuil**|Spécifie le nombre de kilo-octets (Ko) de journal non restauré qui génère un avertissement sur l'instance de serveur miroir. Cet avertissement permet de mesurer le temps de basculement en kilo-octets. Le *temps de basculement* est principalement constitué du temps nécessaire à l'ancien serveur miroir pour restaurer par progression tout journal demeuré dans sa file d'attente de restauration par progression et d'un court laps de temps supplémentaire.<br /><br /> Remarque : Pour un basculement automatique, le temps nécessaire au système pour remarquer l'erreur dépend du temps de basculement.<br /><br /> Pour en savoir plus, voir [Estimer l’interruption de service au cours d’un basculement de rôle &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md).|  
|**Avertir si la durée de vie de la plus ancienne transaction non envoyée dépasse le seuil**|Spécifie le nombre de minutes de transactions pouvant s'accumuler dans la file d'attente d'envoi avant qu'un avertissement ne soit généré sur l'instance de serveur principal. Cet avertissement permet de mesurer le risque de perte de données en termes de durée et s'avère particulièrement approprié en mode hautes performances. Toutefois, l'avertissement est également approprié en mode haute sécurité lorsque la mise en miroir est interrompue ou suspendue en raison de la déconnexion des partenaires.|  
|**Avertir si le temps de traitement de validation de miroir dépasse le seuil**|Spécifie, en millisecondes, le délai moyen par transaction au terme duquel un avertissement est généré sur le serveur principal. Ce délai correspond au temps de traitement pendant lequel l'instance de serveur principal attend que l'instance de serveur miroir écrive l'enregistrement du journal de transaction dans la file d'attente de restauration par progression. Cette valeur est utile uniquement en mode haute sécurité.|  
  
 **Seuil à «**   _&lt;instance_serveur_   **»**  
 Pour chaque avertissement, affiche le cas échéant le seuil spécifié par l'utilisateur actuel pour l'une des instances de serveurs. Le nom complet de l'instance de serveur est spécifié dans l'en-tête de colonne correspondante.  
  
 Pour plus d'informations, consultez la section « Remarques » plus loin dans cette rubrique.  
  
 **Définir les seuils**  
 Cliquez sur ce bouton pour définir un seuil par avertissement sur chaque partenaire de basculement.  
  
 Pour plus d'informations, consultez la section « Remarques » plus loin dans cette rubrique.  
  
## <a name="remarks"></a>Notes  
 Si aucune information n’est actuellement disponible pour une instance de serveur, le texte affiché dans les cellules de la colonne **Seuil à** correspondante apparaît en filigrane sur un fond gris. Si le moniteur n’est pas connecté à l’instance de serveur, chaque cellule de la grille affiche le texte **Non connecté à** _<NOM_SYSTÈME>_ ou **Non connecté à** _<NOM_SYSTÈME>_ **\\** _<nom_instance>_ , selon qu’il s’agit de l’instance par défaut ou d’une instance nommée. Si le moniteur est en attente de retour d’une requête, chaque cellule de la grille affiche le texte **Attente de données...** .  
  
 Quand des informations sont disponibles, la cellule correspondant à chaque avertissement affiche une valeur de seuil spécifique (ainsi qu’une unité de mesure) ou le texte **Non activé**.  
  
 Si une valeur de seuil est dépassée au moment de l'actualisation de la table d'état, un événement est consigné dans le journal des événements Windows, lorsque la ligne d'état est enregistrée. Par défaut, la ligne d'état est enregistrée toutes les minutes si le moniteur n'est pas en cours d'exécution. Vous pouvez configurer une alerte sur chaque type d'événement consigné en utilisant l'Agent SQL Server ou un autre programme tel que Microsoft Operations Manager (MOM).  
  
 Pour un partenaire spécifique, les événements consignés dépendent de son rôle actuel (principal ou miroir). Cependant, nous vous recommandons de définir un seuil d'avertissement pour un événement spécifique sur les deux partenaires, afin de vous assurer que l'avertissement persiste lors du basculement de la base de données. Le seuil approprié pour chaque partenaire dépend des capacités de performance du système du partenaire.  
  
> [!NOTE]  
>  Vous pouvez également utiliser la procédure stockée système **sp_dbmmonitorchangealert** pour configurer des seuils pour les événements équivalents (journal non envoyé, journal non restauré, transaction non envoyée la plus ancienne et temps de traitement de validation de miroir). Pour plus d’informations, consultez [sp_dbmmonitorchangealert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorchangealert-transact-sql.md).  
  
 Le tableau suivant affiche l'ID de l'événement associé à chaque avertissement.  
  
|Avertissement du moniteur de mise en miroir de bases de données|Nom d'événement|ID de l’événement|  
|----------------------------------------|----------------|--------------|  
|**Avertir si le journal non envoyé dépasse le seuil**|Journal non envoyé|32042|  
|**Avertir si le journal non restauré dépasse le seuil**|Journal non restauré|32043|  
|**Avertir si la durée de vie de la plus ancienne transaction non envoyée dépasse le seuil**|Transaction non envoyée la plus ancienne|32044|  
|**Avertir si le temps de traitement de validation de miroir dépasse le seuil**|Charge de validation par le serveur miroir|32045|  
  
## <a name="permissions"></a>Autorisations  
 Pour un accès complet, nécessite l’appartenance au rôle serveur fixe **sysadmin** . Seuls les membres de **sysadmin** peuvent configurer et afficher des seuils d’avertissement pour les mesures de performances clés.  
  
 L’appartenance au rôle **dbm_monitor** vous permet de consulter uniquement la ligne d’état la plus récente dans la page **Avertissements** .  
  
## <a name="see-also"></a>Voir aussi  
 [Démarrer le moniteur de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Surveillance de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Démarrer l’Assistant Configuration de la sécurité de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
