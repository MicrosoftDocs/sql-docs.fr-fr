---
title: Nombres à virgule flottante
description: Découvrez quelques-uns des problèmes rencontrés lors de l’utilisation de nombres à virgule flottante dans le Fournisseur de données Microsoft SqlClient pour SQL Server.
ms.date: 11/13/2020
ms.assetid: 73c218c6-1c44-4402-a167-4f6262629a91
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1ca251c6de5ca6157ace8ee481bae02afa9fb2fb
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771559"
---
# <a name="floating-point-numbers"></a>Nombres à virgule flottante

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Cette rubrique décrit certains des problèmes que les développeurs rencontrent fréquemment lorsqu’ils travaillent avec des nombres à virgule flottante dans le Fournisseur de données Microsoft SqlClient pour SQL Server. Ces problèmes proviennent du mode de stockage des nombres à virgule flottante par les ordinateurs et ne sont pas propres à un fournisseur particulier, tel que <xref:Microsoft.Data.SqlClient>.

## <a name="floating-point-pitfall"></a>Piège à virgule flottante

Généralement, les nombres à virgule flottante n'ont pas de représentation binaire exacte. L'ordinateur stocke à la place une approximation du nombre. À différents moments, différents nombres en chiffres binaires peuvent être utilisés pour représenter le même nombre. Lorsqu'un nombre à virgule flottante est converti d'une représentation à une autre, ses chiffres les moins significatifs peuvent varier légèrement. Généralement, une conversion se produit lorsqu'un nombre est casté d'un type à un autre. La variation survient que la conversion ait lieu dans une base de données, entre des types représentant des valeurs de base de données ou entre des types. En raison de ces modifications, des nombres qui devraient logiquement être égaux peuvent présenter des disparités au niveau de leurs chiffres les moins significatifs et par conséquent avoir des valeurs différentes. Le nombre de chiffres de précision peut être plus grand ou plus petit que prévu pour un nombre. Lorsqu'il est sous forme de chaîne, le nombre risque de ne pas présenter la valeur prévue.

## <a name="suggested-workarounds"></a>Solutions de contournement conseillées

Pour minimiser ces effets, vous devez choisir parmi les différents types numériques disponibles celui qui se rapproche le plus de la valeur d'origine. Par exemple, si vous utilisez SQL Server, la valeur numérique exacte peut changer si vous convertissez une valeur Transact-SQL de type réel en valeur de type float. Dans le .NET, la conversion d’un <xref:System.Single> en <xref:System.Double> peut également entraîner des résultats inattendus. Dans ces deux cas, une stratégie efficace consiste à appliquer le même type numérique à toutes les valeurs de l'application. Vous pouvez également recourir à un type décimal à précision fixe, ou convertir des nombres à virgule flottante en type décimal à précision fixe avant de les utiliser.

Pour contourner les problèmes de comparaison d'égalité, vous pouvez coder l'application de manière à ce que les variations des chiffres les moins significatifs soient ignorées. Par exemple, au lieu de comparer deux nombres pour vérifier s'ils sont égaux, vous pouvez les soustraire. Si la différence est comprise dans une fourchette d'arrondi acceptable, l'application peut traiter ces nombres comme s'ils étaient identiques.

## <a name="see-also"></a>Voir aussi

- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
