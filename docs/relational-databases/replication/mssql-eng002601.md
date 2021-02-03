---
description: MSSQL_ENG002601
title: MSSQL_ENG002601 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG002601 error
ms.assetid: 657c3ae6-9e4b-4c60-becc-4caf7435c1dc
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 8e507b7fc3bce13d775ab313dba6cd1af7da68ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198998"
---
# <a name="mssql_eng002601"></a>MSSQL_ENG002601
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Détails du message  
  
|Attribut|Valeur|  
|-|-|  
|Nom du produit|SQL Server|  
|ID de l’événement|2601|  
|Source de l’événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique|N/A|  
|Texte du message|Impossible d’insérer une ligne de clé en double dans l’objet '%1!' *avec un index unique '%.\*ls'.|  
  
## <a name="explanation"></a>Explication  
 C'est une erreur générale qui peut être déclenchée qu'une base de données soit répliquée ou non. Dans les bases de données répliquées, l'erreur est généralement déclenchée car les clés primaires n'ont pas été gérés de manière appropriée à travers la topologie. Dans un environnement distribué, il est primordial de s'assurer que la même valeur n'est pas insérée dans une colonne de clé primaire ou toute autre colonne unique sur plus d'un seul nœud. Les raisons peuvent être les suivantes :  
  
-   Les insertions et mises à jour sur une ligne se produisent sur plus d'un seul nœud. La réplication de fusion et les abonnements pouvant être mis à jour pour la réplication transactionnelle permettent tous deux la détection et la résolution des conflits, mais il est quand même préférable de n'insérer ou de ne mettre à jour une ligne donnée que sur un seul nœud. La réplication transactionnelle d'égal à égal ne permet pas la détection et la résolution des conflits, elle nécessite que les insertions et mises à jour soient partitionnées.  
  
-   Une ligne a été insérée sur un abonné qui devrait être en lecture seule. Les abonnés aux publications d'instantanés, ainsi que les abonnés au publications transactionnelles devraient être traités comme étant en lecture seule à moins d'utiliser des abonnements pouvant être mis à jour ou la réplication transactionnelle d'égal à égal.  
  
-   Une table avec une colonne d'identité est utilisée, mais la colonne n'est par gérée de façon appropriée.  
  
-   Dans la réplication de fusion, cette erreur peut également se produite lors d'une insertion dans une table système **MSmerge_contents**; l'erreur déclenchée est similaire à : impossible d'insérer une ligne de clé dupliquée dans l'objet « MSmerge_contents » d'index unique « ucl1SycContents ».  
  
## <a name="user-action"></a>Action de l'utilisateur  
 L'action requise dépend de la raison du déclenchement de l'erreur :  
  
-   Les insertions et mises à jour sur une ligne se produisent sur plus d'un seul nœud.  
  
     Quel que soit le type de réplication utilisé, il est recommandé de partitionner les insertions et mises à jour chaque fois que cela est possible, car cela réduit le traitement nécessaire pour la détection et la résolution des conflits. Pour la réplication transactionnelle d'égal à égal, le partitionnement d'insertions et de mises à jour est nécessaire. Pour plus d'informations, consultez [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
-   Une ligne a été insérée sur un abonné qui devrait être en lecture seule.  
  
     N'insérez pas ou ne mettez pas à jour de lignes sur l'Abonné à moins d'utiliser la réplication de fusion, la réplication transactionnelle avec abonnements pouvant être mis à jour ou la réplication transactionnelle d'égal à égal.  
  
-   Une table avec une colonne d'identité est utilisée, mais la colonne n'est par gérée de façon appropriée.  
  
     Pour la réplication de fusion et la réplication transactionnelle avec abonnements pouvant être mis à jour, les colonnes d'identité doivent être gérées automatiquement par la réplication. Pour la réplication transactionnelle d'égal à égal, elles doivent être gérées manuellement. Pour plus d’informations, consultez [ Répliquer des colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
-   L'erreur se produit pendant l'insertion dans une table système **MSmerge_contents**.  
  
     Cette erreur peut se produire en raison d'une valeur incorrecte pour la propriété du filtre de jointure **join_unique_key**. Cette propriété doit être définie avec la valeur TRUE seulement si la colonne de jointure dans la table parente est unique. Si la propriété est définie avec la valeur TRUE mais que la colonne n'est pas unique, cette erreur est déclenchée. Pour plus d'informations sur la définition de cette propriété, consultez [Définir et modifier un filtre de jointure entre des articles de fusion](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
