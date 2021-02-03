---
description: MSSQLSERVER_601
title: MSSQLSERVER_601 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
f1_keywords:
- "601"
helpviewer_keywords:
- 601 (Database Engine error)
ms.assetid: 2039cc0a-9a43-4369-a04a-935e384388b6
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 91fc178421dffd9e221221d8df76a32c9a7a8942
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190703"
---
# <a name="mssqlserver_601"></a>MSSQLSERVER_601
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|601|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique||  
|Texte du message|Impossible de poursuivre l'analyse avec NOLOCK car les données ont été déplacées.|  
  
## <a name="explanation"></a>Explication  
Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ne peut pas poursuivre l'exécution de la requête, car elle tente de lire des données qui ont été mises à jour ou supprimées par une autre transaction. La requête utilise l'indicateur de verrouillage NOLOCK ou le niveau d'isolement de la transaction READ UNCOMMITTED.  
  
L'accès aux données qui sont modifiées par une autre transaction est généralement refusé en raison des verrous appliqués aux données. Toutefois, l'indicateur de verrouillage NOLOCK et le niveau d'isolement de la transaction READ UNCOMMITTED permettent à une requête de lire les données qui sont verrouillées par une autre transaction. Il s'agit alors d'une lecture erronée car vous êtes en mesure de lire les valeurs qui n'ont pas encore été validées et qui sont susceptibles d'être modifiées.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Cette erreur annule la requête. Soumettez une nouvelle fois la requête ou supprimez l'indicateur de verrouillage NOLOCK.  
  
## <a name="see-also"></a>Voir aussi  
[MSSQLSERVER_605](../../relational-databases/errors-events/mssqlserver-605-database-engine-error.md)  
[Indicateurs de table &#40;Transact-SQL&#41;](~/t-sql/queries/hints-transact-sql-table.md)  
[SELECT &#40;Transact-SQL&#41;](~/t-sql/queries/select-transact-sql.md)  
[SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](~/t-sql/statements/set-transaction-isolation-level-transact-sql.md)  
  
