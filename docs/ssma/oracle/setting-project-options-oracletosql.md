---
description: Définition des options du projet (OracleToSQL)
title: Définition des options du projet (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Configuration Options and Modes
ms.assetid: a324d07d-cfdf-43bd-98a0-acf332c5a4db
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 7de2ab3447a35f9d55489673adaa532866a4f89f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067674"
---
# <a name="setting-project-options-oracletosql"></a>Définition des options du projet (OracleToSQL)
Pour chaque projet SSMA, vous pouvez définir des options au niveau du projet. Ces options spécifient la conversion d’objet, le chargement d’objet, l’interface utilisateur et les paramètres de migration de données. Avant de convertir des objets vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou de migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vérifiez que les options de configuration sont appropriées pour le projet.  
  
SSMA vous permet de configurer les options par défaut pour tous les projets. Ces options sont appliquées à tous les nouveaux projets que vous créez. Vous pouvez ensuite personnaliser les options pour chaque projet.  
  
## <a name="configuration-options-and-modes"></a>Options de configuration et modes  
SSMA comporte cinq jeux de paramètres de projet :  
  
-   Informations sur le projet  
  
-   Général (conversion, migration, chargement d’objets)  
  
-   Synchronization  
  
-   Interface graphique utilisateur  
  
-   Mappage de type  
  
Il comporte également quatre modes de configuration de ces paramètres :  
  
-   Default  
  
-   Optimistic  
  
-   Complète  
  
-   Personnalisé  
  
Le mode par défaut est recommandé pour la plupart des utilisateurs. Le mode optimiste conserve davantage la syntaxe Oracle actuelle et est plus facile à lire. Toutefois, il se peut que la conservation de la syntaxe actuelle ne soit pas exacte. Si la syntaxe Oracle doit être convertie en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] une syntaxe équivalente, le mode complet effectue la conversion la plus complète, mais le code obtenu peut être plus difficile à lire. Dans le mode personnalisé, vous définissez les options.  
  
Pour plus d’informations sur les paramètres et sur la façon dont les paramètres sont appliqués dans chaque mode, consultez les rubriques suivantes :  
  
-   [Paramètres du projet &#40;conversion&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-conversion-oracletosql.md)  
  
-   [Paramètres du projet &#40;&#41; &#40;de migration OracleToSQL&#41;](../../ssma/oracle/project-settings-migration-oracletosql.md)  
  
-   [Paramètres du projet&#40;synchronisation&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-synchronization-oracletosql.md)  
  
-   [Paramètres du projet &#40;GUI&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-gui-oracletosql.md)  
  
-   [Paramètres du projet &#40;le mappage de type&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-type-mapping-oracletosql.md)  
  
## <a name="setting-project-options"></a>Définition des options du projet  
Dans SSMA, vous pouvez configurer des paramètres par défaut pour tous les projets. Ces paramètres sont enregistrés dans le fichier de configuration SSMA et appliqués à tous les nouveaux projets que vous créez.  
  
**Pour définir les options de projet par défaut**  
  
1.  Dans le menu **Outils** , cliquez sur **paramètres de projet par défaut**.  
  
2.  Dans la boîte de dialogue **paramètres du projet par défaut** , utilisez l’une des procédures suivantes :  
  
    -   Sélectionnez le type de projet de migration pour lequel les paramètres doivent être affichés ou modifiés dans la liste déroulante **version cible** de la migration cliquez sur **général** en bas du volet gauche, puis sélectionnez conversion ou migration.  
  
    -   Pour sélectionner un mode prédéfini, dans la zone de liste déroulante **mode** , sélectionnez **par défaut**, **optimiste** ou **complète**.  
  
    -   Pour spécifier des paramètres personnalisés, sélectionnez ou entrez les nouveaux paramètres ou valeurs.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
Vous pouvez également personnaliser les paramètres du projet actuel. Ces paramètres sont enregistrés dans le fichier projet actif.  
  
**Pour personnaliser les paramètres du projet actif**  
  
1.  Dans le menu **Outils** , cliquez sur **paramètres du projet**.  
  
2.  Dans la boîte de dialogue **paramètres du projet** , utilisez l’une des procédures suivantes :  
  
    -   Pour sélectionner un mode prédéfini, dans la zone de liste déroulante **mode** , sélectionnez **par défaut**, **optimiste** ou **complète**.  
  
    -   Pour spécifier un mode personnalisé, dans la zone **mode** , sélectionnez **personnalisé**, puis sélectionnez les paramètres de projet appropriés.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante de la migration dépend des besoins de votre projet :  
  
-   Pour personnaliser le mappage des types de données sources et cibles, consultez [mappage des types de données Oracle et SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/mapping-oracle-and-sql-server-data-types-oracletosql.md).  
  
-   Dans le cas contraire, vous pouvez convertir les définitions d’objet de base de données Oracle en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définitions d’objets. Pour plus d’informations, consultez [conversion de schémas Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/converting-oracle-schemas-oracletosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Mappage des types de données Oracle et SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/mapping-oracle-and-sql-server-data-types-oracletosql.md)  
  
