---
description: sp_dbmmonitorchangealert (Transact-SQL)
title: sp_dbmmonitorchangealert (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbmmonitorchangealert_TSQL
- sp_dbmmonitorchangealert
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbmmonitorchangealert
- database mirroring [SQL Server], monitoring
ms.assetid: 1b29f82b-9cf8-4539-8d5c-9a1024db8a50
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e250cc4fb95c32b677db304f5e4de321c14448a6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212407"
---
# <a name="sp_dbmmonitorchangealert-transact-sql"></a>sp_dbmmonitorchangealert (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute ou modifie un seuil d'avertissement pour une métrique de performance de mise en miroir spécifiée.  

  
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dbmmonitorchangealert database_name   
    , alert_id   
    , alert_threshold   
    , enabled   
```  
  
## <a name="arguments"></a>Arguments  
 *database_name*  
 Spécifie la base de données pour laquelle ajouter ou modifier le seuil d'avertissement spécifié.  
  
 *alert_id*  
 Entier qui identifie l'avertissement à ajouter ou à modifier. Spécifiez l'une des valeurs suivantes :  
  
|Valeur|Mesure de performance|Seuil d'avertissement|  
|-----------|------------------------|-----------------------|  
|1|Transaction non envoyée la plus ancienne|Spécifie le nombre de minutes de transactions pouvant s'accumuler dans la file d'attente d'envoi avant qu'un avertissement ne soit généré sur l'instance de serveur principal. Cet avertissement permet de mesurer le risque de perte de données en termes de temps et s'avère particulièrement approprié en mode hautes performances. Toutefois, l'avertissement est également approprié en mode haute sécurité lorsque la mise en miroir est interrompue ou suspendue en raison de la déconnexion des partenaires.|  
|2|Journal non envoyé|Spécifie la quantité de kilo-octets (Ko) de journal non envoyé qui génère un avertissement sur l'instance de serveur principal. Cet avertissement aide à mesurer le risque de perte de données en termes de Ko et il est particulièrement utile pour le mode hautes performances. Toutefois, l'avertissement est également approprié en mode haute sécurité lorsque la mise en miroir est interrompue ou suspendue en raison de la déconnexion des partenaires.|  
|3|Journal non restauré|Spécifie la quantité de Ko de journal non restauré qui génère un avertissement sur l'instance de serveur miroir. Cet avertissement permet de mesurer le temps de basculement. Le *temps de basculement* est principalement constitué du temps nécessaire à l'ancien serveur miroir pour restaurer par progression tout journal demeuré dans sa file d'attente de restauration par progression et d'un court laps de temps supplémentaire.|  
|4|Charge de validation par le serveur miroir|Spécifie le nombre de millisecondes de délai moyen par transaction qui sont tolérés avant qu'un avertissement soit généré sur le serveur principal. Ce délai correspond au temps de traitement pendant lequel l'instance de serveur principal attend que l'instance de serveur miroir écrive l'enregistrement du journal de transaction dans la file d'attente de restauration par progression. Cette valeur est utile uniquement en mode haute sécurité.|  
|5|Période de rétention|Métadonnées qui déterminent la durée de conservation des lignes dans la table de l'état des mises en miroir de base de données.|  
  
 Pour plus d’informations sur les ID d’événement correspondant aux avertissements, consultez [utiliser des seuils d’avertissement et des alertes sur les métriques de performances de mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/use-warning-thresholds-and-alerts-on-mirroring-performance-metrics-sql-server.md).  
  
 *alert_threshold*  
 Valeur de seuil de l'avertissement. Si une valeur supérieure à ce seuil est renvoyée lorsque l'état des mises en miroir est mis à jour, une entrée est insérée dans le journal des événements Windows. Cette valeur est exprimée en Ko, minutes ou millisecondes, suivant la mesure de performance.  
  
> [!NOTE]  
>  Pour afficher les valeurs actuelles, exécutez la procédure stockée [sp_dbmmonitorresults](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md) .  
  
 *activé*  
 Indique si l'avertissement est activé.  
  
 0 = L'avertissement est désactivé.  
  
 1 = L'avertissement est activé.  
  
> [!NOTE]  
>  La période de rétention est toujours activée.  
  
## <a name="return-code-values"></a>Codet de retour  
 Aucun  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant définit les seuils pour chaque métrique de performance et la période de rétention pour la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Le tableau ci-dessous montre les valeurs utilisées dans l'exemple.  
  
|*alert_id*|Mesure de performance|Seuil d'avertissement|Indique si l'avertissement est activé.|  
|-----------------|------------------------|-----------------------|-----------------------------|  
|1|Transaction non envoyée la plus ancienne|30 minutes|Oui|  
|2|Journal non envoyé|10 000 Ko|Oui|  
|3|Journal non restauré|10 000 Ko|Oui|  
|4|Charge de validation par le serveur miroir|1 000 millisecondes|Non|  
|5|Période de rétention|8 heures|Oui|  
  
```  
EXEC sp_dbmmonitorchangealert AdventureWorks2012, 1, 30, 1 ;  
EXEC sp_dbmmonitorchangealert AdventureWorks2012, 2, 10000, 1 ;  
EXEC sp_dbmmonitorchangealert AdventureWorks2012, 3, 10000, 1 ;  
EXEC sp_dbmmonitorchangealert AdventureWorks2012, 4, 1000, 0 ;  
EXEC sp_dbmmonitorchangealert AdventureWorks2012, 5, 8, 1 ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Surveillance de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [sp_dbmmonitorhelpalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorhelpalert-transact-sql.md)   
 [sp_dbmmonitordropalert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitordropalert-transact-sql.md)  
  
  
