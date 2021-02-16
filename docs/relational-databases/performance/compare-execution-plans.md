---
title: Comparer des plans d’exécution | Microsoft Docs
description: Découvrez comment comparer les similitudes et les différences entre des plans d’exécution graphiques réels à l’aide de la fonctionnalité Comparaison de plans de SQL Server Management Studio.
ms.custom: ''
ms.date: 11/21/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- comparing execution plans
- compare execution plans
- plan comparison
- execution plans [SQL Server], comparing
- Query Store, comparing plans
ms.assetid: 9e583a18-5f4a-4054-bfe1-4b2a76630db6
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 03b6ec22a3eadf7281f0513997cfcc28d40eb1b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344736"
---
# <a name="compare-execution-plans"></a>Comparer des plans d’exécution
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Cette rubrique décrit comment comparer les similitudes et les différences entre des plans d’exécution graphiques réels à l’aide de la fonctionnalité Comparaison de plans de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Cette fonctionnalité est disponible à partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v16.
  
> [!NOTE]
> Les plans d’exécution réels sont générés une fois que les requêtes ou les lots [!INCLUDE[tsql](../../includes/tsql-md.md)] ont été exécutés. Pour cette raison, un plan d’exécution réel contient des informations d’exécution, comme la quantité réelle de lignes, les avertissements d’exécution (s’il y en a) et les métriques d’utilisation des ressources. Pour plus d’informations, consultez [Afficher un plan d’exécution réel](../../relational-databases/performance/display-an-actual-execution-plan.md).
  
Comparer des plans est une tâche que les professionnels des bases de données peuvent avoir besoin d’effectuer à des fins de dépannage, notamment pour :
-   Rechercher pourquoi une requête ou un lot a subitement ralenti.
-   Comprendre l’impact d’une réécriture de requête.
-   Observer la manière dont un changement spécifique, visant à améliorer les performances, introduit dans la conception du schéma (par exemple, un nouvel index) a modifié le plan d’exécution.  
 
L’option de menu **Comparaison de plans** permet d’effectuer une comparaison côte à côte de deux plans d’exécution, afin de faciliter l’identification des similitudes et des changements qui expliquent les différents comportements pour toutes les raisons susmentionnées. Cette option peut comparer :
- Deux fichiers de plan d’exécution précédemment enregistrés (extension *.sqlplan*).
- Un plan d’exécution actif et un plan d’exécution de requête précédemment enregistré.
- Deux plans de requête sélectionnés dans le [Magasin des requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).

> [!TIP]
> La comparaison de plans fonctionne avec n’importe quel fichier *.sqlplan*, même ceux des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette option permet aussi d’effectuer une comparaison hors connexion. Il n’est donc pas nécessaire d’être connecté à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

Quand deux plans d’exécution sont comparés, les régions du plan qui **font essentiellement la même chose** sont mises en surbrillance avec la même couleur et le même modèle. Un clic sur une région colorée dans un plan permet de centrer l’autre plan sur le nœud correspondant dans ce plan. Vous pouvez toujours comparer des opérateurs et des nœuds des plans d’exécution sans correspondance, mais dans ce cas vous devez sélectionner manuellement les opérateurs à comparer.

> [!IMPORTANT]
> Seuls les nœuds considérés comme changeant la forme du plan servent à vérifier les similitudes. Ainsi, il peut y avoir un nœud non coloré entre deux nœuds qui se trouvent dans la même sous-section du plan. Dans ce cas, l’absence de couleur implique que les nœuds n’ont pas été pris en compte lors de la vérification de l’égalité des sections.
  
## <a name="to-compare-execution-plans"></a>Pour comparer des plans d’exécution
  
1.  Ouvrez un fichier de plan d’exécution de requête précédemment enregistré (.sqlplan) en cliquant sur **Ouvrir un fichier** dans le menu **Fichier**, ou en faisant glisser un fichier de plan vers la fenêtre [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]. En guise d’alternative, si vous venez d’exécuter une requête et que vous avez choisi d’afficher son plan d’exécution, accédez à l’onglet **Plan d’exécution** dans le volet de résultats. 

2.  Cliquez avec le bouton droit dans une zone vide du plan d’exécution et cliquez sur **Comparer un plan d’exécution de requêtes**. 

    ![Cliquez avec le bouton droit sur Comparer le plan d'exécution de requêtes](../../relational-databases/performance/media/plancomparisonmenuoption.png "Cliquez avec le bouton droit sur Comparer le plan d'exécution de requêtes")   

3.  Choisissez le deuxième fichier de plan de requête avec lequel vous souhaitez effectuer une comparaison. Le deuxième fichier s’ouvrira afin que vous puissiez comparer les plans.

4.  Les plans comparés s’ouvriront dans une nouvelle fenêtre, par défaut l’un dans la partie supérieure et l’autre dans la partie inférieure. La sélection par défaut sera la première occurrence d’un opérateur ou d’un nœud qui est commune dans les plans comparés, mais avec affichage des différences entre les plans. Tous les opérateurs et les nœuds mis en surbrillance existent dans les deux plans comparés. La sélection d’un opérateur en surbrillance dans le plan du haut ou de gauche sélectionne automatiquement l’opérateur correspondant dans le plan du bas ou de droite. La sélection de l’opérateur de nœud racine dans l’un des plans comparés (le nœud SELECT dans l’image ci-dessous) sélectionne également l’opérateur de nœud racine respectif dans l’autre plan comparé.

    ![Comparaison du plan de deux fichiers de plan enregistrés](../../relational-databases/performance/media/plancomparison-plans.png "Comparaison du plan de deux fichiers de plan enregistrés")  

     > [!TIP]
     > Vous pouvez choisir un affichage côte à côte de la comparaison des plans d’exécution en cliquant avec le bouton droit sur une zone vide du plan d’exécution et en sélectionnant **Basculer l’orientation du fractionnement**.

     > [!TIP]
     > Toutes les options de zoom et de navigation disponibles pour les plans d’exécution fonctionnent en mode de comparaison de plans. Pour plus d’informations, consultez [Afficher un plan d’exécution réel](../../relational-databases/performance/display-an-actual-execution-plan.md).

5.  Une double fenêtre de propriétés s’ouvre également sur le côté droit, dans la portée de la sélection par défaut. Les propriétés qui existent dans les deux opérateurs comparés mais qui présentent des différences sont précédées du signe *différent* (&ne;) afin de faciliter leur identification.

    ![Fenêtre Propriétés double](../../relational-databases/performance/media/plancomparison-properties.png "Fenêtre Propriétés double")  

6.  La fenêtre de comparaison **Analyse du plan d’exécution de requêtes** s’ouvre également en bas. Trois onglets sont disponibles :

    1.  Sous l’onglet **Options d’instruction**, la sélection par défaut est *Mettre en évidence les opérations similaires*, et les mêmes opérateurs ou nœuds en surbrillance dans les plans comparés partagent le même modèle de ligne et la même couleur. Naviguez entre les zones similaires dans les plans comparés en cliquant sur un modèle de ligne. Vous pouvez également choisir de mettre en évidence les différences dans les plans (au lieu des similitudes) en sélectionnant *Mettre en évidence les opérateurs qui ne correspondent pas à des segments similaires*. 
    
       > [!NOTE]
       > Par défaut, les noms des bases de données sont ignorés lors de la comparaison des plans, afin d’autoriser la comparaison des plans capturés pour les bases de données qui ont des noms différents mais qui partagent le même schéma. Par exemple, lors de la comparaison des plans des bases de données *ProdDB* et *TestDB*. Vous pouvez changer ce comportement avec l’option *Ignorer le nom de la base de données lors de la comparaison des opérateurs*.

       ![Fenêtre d'analyse du plan d'exécution de requêtes](../../relational-databases/performance/media/plancomparison-analysis.png "Fenêtre d'analyse du plan d'exécution de requêtes") 

    2.  L’onglet **Instructions multiples** est utile lors de la comparaison de plans comprenant plusieurs instructions, car il permet de comparer la bonne paire d’instructions.

        ![Plusieurs instructions dans un plan comparé](../../relational-databases/performance/media/plancomparison-multiple.png "Plusieurs instructions dans un plan comparé")  

    3.  Sous l’onglet **Scénarios** figure une analyse automatisée de certains des aspects les plus pertinents à examiner en ce qui concerne les différences d’[estimation de cardinalité](../../relational-databases/performance/cardinality-estimation-sql-server.md) dans les plans comparés. Pour chaque opérateur listé dans le volet gauche, le volet droit affiche des détails sur le scénario dans le lien *Cliquez ici pour plus d’informations sur ce scénario*, et les raisons possibles pouvant expliquer ce scénario sont listées. 

        ![Lignes estimées différentes](../../relational-databases/performance/media/plancomparison-scenarios.png "Lignes estimées différentes")  

    Si cette fenêtre est fermée, cliquez avec le bouton droit sur une zone vide d’un plan comparé, puis sélectionnez **Options de comparaison de plans d’exécution de requête** pour la rouvrir.

    ![Options de comparaison de plan](../../relational-databases/performance/media/plancomparison-options.png "Options de comparaison de plan")  

## <a name="to-compare-execution-plans-in-query-store"></a>Pour comparer des plans d’exécution dans le Magasin des requêtes

1.  Dans le Magasin des requêtes, identifiez une requête qui a plus d’un plan d’exécution. Pour plus d’informations sur les scénarios de Magasin des requêtes, consultez [Scénarios d’utilisation du Magasin des requêtes](../../relational-databases/performance/query-store-usage-scenarios.md#identify-and-tune-top-resource-consuming-queries).

2.  Utilisez la touche Maj et votre souris pour sélectionner deux plans pour la même requête. 

    ![Sélectionner deux plans dans le Magasin des requêtes](../../relational-databases/performance/media/plancomparison-querystore.png "Sélectionner deux plans dans le Magasin des requêtes")   

3.  Utilisez le bouton **Comparer les plans pour la requête sélectionnée dans une fenêtre distincte** pour commencer la comparaison des plans. Ensuite, les étapes 4 à 6 de *Pour comparer des plans d’exécution* sont applicables. 

    ![Comparer le plan d'exécution de requêtes dans le Magasin des requêtes](../../relational-databases/performance/media/plancomparison-querystoreoption.png "Comparer le plan d'exécution de requêtes dans le Magasin des requêtes") 
