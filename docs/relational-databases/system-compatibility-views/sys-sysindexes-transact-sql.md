---
description: sys.sysindexes (Transact-SQL)
title: Index sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysindexes
- sysindexes_TSQL
- sys.sysindexes
- sys.sysindexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysindexes system table
- sys.sysindexes compatibility view
ms.assetid: f483d89c-35c4-4a08-8f8b-737fd80d13f5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cf8982115b1a4399c327aefc66a5e99a1d46c575
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094191"
---
# <a name="syssysindexes-transact-sql"></a>sys.sysindexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque index et chaque table de la base de données active. Cette vue ne prend pas en charge les index XML. Les tables et les index partitionnés ne sont pas entièrement pris en charge dans cette vue ; Utilisez plutôt l’affichage catalogue [sys. Indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) .  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Identificateur de la table à laquelle l'index appartient.|  
|**statut**|**int**|Informations sur l'état du système.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**first**|**binary(6)**|Pointeur vers la première page ou la page racine.<br /><br /> Non utilisé lorsque **indid** = 0.<br /><br /> NULL = l’index est partitionné lorsque **indid** > 1.<br /><br /> NULL = la table est partitionnée lorsque **indid** est égal à 0 ou 1.|  
|**indid**|**smallint**|Identificateur de l'index :<br /><br /> 0 = Segment de mémoire<br /><br /> 1 = index cluster<br /><br /> >1 = index non cluster|  
|**root**|**binary(6)**|Pour **indid** >= 1, **root** est le pointeur vers la page racine.<br /><br /> Non utilisé lorsque **indid** = 0.<br /><br /> NULL = l’index est partitionné lorsque **indid** > 1.<br /><br /> NULL = la table est partitionnée lorsque **indid** est égal à 0 ou 1.|  
|**minlen**|**smallint**|Taille minimale d'une ligne.|  
|**keycnt**|**smallint**|Nombre de clés.|  
|**GroupID**|**smallint**|Identificateur du groupe de fichiers sur lequel l'objet a été créé.<br /><br /> NULL = l’index est partitionné lorsque **indid** > 1.<br /><br /> NULL = la table est partitionnée lorsque **indid** est égal à 0 ou 1.|  
|**dpages**|**int**|Pour **indid** = 0 ou **indid** = 1, **dpages** est le nombre de pages de données utilisées.<br /><br /> Pour **indid** > 1, **dpages** est le nombre de pages d’index utilisées.<br /><br /> 0 = l’index est partitionné lorsque **indid** > 1.<br /><br /> 0 = la table est partitionnée lorsque **indid** est égal à 0 ou 1.<br /><br /> Ne fournit pas de résultats précis en cas de dépassement de capacité des données sur des lignes.|  
|**réservé**|**int**|Pour **indid** = 0 ou **indid** = 1, **reserved** est le nombre de pages allouées pour tous les index et données de table.<br /><br /> Pour **indid** > 1, **reserved** est le nombre de pages allouées pour l’index.<br /><br /> 0 = l’index est partitionné lorsque **indid** > 1.<br /><br /> 0 = la table est partitionnée lorsque **indid** est égal à 0 ou 1.<br /><br /> Ne fournit pas de résultats précis en cas de dépassement de capacité des données sur des lignes.|  
|**servir**|**int**|Pour **indid** = 0 ou **indid** = 1, **utilisé** est le nombre total de pages utilisées pour toutes les données d’index et de table.<br /><br /> Pour **indid** > 1, **utilisé** est le nombre de pages utilisées pour l’index.<br /><br /> 0 = l’index est partitionné lorsque **indid** > 1.<br /><br /> 0 = la table est partitionnée lorsque **indid** est égal à 0 ou 1.<br /><br /> Ne fournit pas de résultats précis en cas de dépassement de capacité des données sur des lignes.|  
|**rowcnt**|**bigint**|Nombre de lignes de niveau données basé sur **indid** = 0 et **indid** = 1.<br /><br /> 0 = l’index est partitionné lorsque **indid** > 1.<br /><br /> 0 = la table est partitionnée lorsque **indid** est égal à 0 ou 1.|  
|**rowmodctr**|**int**|Compte le nombre total de lignes insérées, supprimées ou mises à jour depuis la dernière mise à jour des statistiques de la table.<br /><br /> 0 = l’index est partitionné lorsque **indid** > 1.<br /><br /> 0 = la table est partitionnée lorsque **indid** est égal à 0 ou 1.<br /><br /> Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures, **rowmodctr** n’est pas entièrement compatible avec les versions antérieures. Pour plus d'informations, consultez la section Notes.|  
|**reserved3**|**int**|Retourne 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**reserved4**|**int**|Retourne 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xmaxlen**|**smallint**|Taille maximale d'une ligne|  
|**maxirow**|**smallint**|Taille maximale d'une ligne d'index non-feuille.<br /><br /> Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures, **maxirow** n’est pas entièrement compatible avec les versions antérieures.|  
|**OrigFillFactor**|**tinyint**|Valeur d'origine du taux de remplissage utilisé lors de la création de l'index. Cette valeur n'est pas conservée ; elle peut toutefois s'avérer utile si vous devez recréer un index et si vous avez oublié le taux de remplissage utilisé.|  
|**StatVersion**|**tinyint**|Retourne 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**reserved2**|**int**|Retourne 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**FirstIAM**|**binary(6)**|NULL = L'index est partitionné.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**impid**|**smallint**|Indicateur d'implémentation de l'index.<br /><br /> Retourne 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**lockflags**|**smallint**|Utilisé pour limiter les granularités de verrouillage d'un index. Par exemple, une table de recherche qui est essentiellement accessible en lecture seule peut être configurée pour poser uniquement des verrous de niveau table, de façon à minimiser les coûts de verrouillage.|  
|**pgmodctr**|**int**|Retourne 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**légende**|**varbinary(816)**|Liste des identificateurs de colonne pour les colonnes qui constituent la clé d'index.<br /><br /> Renvoie NULL.<br /><br /> Pour afficher les colonnes de clé d’index, utilisez [sys.sysindexkeys](../../relational-databases/system-compatibility-views/sys-sysindexkeys-transact-sql.md).|  
|**name**|**sysname**|Nom de l'index ou de la statistique. Retourne NULL lorsque **indid** = 0. Modifiez votre application pour rechercher le nom d'un segment de mémoire de valeur NULL.|  
|**statblob**|**image**|Statistiques sur les objets binaires volumineux (BLOB).<br /><br /> Renvoie NULL.|  
|**maxlen**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**rows**|**int**|Nombre de lignes de niveau données basé sur **indid** = 0 et **indid** = 1, et la valeur est répétée pour **indid** >1.|  
  
## <a name="remarks"></a>Notes  
 Les colonnes définies comme réservées ne doivent pas être utilisées.  
  
 Les colonnes **dpages**, **reserved** et **used** ne retournent pas de résultats précis si la table ou l’index contient des données dans l’unité d’allocation ROW_OVERFLOW. De plus, les nombres de pages de chaque index sont suivis séparément et ne sont pas agrégés pour la table de base. Pour afficher les nombres de pages, utilisez les affichages catalogue [sys.allocation_units](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md) ou [sys. partitions](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md) , ou la vue de gestion dynamique [sys.dm_db_partition_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md) .  
  
 Dans SQL Server 2000 et versions antérieures, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] conservait les compteurs de modifications de niveau ligne. De tels compteurs sont maintenant gérés au niveau colonne. Par conséquent, la colonne **rowmodctr** est calculée et produit des résultats similaires à ceux des versions antérieures, mais elles ne sont pas exactes.  
  
 Si vous utilisez la valeur dans **rowmodctr** pour déterminer à quel moment mettre à jour les statistiques, tenez compte des solutions suivantes :  
  
-   Ne rien faire. La nouvelle valeur de **rowmodctr** vous aide souvent à déterminer quand mettre à jour les statistiques, car le comportement est raisonnablement proche des résultats des versions antérieures.  
  
-   Utiliser AUTO_UPDATE_STATISTICS. Pour plus d’informations, consultez [statistiques](../../relational-databases/statistics/statistics.md).  
  
-   Utiliser une limite de temps pour déterminer à quel moment mettre à jour les statistiques. (par exemple, toutes les heures, chaque jour ou chaque semaine).  
  
-   Utiliser les informations de niveau application pour déterminer à quel moment mettre à jour les statistiques. Par exemple, chaque fois que la valeur maximale d’une colonne d' **identité** change de plus de 10 000, ou à chaque fois qu’une opération d’insertion en bloc est effectuée.  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Mappage de tables système à des vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  
