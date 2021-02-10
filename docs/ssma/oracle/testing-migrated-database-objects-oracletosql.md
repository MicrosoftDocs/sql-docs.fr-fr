---
description: Test des objets de base de données migrés (OracleToSQL)
title: Test des objets de base de données migrés (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: f03ef5e1-66e6-4c84-ada2-252dd5ada82f
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: fb85a67f90d98eadddc00aa636b6ba38ac899776
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064248"
---
# <a name="testing-migrated-database-objects-oracletosql"></a>Test des objets de base de données migrés (OracleToSQL)
[!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration pour le testeur Oracle (testeur SSMA) teste automatiquement la conversion de l’objet de base de données et la migration des données effectuée par SSMA. Une fois toutes les étapes de migration de SSMA terminées, utilisez le testeur SSMA pour vérifier que les objets convertis fonctionnent de la même façon et que toutes les données ont été correctement transférées.  
  
Vous pouvez tester les types d’objets suivants avec SSMA tester :  
  
-   Tables  
  
-   Procédures stockées, y compris les procédures empaquetées.  
  
-   Fonctions définies par l’utilisateur, y compris les fonctions empaquetées.  
  
-   Vues.  
  
-   Instructions autonomes.  
  
SSMA tester exécute les objets sélectionnés à des fins de test sur Oracle et leurs équivalents dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Après cela, il compare les résultats en fonction des critères suivants :  
  
-   Les modifications apportées aux données de la table sont-elles identiques ?  
  
-   Les valeurs des paramètres de sortie des procédures et des fonctions sont-elles identiques ?  
  
-   Les fonctions retournent-elles les mêmes résultats ?  
  
-   Les jeux de résultats sont-ils identiques ?  
  
> [!NOTE]  
> Attention ! N’utilisez jamais le testeur SSMA sur les systèmes de production. Au cours de l’exécution du testeur, le schéma source et les données sont modifiés. Pendant ce temps, la restauration complète de l’état d’origine peut être impossible pour certains types de code testé.  
  
## <a name="prerequisites"></a>Prérequis  
Si vous souhaitez utiliser SSMA tester, installez SSMA Oracle extension Pack avec l’option **installer la base de données du testeur** activée.  
  
Pour permettre la comparaison des données de la table résultante, définissez l’option **générer la colonne ROWID** sur **Oui** avant le début de la conversion du schéma. SSMA ajoute une colonne ROWID à toutes les tables lors de l’exécution de la commande **Convert Schema** .  
  
En outre, vérifiez les éléments suivants :  
  
-   Les outils clients Oracle sont installés sur l’ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute.  
  
-   L’intégration du Common Language Runtime (CLR) a été activée sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] moteur de base de données.  
  
Notez que la version actuelle de SSMA tester ne prend pas en charge l’exécution parallèle de différents utilisateurs sur le même serveur source ou cible.  
  
## <a name="getting-started"></a>Mise en route  
[Création de cas de test &#40;OracleToSQL&#41;](../../ssma/oracle/creating-test-cases-oracletosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Installation des composants SSMA sur SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/installing-ssma-components-on-sql-server-oracletosql.md)  
[Paramètres du projet &#40;conversion&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-conversion-oracletosql.md)  
  
