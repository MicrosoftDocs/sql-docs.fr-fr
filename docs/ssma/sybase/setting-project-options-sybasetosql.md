---
description: Définition des options du projet (SybaseToSQL)
title: Définition des options du projet (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Project Options Setting
ms.assetid: 97b70fc8-1f68-4f15-8e22-db5b784ea4ec
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: fa8e45af93faa4ef175d129487a1d655a6d02ead
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030757"
---
# <a name="setting-project-options-sybasetosql"></a>Définition des options du projet (SybaseToSQL)
Pour chaque projet SSMA, vous pouvez définir des options au niveau du projet. Ces options spécifient la conversion d’objet, le chargement d’objet, SQL Azure, l’interface utilisateur et les paramètres de migration de données. Avant de convertir des objets en SQL Azure ou de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] migrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des données vers ou SQL Azure, vérifiez que les options de configuration sont appropriées pour le projet.  
  
SSMA vous permet de configurer les options par défaut pour tous les projets. Ces options sont appliquées à tous les nouveaux projets que vous créez. Vous pouvez ensuite personnaliser les options pour chaque projet.  
  
## <a name="configuration-options-and-modes"></a>Options de configuration et modes  
SSMA comporte cinq jeux de paramètres de projet :  
  
1.  Informations sur le projet  
  
2.  Général (conversion, migration et collecte des données)  
  
3.  Synchronization  
  
4.  Interface graphique utilisateur  
  
5.  Mappage de type  
  
Il comporte également quatre modes de configuration de ces paramètres :  
  
1.  Default  
  
2.  Optimistic  
  
3.  Complète  
  
4.  Personnalisé  
  
Le mode par défaut est recommandé pour la plupart des utilisateurs. Le mode optimiste conserve davantage la syntaxe Sybase Adaptive Server Enterprise (ASE) actuelle et est plus facile à lire. Toutefois, il se peut que la conservation de la syntaxe actuelle ne soit pas exacte. Si la syntaxe de l’ASE doit être convertie en syntaxe équivalente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, le mode complet effectue une conversion complète, mais le code obtenu peut être plus difficile à lire. Dans le mode personnalisé, vous définissez les options.  
  
Les paramètres sont décrits dans la section Référence de l’interface utilisateur de cette documentation. Pour plus d’informations sur les paramètres et sur la façon dont les paramètres sont appliqués dans chaque mode, consultez les rubriques suivantes :  
  
-   [Paramètres du projet &#40;conversion&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-conversion-sybasetosql.md)  
  
-   [Paramètres du projet &#40;&#41; &#40;de migration SybaseToSQL&#41;](../../ssma/sybase/project-settings-migration-sybasetosql.md)  
  
-   [Paramètres du projet &#40;synchronisation&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-synchronization-sybasetosql.md)  
  
-   [Paramètres du projet &#40;GUI&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-gui-sybasetosql.md)  
  
-   [Paramètres du projet &#40;le mappage de type&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-type-mapping-sybasetosql.md)  
  
-   [Paramètres du projet &#40;Azure SQL Database &#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-azure-sql-db-sybasetosql.md)  
  
## <a name="setting-project-options"></a>Définition des options du projet  
Dans SSMA, vous pouvez configurer des paramètres par défaut pour tous les projets. Ces paramètres sont enregistrés dans le fichier de configuration SSMA et appliqués à tous les nouveaux projets que vous créez.  
  
**Pour définir les options de projet par défaut**  
  
1.  Dans le menu **Outils** , sélectionnez **paramètres du projet par défaut**.  
  
2.  Dans la boîte de dialogue **paramètres du projet par défaut** , utilisez l’une des procédures suivantes :  
  
    -   Sélectionnez le type de projet de migration pour lequel les paramètres doivent être affichés ou modifiés dans la liste déroulante **version cible** de la migration cliquez sur général en bas du volet gauche, puis sélectionnez conversion ou migration ou SQL Azure.  
  
    -   Pour sélectionner un mode prédéfini, dans la zone de liste déroulante **mode** , sélectionnez **par défaut**, **optimiste** ou **complète**.  
  
    -   Pour spécifier des paramètres personnalisés, il vous suffit de sélectionner ou d’entrer les nouveaux paramètres ou valeurs.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
Vous pouvez également personnaliser les paramètres du projet actuel. Ces paramètres sont enregistrés dans le fichier projet actif.  
  
**Pour personnaliser les paramètres du projet actif**  
  
1.  Dans le menu **Outils** , sélectionnez **paramètres du projet**.  
  
2.  Dans la boîte de dialogue **paramètres du projet** , utilisez l’une des procédures suivantes :  
  
    -   Pour sélectionner un mode prédéfini, dans la zone de liste déroulante **mode** , sélectionnez **par défaut**, **optimiste** ou **complète**.  
  
    -   Pour spécifier un mode personnalisé, dans la zone déroulante **mode** , sélectionnez **personnalisé**, sélectionnez une option dans le volet gauche, cliquez sur le paramètre ou la valeur dans le volet droit, puis sélectionnez ou entrez le nouveau paramètre ou la nouvelle valeur.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante de la migration dépend des besoins de votre projet :  
  
-   Si vous souhaitez personnaliser le mappage des types de données sources et cibles, consultez [mappage des types de données Sybase ASE et SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/mapping-sybase-ase-and-sql-server-data-types-sybasetosql.md).  
  
-   Dans le cas contraire, vous pouvez convertir les définitions d’objet de base de données Sybase en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL Azure définitions d’objets ou. Pour plus d’informations, consultez [conversion d’objets de base de données Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données Sybase ASE vers SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
