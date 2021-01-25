---
title: Page Options SSMS - Exécution des requêtes
description: Options SSMS (Exécution des requêtes)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Query_Execution.Sql_Server.General
dev_langs:
- TSQL
author: markingmyname
ms.author: maghan
ms.date: 01/13/2021
ms.openlocfilehash: 29ee1a365031eedae80abcffdb1147053d56f069
ms.sourcegitcommit: 23649428528346930d7d5b8be7da3dcf1a2b3190
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241770"
---
# <a name="options-query-execution---general"></a>Options (Exécution des requêtes - Général)

Cette page vous permet de spécifier les options d’exécution des requêtes Microsoft SQL Server. Pour accéder à cette boîte de dialogue, cliquez avec le bouton de droite sur le corps d’une fenêtre de l’éditeur de requête, puis sélectionnez **Options de requête** ou accédez à **Outils > Options > Exécution des requêtes** dans la barre de menus supérieure.

- **SET ROWCOUNT** La valeur par défaut 0 indique que SQL Server attend les résultats, tant que tous les résultats ne sont pas reçus. Spécifiez une valeur supérieure à 0 pour que SQL Server arrête la requête après avoir obtenu le nombre de lignes spécifié. Pour désactiver cette option, de manière à renvoyer toutes les lignes, spécifiez SET ROWCOUNT 0.

- **SET TEXTSIZE** La valeur par défaut de 2 147 483 647 octets indique que SQL Server fournira un champ de données complet jusqu'à la limite des champs de données text, ntext, nvarchar(max) et varchar(max). Cela n'affecte pas le type de données XML. Spécifiez un nombre inférieur pour limiter les résultats en cas de valeurs importantes. Les colonnes d'une taille supérieure au nombre spécifié sont tronquées.

- **Délai d’exécution** Cette zone spécifie le nombre de secondes à attendre avant d'annuler la requête. La valeur 0 indique un délai d'attente illimité ou pas de délai.

- **Délimiteur de lot** Tapez un mot à utiliser pour séparer les instructions Transact-SQL en traitements. La valeur par défaut est GO.

- **Par défaut, ouvrir les nouvelles requêtes en mode SQLCMD** Activez cette case à cocher pour ouvrir les nouvelles requêtes en mode SQLCMD. Cette case à cocher est visible seulement lorsque la boîte de dialogue est ouverte via le menu Outils.

    Lorsque vous sélectionnez cette option, prenez en compte les limitations suivantes :

    - IntelliSense dans l'éditeur de requête du moteur de base de données est désactivé.

    - L'Éditeur de requête ne s'exécutant pas à partir de la ligne de commande, vous ne pouvez pas passer de paramètres de ligne de commande tels que des variables.

    - L'Éditeur de requête étant incapable de répondre aux invites du système d'exploitation, vous devez prendre garde de ne pas exécuter d'instructions interactives.

- **Rétablir les valeurs par défaut** Rétablit toutes les valeurs par défaut initiales des options de cette page.