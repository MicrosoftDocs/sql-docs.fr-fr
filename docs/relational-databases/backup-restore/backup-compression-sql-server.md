---
title: Compression des sauvegardes (SQL Server) | Microsoft Docs
description: Découvrez la compression des sauvegardes SQL Server, notamment les restrictions, les compromis à faire au niveau des performances, la configuration de la compression des sauvegardes et le taux de compression.
ms.custom: ''
ms.date: 07/08/2020
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], backup compression
- backup compression [SQL Server], about backup compression
- compression [SQL Server], backup compression
- backups [SQL Server], compression
- backing up [SQL Server], backup compression
- backup compression [SQL Server]
ms.assetid: 05bc9c4f-3947-4dd4-b823-db77519bd4d2
author: cawrites
ms.author: chadam
ms.openlocfilehash: f083aff61ad674e0190a8789c9d8c19b0f4a9821
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236152"
---
# <a name="backup-compression-sql-server"></a>Compression de sauvegardes (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique décrit la compression des sauvegardes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , notamment les restrictions, les compromis en termes de performances pour la compression des sauvegardes, la configuration pour la compression des sauvegardes et le taux de compression.  La compression de la sauvegarde est prise en charge uniquement sur les éditions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : Entreprise, Standard et Développeur.  Chaque édition de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et ultérieure peut restaurer une sauvegarde compressée. 
 
  
##  <a name="benefits"></a><a name="Benefits"></a> Avantages  
  
-   Une sauvegarde compressée étant plus petite qu'une sauvegarde non compressée des mêmes données, la compression d'une sauvegarde requiert en général moins d'E/S de périphérique et, par conséquent, augmente souvent considérablement la vitesse de la sauvegarde.  
  
     Pour plus d'informations, consultez [Impact sur les performances de la compression des sauvegardes](#PerfImpact), plus loin dans cette rubrique.  
  
  
##  <a name="restrictions"></a><a name="Restrictions"></a> Restrictions  
 Les restrictions suivantes s'appliquent aux sauvegardes compressées :  
  
-   Les sauvegardes compressées et non compressées ne peuvent pas co-exister dans un support de sauvegardes.  
  
-   Toutefois, les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peuvent pas lire les sauvegardes compressées.  
  
-   NTbackups ne peut pas partager de bande avec les sauvegardes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compressées.  
  
  
##  <a name="performance-impact-of-compressing-backups"></a><a name="PerfImpact"></a> Impact sur les performances de la compression des sauvegardes  
 Par défaut, la compression augmente considérablement l'utilisation de l'UC et l'UC supplémentaire consommée par le processus de compression peut avoir un impact néfaste sur les opérations simultanées. Ainsi, dans une session où l’utilisation de l’UC est limitée, il peut être préférable de créer une sauvegarde compressée de priorité basse à l’aide de[Resource Governor](../../relational-databases/resource-governor/resource-governor.md). Pour plus d'informations, consultez [Utiliser Resource Governor pour limiter l’utilisation de l’UC par compression de la sauvegarde &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md).  
  
 Pour obtenir une bonne image de vos performances d'E/S de sauvegarde, vous pouvez isoler l'E/S de sauvegarde en direction ou depuis des unités en évaluant les types suivants de compteurs de performance :  
  
-   Compteurs de performance d'E/S Windows, tels que les compteurs de disque physique  
  
-   Compteur **Débit d’unité en octets/s** de l’objet [SQLServer:Backup Device](../../relational-databases/performance-monitor/sql-server-backup-device-object.md)  
  
-   Compteur **Débit de sauvegarde/restauration/s** de l’objet [SQLServer:Databases](../../relational-databases/performance-monitor/sql-server-databases-object.md)  
  
 Pour des informations sur les compteurs Windows, consultez l'aide de Windows. Pour obtenir des informations sur l’utilisation des compteurs SQL Server, consultez [Utiliser des objets SQL Server](../../relational-databases/performance-monitor/use-sql-server-objects.md).  
  
   
##  <a name="calculate-the-compression-ratio-of-a-compressed-backup"></a><a name="CompressionRatio"></a> Calculer le taux de compression d'une sauvegarde compressée  
 Pour calculer le taux de compression d’une sauvegarde, utilisez les valeurs pour la sauvegarde dans les colonnes **backup_size** et **compressed_backup_size** de la table de l’historique [backupset](../../relational-databases/system-tables/backupset-transact-sql.md) , comme suit :  
  
 **backup_size**:**compressed_backup_size**  
  
 Par exemple, un taux de compression 3:1 indique que vous économisez environ 66 % de l'espace disque. Pour effectuer une requête sur ces colonnes, vous pouvez utiliser l'instruction Transact-SQL suivante :  
  
```sql  
SELECT backup_size/compressed_backup_size FROM msdb..backupset;  
```  
  
 Le taux de compression d'une sauvegarde compressée dépend des données compressées. Divers facteurs peuvent avoir une incidence sur le taux de compression obtenu. Les facteurs majeurs sont :  
  
-   Le type des données.  
  
     Les données caractères se compressent plus que d'autres types de données.  
  
-   La cohérence des données dans les lignes sur une page.  
  
     En général, si une page contient plusieurs lignes dans lesquelles un champ contient la même valeur, une compression importante peut se produire pour cette valeur. En revanche, pour une base de données qui contient des données aléatoires ou qui contient une seule grande ligne par page, une sauvegarde compressée serait presque aussi importante qu'une sauvegarde non compressée.  
  
-   Si les données sont chiffrées  
  
     Le taux de compression des données chiffrées est beaucoup moins élevé que celui des données non chiffrées correspondantes. Par exemple, si les données sont chiffrées au niveau de la colonne avec Always Encrypted, ou avec un autre chiffrement au niveau de l’application, la compression des sauvegardes peut ne pas réduire la taille de manière significative.

     Pour plus d’informations sur la compression des bases de données chiffrées avec Transparent Data Encryption (TDE), consultez [Compression des sauvegardes avec TDE](#backup-compression-with-tde).

-   Si la base de données est compressée.  
  
     Si la base de données est compressée, compresser des sauvegardes peut réduire faiblement leur taille, voire pas du tout.  

## <a name="backup-compression-with-tde"></a>Compression des sauvegardes avec TDE

Avec [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures, définir `MAXTRANSFERSIZE` **sur une valeur supérieure à 65536 (64 Ko)** permet d’utiliser un algorithme de compression optimisé pour les bases de données chiffrées avec [Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md), qui chiffre d’abord une page, la compresse, puis la chiffre de nouveau. Si `MAXTRANSFERSIZE` n’est pas spécifiée, ou si `MAXTRANSFERSIZE = 65536` (64 Ko) est utilisé, la compression de sauvegarde pour les bases de données chiffrées avec TDE compresse directement les pages chiffrées et peut ne pas fournir de bons taux de compression. Pour plus d’informations, consultez [Backup Compression for TDE-enabled Databases](/archive/blogs/sqlcat/sqlsweet16-episode-1-backup-compression-for-tde-enabled-databases).

À partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU5, la définition de `MAXTRANSFERSIZE` n’est plus nécessaire pour activer cet algorithme de compression optimisé avec TDE. Si la commande de sauvegarde est spécifiée `WITH COMPRESSION` ou que la configuration serveur de *compression par défaut des sauvegardes* est définie sur 1, `MAXTRANSFERSIZE` sera automatiquement augmentée à 128 K pour activer l’algorithme optimisé. Si `MAXTRANSFERSIZE` est spécifiée dans la commande Backup avec une valeur > 64 Ko, la valeur fournie est respectée. En d’autres termes, SQL Server ne diminue jamais automatiquement la valeur, elle l’augmente uniquement. Si vous avez besoin de sauvegarder une base de données chiffrée TDE avec `MAXTRANSFERSIZE = 65536`, vous devez spécifier `WITH NO_COMPRESSION` ou vous assurer que la configuration serveur de *compression par défaut des sauvegardes* est définie sur 0.

Pour plus d’informations, consultez [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md).

##  <a name="allocation-of-space-for-the-backup-file"></a><a name="Allocation"></a> Allocation d'espace pour le fichier de sauvegarde  
 Pour les sauvegardes compressées, la taille du fichier de sauvegarde final dépend de la capacité de compression des données. Or, celle-ci n'est pas connue avant la fin de l'opération de sauvegarde.  Par conséquent, par défaut, lors de la sauvegarde d'une base de données faisant appel à la compression, le moteur de base de données utilise un algorithme de préallocation pour le fichier de sauvegarde. Cette algorithme préalloue un pourcentage prédéfini de la taille de la base de données pour le fichier de sauvegarde. Si davantage d'espace est requis au cours de l'opération de sauvegarde, le moteur de base de données augmente la taille du fichier. Si la taille finale est inférieure à l'espace alloué, à la fin de l'opération de sauvegarde, le moteur de base de données réduit le fichier à la taille finale réelle de la sauvegarde.  
  
 Pour permettre au fichier de sauvegarde de croître autant que nécessaire uniquement afin d'atteindre sa taille définitive, utilisez l'indicateur de trace 3042. L'indicateur de trace 3042 permet à l'opération de sauvegarde de contourner l'algorithme de préallocation de la compression de sauvegarde par défaut. Cet indicateur de trace est utile si vous devez économiser de l'espace en allouant uniquement la taille réelle requise pour la sauvegarde compressée. Toutefois, le recours à cet indicateur de trace peut entraîner une légère baisse des performances (augmentation possible de la durée de l'opération de sauvegarde).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Configurer la compression de sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/configure-backup-compression-sql-server.md)  
  
-   [Afficher ou configurer la compression par défaut des sauvegardes (option de configuration de serveur)](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)  
  
-   [Utiliser Resource Governor pour limiter l’utilisation de l’UC par compression de la sauvegarde &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md)  
  
-   [DBCC TRACEON &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md)  
  
-   [DBCC TRACEOFF &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceoff-transact-sql.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble de la sauvegarde &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Indicateurs de trace &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)  
  
