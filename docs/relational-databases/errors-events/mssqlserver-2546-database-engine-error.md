---
description: MSSQLSERVER_2546
title: MSSQLSERVER_2546 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2546 (Database Engine error)
ms.assetid: c8f0e1b4-c7c4-45f2-9221-746714172313
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e85f7b8bfdcb06e0915e9c0966ff92bea9c35f9e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188859"
---
# <a name="mssqlserver_2546"></a>MSSQLSERVER_2546
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|2546|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBCC_INDEX_MARKED_DISABLED|  
|Texte du message|L'index 'INDEX_NAME' de la table 'OBJECT_NAME' est marqué comme étant désactivé. Reconstruisez l'index pour qu'il soit en ligne.|  
  
## <a name="explanation"></a>Explication  
L'index spécifié est marqué comme étant hors connexion ou il est désactivé. Cet index ne peut donc pas être vérifié.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Reconstruisez l'index en utilisant ALTER INDEX.  
  
## <a name="see-also"></a>Voir aussi  
[ALTER INDEX &#40;Transact-SQL&#41;](~/t-sql/statements/alter-index-transact-sql.md)  
[Réorganiser et reconstruire des index](~/relational-databases/indexes/reorganize-and-rebuild-indexes.md)  
  
