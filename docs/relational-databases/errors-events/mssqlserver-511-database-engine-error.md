---
description: MSSQLSERVER_511
title: MSSQLSERVER_511 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 511 (Database Engine error)
ms.assetid: 0c85686a-53c1-4180-ba8c-2000e68a0d63
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a9b0d8a76d5b45b1de5926a7ba3d3394470eeb0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185616"
---
# <a name="mssqlserver_511"></a>MSSQLSERVER_511
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|511|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|ROW_TOOBIG|  
|Texte du message|Impossible de créer une ligne de dimension %d supérieure au maximum autorisé %d.|  
  
## <a name="explanation"></a>Explication  
L'opération que vous avez tentée a dépassé la taille maximale d'une ligne. Habituellement, la taille maximale d'une ligne est 8 060 octets. Certains formats de stockage contiennent une surcharge susceptible de réduire la taille de ligne disponible pour les données. Par exemple, lorsque vous utilisez des colonnes éparses, la taille maximale d'une ligne est 8 018 octets. Certaines opérations qui ajoutent ou suppriment des lignes et certaines opérations qui modifient le type de données d'une colonne exigent que la ligne soit réécrite dans la page de données et que la ligne d'origine soit ensuite supprimée. Dans ces opérations, la limite effective de la taille de la ligne correspond à la moitié de la limite maximale. Cela est dû au fait que la ligne d'origine et la ligne modifiée doivent toutes les deux être incluses dans la page de données pour une courte période.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Si cela est possible, réduisez la taille de la ligne.  
  
Si vous pensez que le problème provient d'une mise à jour sur place de la ligne, vous devez modifier la table en plusieurs étapes. Créez une nouvelle table et transférez les données dans la nouvelle table. Ensuite, supprimez la table d'origine et renommez la nouvelle table, ou tronquez la table d'origine, modifiez les lignes dans la table d'origine, puis replacez les données dans cette table.  
  
