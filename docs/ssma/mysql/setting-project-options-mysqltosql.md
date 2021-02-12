---
description: Définition des options du projet (MySQLToSQL)
title: Définition des options du projet (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Setting project options,configuration options
ms.assetid: 08820d88-e157-4d49-9401-38580dd7ec2d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 75c4f6ca899135141aa68ec147889fcce8cfa549
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074510"
---
# <a name="setting-project-options-mysqltosql"></a>Définition des options du projet (MySQLToSQL)
Pour chaque projet SSMA, vous pouvez définir des options au niveau du projet. Ces options spécifient la façon dont les objets sont convertis, la façon dont les données sont migrées et la manière dont les types de données sources sont mappés aux types de données cibles.  Avant de convertir des objets en SQL Server ou SQL Azure ou de migrer des données vers SQL Server ou SQL Azure, vérifiez que les options de configuration sont appropriées pour le projet.  
  
SSMA vous permet de configurer les options par défaut pour tous les projets. Ces options sont appliquées à tous les nouveaux projets que vous créez. Vous pouvez ensuite personnaliser les options pour chaque projet.  
  
## <a name="configuration-options-and-modes"></a>Options de configuration et modes  
SSMA comporte cinq jeux de paramètres de projet :  
  
-   Informations sur le projet  
  
-   Général (conversion, migration et SQL Azure)  
  
-   Synchronization  
  
-   Interface graphique utilisateur  
  
-   Mappage de type  
  
Les paramètres du projet peuvent être configurés de quatre façons :  
  
-   Default  
  
-   Optimistic  
  
-   Complète  
  
-   Personnalisé  
  
Le mode par défaut est recommandé pour la plupart des utilisateurs. Le mode optimiste conserve la syntaxe MySQL actuelle et est plus facile à lire. Toutefois, il se peut que la conservation de la syntaxe actuelle ne soit pas exacte. Si la syntaxe MySQL doit être convertie en SQL Server ou SQL Azure syntaxe équivalente, le mode complet effectue la conversion la plus complète. Toutefois, le code résultant peut être plus difficile à lire. Dans le mode personnalisé, vous pouvez définir les options.  
  
Pour plus d’informations sur les paramètres et sur la façon dont les paramètres sont appliqués dans chaque mode, consultez les rubriques suivantes :  
  
-   [Paramètres du projet &#40;conversion&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-conversion-mysqltosql.md)  
  
-   [Paramètres du projet &#40;&#41; &#40;de migration MySQLToSQL&#41;](../../ssma/mysql/project-settings-migration-mysqltosql.md)  
  
-   [Paramètres du projet (GUI) (SSMA commun)](../sybase/project-settings-gui-sybasetosql.md)  
  
-   [Paramètres du projet &#40;le mappage de type&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-type-mapping-mysqltosql.md)  
  
-   [Paramètres du projet &#40;synchronisation&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-synchronization-mysqltosql.md)  
  
-   [Paramètres du projet &#40;Azure SQL Database&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-azure-sql-db-mysqltosql.md)  
  
## <a name="setting-project-options"></a>Définition des options du projet  
Dans SSMA, vous pouvez configurer des paramètres par défaut pour tous les projets. Ces paramètres sont enregistrés dans le fichier de configuration SSMA et appliqués à tous les nouveaux projets que vous créez.  
  
**Pour définir les options de projet par défaut**  
  
1.  Dans le menu **Outils** , cliquez sur **paramètres de projet par défaut**.  
  
2.  Dans la boîte de dialogue **paramètres du projet par défaut** , utilisez l’une des procédures suivantes :  
  
    1.  Sélectionnez le type de projet de migration pour lequel les paramètres doivent être affichés/modifiés dans la liste déroulante de la **version cible** de la migration, cliquez sur **général** en bas du volet gauche, puis sélectionnez option **conversion ou migration ou SQL Azure** .  
  
    2.  Pour sélectionner un mode prédéfini, sélectionnez **par défaut**, **optimiste** ou **complète** dans la zone de liste déroulante **mode** .  
  
    3.  Pour spécifier des paramètres personnalisés, sélectionnez ou entrez les nouveaux paramètres ou valeurs.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
Vous pouvez également personnaliser les paramètres du projet actuel. Les paramètres sont enregistrés dans le fichier projet actuel.  
  
**Pour personnaliser les paramètres du projet actif**  
  
1.  Dans le menu **Outils** , cliquez sur **ProjectSettings**.  
  
2.  Dans la boîte de dialogue **ProjectSettings** , utilisez l’une des procédures suivantes :  
  
    1.  Pour sélectionner un mode prédéfini, sélectionnez **par défaut**, **optimiste** ou **complète** dans la zone de liste déroulante **mode** .  
  
    2.  Pour spécifier un mode personnalisé, sélectionnez **personnalisé** dans la zone de liste déroulante **mode** . Puis sélectionnez les paramètres de projet appropriés.  
  
3.  Cliquez sur **OK** pour enregistrer les paramètres.  
  
## <a name="next-step"></a>étape suivante  
L’étape suivante de la migration dépend des besoins de votre projet :  
  
-   Pour personnaliser le mappage des types de données sources et cibles, consultez [mappage des types de données MySQL et SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)  
  
-   Dans le cas contraire, vous pouvez convertir les définitions d’objet de base de données MySQL en SQL Server ou SQL Azure définitions d’objets. Pour plus d’informations, consultez [conversion de bases de données MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Mappage de types de données MySQL et SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)  
