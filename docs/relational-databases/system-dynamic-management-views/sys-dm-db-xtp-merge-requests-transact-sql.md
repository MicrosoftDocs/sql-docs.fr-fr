---
description: sys.dm_db_xtp_merge_requests (Transact-SQL)
title: sys.dm_db_xtp_merge_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: c1224e88-af74-4c99-ae32-d5d2c552a1f5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7a7f8f4a38bffb8abc6be1380a2225077130f143
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099850"
---
# <a name="sysdm_db_xtp_merge_requests-transact-sql"></a>sys.dm_db_xtp_merge_requests (Transact-SQL)

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

Suit les demandes de fusion de bases de données. La demande de fusion a peut-être été générée par SQL Server ou la demande a pu être effectuée par un utilisateur avec [sys.sp_xtp_merge_checkpoint_files (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-xtp-merge-checkpoint-files-transact-sql.md).

> [!NOTE]
> Cette vue de gestion dynamique (DMV), sys.dm_db_xtp_merge_requests, existe jusqu’à Microsoft SQL Server 2014.
> Mais à partir de SQL Server 2016 cette DMV ne s’applique plus.

## <a name="columns-in-the-report"></a>Colonnes du rapport

| Nom de la colonne | Type de données | Description |
| :-- | :-- | :-- |
| request_state | TINYINT | État de la demande de fusion :<br/>0 = demandé<br/>1 = en attente<br/>2 = installé<br/>3 = abandonné |
| request_state_desc | nvarchar(60) | Significations de l’état actuel de la requête :<br/><br/>Demandé : une demande de fusion existe.<br/>En attente : la fusion est en cours de traitement.<br/>Installé : la fusion est terminée.<br/>Abandonné-la fusion n’a pas pu se terminer, peut-être en raison d’un manque de stockage. |
| destination_file_id | GUID | Identificateur unique du fichier de destination pour la fusion des fichiers sources. |
| lower_bound_tsn | bigint | Horodateur minimal pour le fichier de fusion cible. Horodateur de transaction le plus bas pour tous les fichiers sources à fusionner. |
| upper_bound_tsn | bigint | Horodateur maximal pour le fichier de fusion cible. Horodateur de transaction le plus haut pour tous les fichiers sources à fusionner. |
| collection_tsn | bigint | Horodateur auquel la ligne actuelle peut être collectée.<br/><br/>Une ligne dans l'état Installé est supprimée lorsque checkpoint_tsn est supérieur à collection_tsn.<br/><br/>Une ligne dans l'état Abandonné est supprimée lorsque checkpoint_tsn est inférieur à collection_tsn. |
| checkpoint_tsn | bigint | Heure de démarrage du point de contrôle.<br/><br/>Toutes les suppressions effectuées par des transactions avec un horodateur inférieur à celui-ci sont prises en compte pour le nouveau fichier de données. Les suppressions restantes sont déplacées dans le fichier delta cible. |
| sourcenumber_file_id | GUID | Jusqu’à 16 ID de fichier internes qui identifient de façon unique les fichiers sources dans la fusion. |

## <a name="permissions"></a>Autorisations

Nécessite l'autorisation VIEW DATABASE STATE sur la base de données active.

## <a name="see-also"></a>Voir aussi

[Vues de gestion dynamique des tables mémoire optimisées (Transact-SQL)](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)
