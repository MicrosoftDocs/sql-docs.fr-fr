---
title: Définir des seuils d’avertissement | Microsoft Docs
description: Découvrez comment utiliser la boîte de dialogue Définir les seuils d’avertissement pour activer et configurer les seuils d’avertissement pour les bases de données mises en miroir sélectionnées dans SQL Server.
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.setwarningthreshold.f1
ms.assetid: 17f93147-e7d9-4092-b4c2-c11b38051171
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c5433b64027311fbf98b8c62bd8d25d1fbc3fdfb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352256"
---
# <a name="set-warning-thresholds"></a>Définir les seuils d'avertissement
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilisez cette boîte de dialogue pour activer et configurer un ou plusieurs seuils d'avertissement pour la base de données sélectionnée dans l'arborescence de navigation de la boîte de dialogue **Moniteur de mise en miroir de bases de données** .  
  
 La boîte de dialogue tente d'établir une connexion avec les deux instances de serveurs. Ces connexions sont établies de façon asynchrone. La boîte de dialogue affiche l'état de la connexion de chaque partenaire. Si le partenaire n'est pas connecté, vous pouvez cliquer sur **Se connecter**.  
  
 **Pour utiliser SQL Server Management Studio pour contrôler la mise en miroir de base de données**  
  
-   [Démarrer le moniteur de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="options"></a>Options  
 *Instance de serveur et état de la connexion*  
 Nom d’une instance de serveur partenaire sous la forme *SYSTÈME* **\\** _NOM_INSTANCE_. Pour une instance de serveur par défaut, seul le nom du système s'affiche.  
  
 Ce champ indique également si le moniteur est actuellement connecté à cette instance de serveur. Les états de connexion possibles sont les suivants :  
  
-   **Non connecté à**  *nom_instance_serveur*  
  
-   **Tentative de connexion à**  *nom_instance_serveur*  
  
-   **Connecté à**   *nom_instance_serveur*  
  
    > [!NOTE]  
    >  Si vous n’êtes pas membre du rôle de serveur fixe **sysadmin**, l’état est **Connecté à** *nom_instance_serveur* **(Autorisations limitées)** .  
  
 Le nom de chaque instance de serveur partenaire est affiché dans un champ d' *instance de serveur et d'état de la connexion* distinct. Le premier champ affiche le serveur principal au démarrage du moniteur.  
  
 **Se connecter**/**Annuler**  
 Un bouton **Se connecter**/**Annuler** est associé à chaque champ d’ *instance de serveur et état de la connexion* . L'état de ce bouton dépend de l'état de la connexion :  
  
-   Si aucune connexion à l'instance de serveur n'est établie, l'intitulé du bouton est **Se connecter**. Cliquez sur le bouton pour vous connecter à l'instance de serveur.  
  
-   Lorsqu'une tentative de connexion est en cours, l'intitulé du bouton est **Annuler**. Cliquez sur le bouton pour annuler la tentative de connexion.  
  
-   Si le serveur est connecté, le bouton est grisé et son intitulé est **Connecté**.  
  
 **Seuils**  
 La grille **Seuils** affiche les paramètres d’avertissement pour les deux instances de serveurs.  
  
> [!NOTE]  
>  Si une instance de serveur n'est pas connectée, ses colonnes affichent des cellules vides sur un fond gris. Lorsque la connexion s'ouvre, la grille affiche automatiquement le contenu à partir de l'instance.  
  
 Cette grille comporte les colonnes suivantes :  
  
 **Avertissements**  
 Répertorie les avertissements pris en charge :  
  
|Avertissement|Description|  
|-------------|-----------------|  
|**Avertir si le journal non envoyé dépasse le seuil**|Ce seuil indique la taille en kilo-octets (Ko) du journal non envoyé dans la file d'attente d'envoi sur le principal.|  
|**Avertir si le journal non restauré dépasse le seuil**|Ce seuil indique la taille (en Ko) de la file d'attente de restauration sur l'instance du serveur miroir.|  
|**Avertir si la durée de vie de la plus ancienne transaction non envoyée dépasse le seuil**|Ce seuil indique la durée exprimée en minutes des transactions qui n'ont pas encore été envoyées depuis la file d'attente d'envoi vers l'instance de serveur miroir. Cette valeur permet de mesurer la perte de données potentielle en termes de temps.|  
|**Avertir si le temps de traitement de validation de miroir dépasse le seuil**|Ce seuil indique le délai en millisecondes par transaction (utile uniquement en mode haute sécurité). Ce délai correspond au temps de traitement pendant lequel l'instance de serveur principal attend que l'instance de serveur miroir écrive l'enregistrement du journal de transaction dans la file d'attente de restauration par progression.|  
  
 **Activé à «**   *\<server instance>*   **»**  
 Une case à cocher vide indique que l'avertissement est actuellement désactivé sur l'instance de serveur. Pour activer l'avertissement, activez la case à cocher.  
  
 **Seuil à «**   *\<server instance>*   **»**  
 Lorsqu'un avertissement est activé, définissez le seuil dans la partie gauche de cette colonne. Un événement se produit si le seuil spécifié est atteint lors de la mise à jour de la table d'état. Si vous désactivez un seuil après avoir configuré une valeur, cette valeur reste dans le champ et sera utilisée lorsque vous activerez de nouveau l'avertissement.  
  
 Lorsqu'un avertissement n'est pas activé, ce champ est inactif.  
  
 **OK**  
 Quand vous cliquez sur **OK** , cette boîte de dialogue se ferme et les valeurs actuellement spécifiées pour les seuils d’avertissement s’affichent dans la grille **Seuils** de la page à onglets **Avertissements**.  
  
## <a name="remarks"></a>Notes  
 Un seuil est applicable à un seul partenaire à la fois, mais nous vous recommandons de définir un seuil pour un événement donné sur les deux partenaires, afin de vous assurer que l'avertissement persiste lors du basculement de la base de données. Le seuil approprié pour chaque partenaire dépend des capacités de performance du système du partenaire.  
  
 Un événement est écrit dans le journal des événements pour une performance uniquement si sa valeur est identique ou supérieure au seuil au moment de la mise à jour de la table d'état. Si une valeur maximale atteint momentanément le seuil entre des mises à jour d'état, cette valeur est ignorée.  
  
## <a name="see-also"></a>Voir aussi  
 [Démarrer le moniteur de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Surveillance de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Démarrer l’Assistant Configuration de la sécurité de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
