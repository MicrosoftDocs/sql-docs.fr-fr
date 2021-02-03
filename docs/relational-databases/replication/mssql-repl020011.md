---
description: MSSQL_REPL020011
title: MSSQL_REPL020011 | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_REPL020011 error
ms.assetid: f72072d7-bbb6-48ad-ac88-afa74aeb4d58
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: c5184aa24cd4405ee60a65c3a45ed56daf11645f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206173"
---
# <a name="mssql_repl020011"></a>MSSQL_REPL020011
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Détails du message  
  
|Attribut|Valeur|  
|-|-|  
|Nom du produit|SQL Server|  
|ID de l’événement|20011|  
|Source de l’événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|Le processus n'a pas pu exécuter '%1' sur '%2'.|  
  
## <a name="explanation"></a>Explication  
 Cette erreur peut être signalée dans plusieurs circonstances lors du traitement de la réplication transactionnelle, par exemple quand l’Agent de lecture du journal exécute **sp_replcmds** (Le processus n’a pas pu exécuter « sp_replcmds » sur \<ServerName>) ou **sp_repldone** (Le processus n’a pas pu exécuter « sp_repldone » sur \<ServerName>).  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Si cette erreur est signalée dans une base de données que vous venez tout juste de restaurer à partir d'une sauvegarde, vérifiez que vous avez suivi les étapes indiquées dans la documentation sur la sauvegarde et la restauration, notamment l'exécution de **sp_replrestart** si elle est appropriée. Pour plus d’informations, voir [Stratégies de sauvegarde et de restauration de la réplication transactionnelle et d'instantané](../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md).  
  
 Cette erreur est une erreur de traitement interne et, si elle est signalée dans d'autres circonstances que celle d'une restauration, elle indique généralement que la réplication doit être supprimée et reconfigurée. Si vous ne parvenez pas à supprimer la réplication, contactez le support technique pour obtenir de l'aide.  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)   
 [sp_repldone &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md)  
  
  
