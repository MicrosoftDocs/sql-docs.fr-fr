---
description: MSSQLSERVER_3159
title: MSSQLSERVER_3159 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3159 (Database Engine error)
ms.assetid: c93c1003-0e3a-40aa-9873-44a0f5b8b57e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d83e149a8f17c509f2c34c9284c43b92828091dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194229"
---
# <a name="mssqlserver_3159"></a>MSSQLSERVER_3159
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|ID de l’événement|3159|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|LDDB_LOGNOTBACKEDUP|  
|Texte du message|La fin du journal pour la base de données "%ls" n'a pas été sauvegardée. Utilisez BACKUP LOG WITH NORECOVERY pour sauvegarder le journal s'il contient des travaux que vous ne voulez pas perdre. Utilisez la clause WITH REPLACE ou WITH STOPAT de l'instruction RESTORE pour remplacer simplement le contenu du journal.|  
  
## <a name="explanation"></a>Explication  
Dans la plupart des cas, en mode de restauration complète ou en mode de récupération utilisant les journaux de transactions, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exige que vous sauvegardiez la fin du journal pour capturer les enregistrements du journal qui n'ont pas encore été sauvegardés. Une sauvegarde de la fin du journal effectuée juste avant une opération de restauration s'appelle une « sauvegarde de la fin du journal ».  
  
Si vous récupérez une base de données jusqu'à la défaillance, la sauvegarde de la fin du journal est la dernière sauvegarde pertinente du plan de récupération. Si vous ne pouvez pas effectuer la sauvegarde de la fin du journal, vous pouvez récupérer une base de données uniquement jusqu'à la fin de la dernière sauvegarde ayant été créée avant la défaillance.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiert habituellement la réalisation d'une sauvegarde de la fin du journal avant que vous commenciez à restaurer une base de données. Une sauvegarde de la fin du journal empêche la perte de données et préserve la continuité de la séquence de journaux de transactions consécutifs. Néanmoins, tous les scénarios de restauration ne nécessitent pas une sauvegarde de la fin du journal. Vous n'êtes pas obligé de disposer d'une sauvegarde de la fin du journal si le point de récupération est contenu dans une sauvegarde de journal antérieure ou si vous déplacez ou remplacez (par écrasement) la base de données et ne souhaitez pas la restaurer à un point donné après la sauvegarde la plus récente. De plus, si les fichiers journaux sont endommagés et une sauvegarde de la fin du journal ne peut pas être créée, vous devez restaurer la base de données sans utiliser une sauvegarde de la fin du journal. Les transactions validées après la dernière sauvegarde de journal sont perdues. Pour plus d’informations, consultez « Restauration sans utiliser de sauvegarde de la fin du journal » plus loin dans cette rubrique.  
  
> [!CAUTION]  
> L'option REPLACE doit être utilisée rarement et uniquement après un examen attentif.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Effectuez une sauvegarde de la fin du journal et retentez l'opération de restauration.  
  
Si vous ne pouvez pas sauvegarder la fin du journal, utilisez la clause WITH STOPAT ou WITH REPLACE dans vos instructions RESTORE.  
  
## <a name="see-also"></a>Voir aussi  
[Restaurer une base de données SQL Server jusqu’à une limite dans le temps &#40;mode de récupération complète&#41;](~/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
[Sauvegarder le journal des transactions lorsque la base de données est endommagée &#40;SQL Server&#41;](~/relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)  
[Sauvegarder un journal des transactions &#40;SQL Server&#41;](~/relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
[Sauvegardes de la fin du journal &#40;SQL Server&#41;](~/relational-databases/backup-restore/tail-log-backups-sql-server.md)  
  
