---
description: Test des objets de base de données migrés (SybaseToSQL)
title: Test des objets de base de données migrés (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 4937f6b4-86bd-4070-88df-3d216306c33a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f889538e5232eedaf3bab8006c54d56e1649ea4c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014787"
---
# <a name="testing-migrated-database-objects-sybasetosql"></a>Test des objets de base de données migrés (SybaseToSQL)
Assistant Migration Microsoft SQL Server pour Sybase tester (testeur SSMA) teste automatiquement la conversion de l’objet de base de données et la migration des données effectuée par SSMA. Une fois toutes les étapes de migration de SSMA terminées, utilisez le testeur SSMA pour vérifier que les objets convertis fonctionnent de la même façon et que toutes les données ont été correctement transférées.  
  
> [!NOTE]  
> Le composant testeur est désactivé dans le cas de la connectivité Azure.  
  
Vous pouvez tester les types d’objets suivants avec SSMA tester :  
  
-   Tables  
  
-   Procédures stockées  
  
-   Vues.  
  
-   Instructions autonomes.  
  
SSMA tester exécute les objets sélectionnés pour le test sur Sybase et leurs équivalents dans SQL Server. Après cela, il compare les résultats en fonction des critères suivants :  
  
-   Les modifications apportées aux données de la table sont-elles identiques ?  
  
-   Les valeurs des paramètres de sortie des procédures et des fonctions sont-elles identiques ?  
  
-   Les fonctions retournent-elles les mêmes résultats ?  
  
-   Les jeux de résultats sont-ils identiques ?  
  
> [!NOTE]  
> Attention ! N’utilisez jamais le testeur SSMA sur les systèmes de production. Au cours de l’exécution du testeur, le schéma source et les données sont modifiés. Pendant ce temps, la restauration complète de l’état d’origine peut être impossible pour certains types de code testé.  
  
## <a name="prerequisites"></a>Prérequis  
Si vous souhaitez utiliser SSMA tester, installez SSMA Sybase extension Pack avec l’option **installer la base de données du testeur** activée.  
  
En outre, vérifiez les éléments suivants :  
  
-   Le fournisseur Sybase OLE DB est installé sur l’ordinateur où [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute.  
  
-   L’intégration du Common Language Runtime (CLR) a été activée sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] moteur de base de données.  
  
Notez que la version actuelle de SSMA tester ne prend pas en charge l’exécution parallèle de différents utilisateurs sur le même serveur source ou cible.  
  
## <a name="getting-started"></a>Mise en route  
[Création de cas de test &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-test-cases-sybasetosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Installation des composants SSMA sur SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)  
[Paramètres du projet &#40;conversion&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-conversion-sybasetosql.md)  
  
