---
title: access check cache (option de configuration de serveur) | Microsoft Docs
description: Découvrez le cache des résultats de la vérification d’accès et les options qui contrôlent le comportement du cache. Découvrez quand modifier ces options dans SQL Server.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- access check cache option
- access check cache bucket count
- access check cache quota
ms.assetid: 0a992ea8-3ec6-4a4d-97b5-460ae7326247
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dc1ff18d464c03d1eb5e96834f4d753bfead5559
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99233982"
---
# <a name="access-check-cache-server-configuration-options"></a>access check cache (option de configuration de serveur)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]accède aux objets de base de données, la vérification de l'accès est mise en cache dans une structure interne appelée le **cache de résultat de la vérification d'accès**. 
  
L’option **access check cache bucket count** contrôle le nombre de compartiments de hachage utilisés pour le cache des résultats de la vérification d’accès. 

L’option **access check cache quota** contrôle le nombre d’entrées stockées dans le cache des résultats de la vérification d’accès. Quand le nombre maximal d’entrées est atteint, les entrées les plus anciennes sont supprimées du cache des résultats de la vérification d’accès.
  
Les valeurs par défaut 0 indiquent que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gère ces options. À compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], les valeurs par défaut se traduisent par les configurations internes suivantes :
-   Pour access check cache bucket count, la valeur 0 définit une valeur par défaut de 256 compartiments.
-   Pour access check cache quota, la valeur 0 définit une valeur par défaut de 1 024 entrées.

Dans de rares circonstances, la modification de ces options peut améliorer les performances. Par exemple, vous pouvez réduire la taille du cache des résultats de la vérification d’accès si la quantité de mémoire utilisée est trop importante. Vous pouvez aussi augmenter la taille du cache des résultats de la vérification d’accès si vous constatez une utilisation intensive du processeur quand les autorisations sont recalculées.
 
> [!IMPORTANT]
> Microsoft recommande de ne modifier ces options que sur recommandation du support technique. Si vous devez modifier les valeurs de « access check cache bucket count » et de « access check cache quota », utilisez un ratio de 1:4. Par exemple, si vous changez la valeur de « access check cache bucket count » en 512, vous devez changez la valeur de « access check cache quota » en 2 048. 
  
## <a name="see-also"></a>Voir aussi  
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
