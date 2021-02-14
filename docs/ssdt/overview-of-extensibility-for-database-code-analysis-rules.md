---
title: Extensibilité pour les règles d'analyse du code de base de données
description: Familiarisez-vous avec les divers composants des règles d’analyse du code de base de données et la façon dont elles interagissent dans SQL Server Data Tools. Découvrez comment créer des règles personnalisées.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 62f5c980-18d5-43fe-b443-c9e149d01fc7
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 24b0224e21f6acec7cc346d35935b0fb0ea692dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080820"
---
# <a name="overview-of-extensibility-for-database-code-analysis-rules"></a>Vue d'ensemble de l'extensibilité pour les règles d'analyse du code de base de données

Les éditions de Visual Studio contenant SQL Server Data Tools comportent des règles d’analyse du code permettant de générer des rapports sur les avertissements de conception, d’affectation de noms et de performances de Transact\-SQL dans le code de la base de données. Pour plus d’informations, voir [Analyser le code de la base de données pour améliorer la qualité du code](/previous-versions/visualstudio/visual-studio-2010/dd172133(v=vs.100)).  
  
Si les règles d’analyse du code intégrées ne couvrent pas un problème Transact\-SQL que vous souhaitez inclure, vous pouvez créer des règles personnalisées d’analyse du code de base de données. Vous pouvez, par exemple, créer une règle personnalisée qui évite d’utiliser l’instruction WAITFOR DELAY, comme dans [Procédure de création d’un assembly de règle personnalisée d’analyse du code statique pour SQL Server](../ssdt/walkthrough-author-custom-static-code-analysis-rule-assembly.md). Pour créer des règles personnalisées d’analyse du code de base de données, utilisez les classes de l’espace de noms [CodeAnalysis](/dotnet/api/microsoft.sqlserver.dac.codeanalysis).  
  
Avant de créer des règles d'analyse du code personnalisées, vous devez comprendre l'architecture de base des différents composants des règles d'analyse du code de base de données.  
  
## <a name="database-code-analysis-rules-components"></a>Composants des règles d'analyse du code de base de données  
Le diagramme suivant illustre l'interaction entre les composants de règles d'analyse du code de base de données :  
  
![Composants des règles d'analyse du code de base de données](../ssdt/media/ssdt-database-code-analysis-rules-components.jpg "Composants des règles d'analyse du code de base de données")  
  
Quand vous utilisez la fonctionnalité d’analyse du code de base de données, soit en exécutant directement l’analyse du code statique (pour plus d’informations, voir [Guide pratique : Analyser du code Transact-SQL afin de détecter des défauts](/previous-versions/visualstudio/visual-studio-2010/dd172119(v=vs.100))), soit en effectuant une génération, toutes les règles sont chargées et utilisées selon la manière dont vous les avez configurées dans votre projet. Pour plus d’informations, consultez [Procédure : activer et désactiver des règles spécifiques pour l’analyse statique du code d’une base de données](/previous-versions/visualstudio/visual-studio-2010/dd172131(v=vs.100)). Le Gestionnaire d'extensions charge également les éventuels assemblys de règles personnalisées que vous avez créés et enregistrés. Pour plus d’informations, consultez [Procédure : installer et gérer des extensions de fonctionnalités](../ssdt/how-to-install-and-manage-feature-extensions.md).  
  
Une classe de règle personnalisée d’analyse du code hérite de [SqlCodeAnalysisRule](/dotnet/api/microsoft.sqlserver.dac.codeanalysis.sqlcodeanalysisrule). La classe de règle personnalisée peut accéder à plusieurs objets utiles via son contexte d'exécution de règle, notamment :  
  
-   Des métadonnées relatives à la règle elle-même.  
  
-   Le modèle Dac.Model.TSqlModel représentant le schéma de la base de données, y compris tous les éléments de modèle, les relations qu’ils entretiennent et toutes les propriétés des éléments.  
  
-   Pour les règles qui examinent des éléments spécifiques, l’objet Dac.Model.TSqlObject représentant cet élément de schéma dans le modèle est inclus dans le contexte.  
  
-   De nombreux objets de schéma ont également une représentation [ScriptDom](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom) accessible par le biais de ce contexte. Il s’agit d’une représentation AST d’un élément qui peut être utile pour voir les éventuels problèmes de syntaxe, comme la présence de [SelectStarExpression](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.selectstarexpression).  
  
Un élément Dac.CodeAnalysis.SqlRuleProblem est créé par la règle pour représenter tous les problèmes qu’elle a détectés. Lors de cette création, l’objet Dac.Model.TSqlObject correspondant et éventuellement un élément de représentation [ScriptDom](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom) sont passés au constructeur et utilisés pour déterminer l’emplacement du problème dans les fichiers de code source. À la fin de l'analyse, tous ces problèmes sont transmis au gestionnaire d'erreurs et affichés dans la liste d'erreurs.  
  
## <a name="see-also"></a>Voir aussi  
[Extension des fonctionnalités de base de données](../ssdt/extending-the-database-features.md)  
