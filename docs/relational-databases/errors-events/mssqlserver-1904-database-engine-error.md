---
description: MSSQLSERVER_1904
title: MSSQLSERVER_1904 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1904 (Database Engine error)
ms.assetid: 2a35d57d-74e2-45a2-8f67-3f2e51d69712
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 198fe8af27532157397b32875882a072f26f6f4f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196481"
---
# <a name="mssqlserver_1904"></a>MSSQLSERVER_1904
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|1904|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|KEYCOUNT|  
|Texte du message|Le %S_MSG '%.*ls' sur la table '%.\*ls' a %d noms de colonnes dans la liste de clés %S_MSG. La limite maximale pour la liste des colonnes de clés d'index ou de statistiques est %d.|  
  
## <a name="explanation"></a>Explication  
La liste des colonnes de clés pour l'index ou les statistiques spécifiés dépasse le nombre maximal autorisé de colonnes.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Modifiez la liste des colonnes de clés de sorte qu'elle n'inclue pas plus de colonnes que le nombre maximal spécifié.  
  
Pour les index non cluster, pensez à utiliser la clause INCLUDE dans l'instruction CREATE INDEX pour ajouter des colonnes à l'index en tant que colonnes non clés. Cette méthode évite de dépasser la limite actuelle de taille d'index fixée à un maximum de 16 colonnes clés. Pour plus d’informations, consultez [Créer des index avec colonnes incluses](~/relational-databases/indexes/create-indexes-with-included-columns.md).  
  
## <a name="see-also"></a>Voir aussi  
[CREATE INDEX &#40;Transact-SQL&#41;](~/t-sql/statements/create-index-transact-sql.md)  
[CREATE STATISTICS &#40;Transact-SQL&#41;](~/t-sql/statements/create-statistics-transact-sql.md)  
  
