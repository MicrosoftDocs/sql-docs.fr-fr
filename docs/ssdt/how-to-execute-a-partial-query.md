---
title: Exécuter une requête partielle
description: Obtenez de l’aide pour le débogage d’une section d’une requête complexe. Utilisez L'éditeur Transact-SQL pour mettre en surbrillance un segment spécifique du script et l'exécuter en tant que requête unique.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: af04ab37-6cbb-4185-9382-e5922fa5b1df
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: f9aaa7712726b37d03c7d7de0994bb8abe01a7d9
ms.sourcegitcommit: b860fe41b873977649dca8c1fd5619f294c37a58
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2020
ms.locfileid: "85518799"
---
# <a name="how-to-execute-a-partial-query"></a>Procédure : Exécuter une requête partielle

L'éditeur Transact\-SQL permet de mettre en surbrillance un segment spécifique du script et de l'exécuter en tant que requête unique. Ainsi, il est plus facile pour vous de déboguer des sections de requêtes complexes.  
  
> [!WARNING]  
> Les procédures suivantes utilisent les entités créées dans les procédures précédentes des sections [Développement de base de données connectée](../ssdt/connected-database-development.md) et [Développement de base de données hors connexion orientée projet](../ssdt/project-oriented-offline-database-development.md).  
  
## <a name="to-partially-execute-a-query"></a>Pour exécuter partiellement une requête  
  
1. Dans l’**Explorateur d’objets SQL Server**, double-cliquez sur **PerishableFruits** sous **Vues** pour l’ouvrir dans l’éditeur Transact\-SQL.  
  
2. Sélectionnez le segment `SELECT p.Id, p.Name FROM dbo.Product p` dans le code, cliquez avec le bouton droit, puis sélectionnez **Exécuter la requête**.  
  
3. Notez que toutes les lignes avec les champs spécifiés dans la table `Products` sont retournées dans le volet de **résultats**.