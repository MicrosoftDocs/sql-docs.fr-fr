---
title: 'Tutoriel : Database Engine Tuning Advisor'
description: L’Assistant Paramétrage du moteur de base de données examine la manière dont les requêtes sont traitées et recommande comment améliorer les performances de traitement des requêtes en modifiant les structures de base de données.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- Database Engine Tuning Advisor [SQL Server], tutorials
- tutorials [Database Engine Tuning Advisor]
ms.assetid: 3b54cbbe-d8c6-424d-92f1-aa58179f4da8
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 4f1de68f73e54af6e50672f78b0a8c90999a4115
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353419"
---
# <a name="tutorial-database-engine-tuning-advisor"></a>Tutoriel : Database Engine Tuning Advisor

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Bienvenue dans le Didacticiel Assistant Paramétrage du moteur de base de données. Cet outil examine la façon dont les requêtes sont traitées dans les bases de données que vous spécifiez, puis il recommande des moyens d'améliorer les performances du traitement des requêtes en modifiant les structures des bases de données telles que les index, les vues indexées et le partitionnement.  
  
L’Assistant Paramétrage du moteur de base de données fournit deux interfaces utilisateur : une interface utilisateur graphique (GUI) et l’utilitaire en ligne de commande **dta** . L’interface utilisateur graphique facilite l’affichage des résultats des sessions de paramétrage, tandis que l’utilitaire en ligne de commande **dta** facilite l’intégration de la fonctionnalité Assistant Paramétrage du moteur de base de données aux scripts pour automatiser le paramétrage. De plus, l'Assistant Paramétrage du moteur de base de données peut accepter les entrées XML, ce qui permet de mieux contrôler la procédure de paramétrage.  
  
## <a name="what-you-will-learn"></a>Contenu du didacticiel  
Dans ce didacticiel, vous allez apprendre à vous déplacer dans l’interface utilisateur graphique de l’Assistant Paramétrage du moteur de base de données et également à effectuer des tâches de base en utilisant à la fois l’interface graphique et l’utilitaire **dta** . Le didacticiel comprend les leçons suivantes :  
  
[Leçon 1 : Navigation de base dans l’Assistant Paramétrage du moteur de base de données](../../tools/dta/lesson-1-basic-navigation-in-database-engine-tuning-advisor.md)  
Au cours de cette leçon, vous allez vous familiariser avec la nouvelle interface de l'Assistant Paramétrage du moteur de base de données et apprendrez comment définir les options d'affichage et la disposition.  
  
[Leçon 2 : Utilisation de l'Assistant Paramétrage du moteur de base de données](../../tools/dta/lesson-2-using-database-engine-tuning-advisor.md)  
Au cours de cette leçon, vous allez apprendre à effectuer des tâches de paramétrage de base en utilisant l'interface utilisateur de l'Assistant Paramétrage du moteur de base de données.  
  
[Leçon 3 : Utilisation de l’utilitaire de ligne de commande dta](../../tools/dta/lesson-3-using-the-dta-command-prompt-utility.md)  
Au cours de cette leçon, vous allez apprendre à démarrer l’utilitaire en ligne de commande **dta** et à exécuter des commandes de paramétrage simples.  
  
## <a name="requirements"></a>Spécifications  
Ce didacticiel s’adresse aux administrateurs de base de données qui ne connaissent pas l’interface utilisateur de l’Assistant Paramétrage du moteur de base de données ou l’utilitaire en ligne de commande **dta** , mais qui sont des administrateurs de base de données expérimentés et qui maîtrisent les concepts et les structures propres aux bases de données comme les index et les vues indexées.  
  
Vous devez installer [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] avec l’exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Pour des raisons de sécurité, les exemples de bases de données ne sont pas installés par défaut. Pour installer les exemples de bases de données, consultez [Installation des exemples SQL Server et des exemples de bases de données](https://sqlserversamples.codeplex.com).  
  
## <a name="after-you-finish-this-tutorial"></a>Fin du didacticiel  
Une fois les leçons du didacticiel terminées, reportez-vous aux rubriques suivantes pour plus d'informations sur l'Assistant Paramétrage du moteur de base de données :  
  
-   [Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/database-engine-tuning-advisor.md) : cette rubrique propose des descriptions de la façon d’effectuer des tâches avec cet outil.  
  
-   [Utilitaire dta](../../tools/dta/dta-utility.md) : cette rubrique propose des documents de référence sur l’utilitaire en ligne de commande et le fichier XML facultatif que vous pouvez utiliser pour contrôler le fonctionnement de l’utilitaire.  
  
## <a name="next-lesson"></a>Leçon suivante  
[Leçon 1 : Navigation de base dans l’Assistant Paramétrage du moteur de base de données](../../tools/dta/lesson-1-basic-navigation-in-database-engine-tuning-advisor.md)  
  
  
  
