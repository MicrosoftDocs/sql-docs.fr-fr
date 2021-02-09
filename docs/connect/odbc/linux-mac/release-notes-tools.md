---
title: Notes de publication pour mssql-tools sur Linux et macOS
description: Découvrez les nouveautés et les évolutions apportées aux versions publiées des outils Microsoft SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
author: v-zhangw
ms.author: v-zhangw
manager: kenvh
ms.openlocfilehash: 75f1144e84792b3c9361dcab74dcdbcb7b072268
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075921"
---
# <a name="release-notes-for-the-microsoft-sql-server-tools-on-linux-and-macos"></a>Notes de publication des outils Microsoft SQL Server sur Linux et macOS

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Cet article liste et décrit les nouveautés des publications versionnées des outils [!INCLUDE[msCoName](../../../includes/msconame_md.md)] SQL Server pour sur Linux et macOS.

## <a name="17711-january-2021"></a>17.7.1.1, janvier 2021

| Fonctionnalité ajoutée | Détails |
| :------------ | :------ |
| Sqlcmd Bugfix | Correction du bogue de redirection d’entrée et des lignes vides conduisant à une exécution répétée. |
| Sqlcmd Bugfix | Correction du signalement erroné d’erreurs pour les options r, p, X et k dans certaines mises en forme. |
| Option Sqlcmd -z/-Z "Mot de passe" | Maintenant prise en charge. |
| &nbsp; | &nbsp; |

## <a name="17611-july-2020"></a>17.6.1.1, juillet 2020

| Fonctionnalité ajoutée | Détails |
| :------------ | :------ |
| Analyseur de ligne de commande sqlcmd mis à jour | Correction des bogues dans lesquels un comportement inattendu se produisait lors de l’utilisation de certaines options dans des ordres différents. |
| Messages d’erreur sqlcmd mis à jour | Correction de diverses incohérences dans la façon dont les erreurs dans sqlcmd étaient retournées. |
| Option sqlcmd -Y corrigée | Correction du problème où l’option -Y ne s’appliquait pas |
| Troncation du nom de colonne sqlcmd corrigé | Correction d’un problème où les noms de colonnes étaient tronqués de manière incorrecte |
| Codes de sortie de sqlcmd Linux | Correction du problème où le code de sortie du processus était manquant sur Linux |
| &nbsp; | &nbsp; |

## <a name="next-steps"></a>Étapes suivantes

Apprenez-en plus sur la connexion avec [BCP](connecting-with-bcp.md) et [SQLCMD](connecting-with-sqlcmd.md) !
