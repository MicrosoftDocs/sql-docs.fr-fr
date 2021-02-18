---
title: Utilisation de l’Assistant Paramétrage du moteur de base de données
description: En savoir plus sur la façon dont l’interface graphique utilisateur de l’Assistant Paramétrage du moteur de base de données SQL Server ajuste les bases de données, gère les sessions de réglage et affiche des suggestions de réglage.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], tutorials
ms.assetid: 3317d4f8-ed9e-4f2e-b5f1-a6bf3a9d6c8d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 10d32bb3267e79f4f97ce7a4832c1a999751904c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353512"
---
# <a name="lesson-2-using-database-engine-tuning-advisor"></a>Leçon 2 : Utilisation de l’Assistant Paramétrage du moteur de base de données

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

L'Assistant Paramétrage du moteur de base de données permet de paramétrer les bases de données, de gérer les sessions de paramétrage et d'afficher les recommandations de paramétrage. Les utilisateurs possédant des connaissances avancées sur les structures de création physiques peuvent utiliser cet outil pour réaliser des analyses exploratoires de paramétrage des bases de données. Les utilisateurs novices dans le paramétrage des bases de données peuvent également l'utiliser pour rechercher les configurations les plus adaptées aux structures de création physiques des charges de travail qu'ils doivent paramétrer. Cette leçon propose des exercices pratiques de base aux administrateurs de base de données qui sont novices dans l'utilisation de l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données et aux administrateurs système qui n'ont pas de connaissances étendues sur les structures de création physiques.  

## <a name="prerequisites"></a>Prérequis 

Pour suivre ce tutoriel, vous avez besoin de SQL Server Management Studio, de l’accès à un serveur qui exécute SQL Server et d’une base de données AdventureWorks.

- Installez [SQL Server Management Studio.](../../ssms/download-sql-server-management-studio-ssms.md)
- Installez [SQL Server 2017 Developer Edition.](https://www.microsoft.com/sql-server/sql-server-downloads)
- Téléchargez [l’exemple de base de données AdventureWorks2017.](../../samples/adventureworks-install-configure.md)


Les instructions de restauration de bases de données dans SSMS se trouvent ici : [Restaurer une base de données.](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)

  >[!NOTE]
  > Ce tutoriel est destiné aux utilisateurs familiarisés avec l’utilisation de SQL Server Management Studio et les tâches de base d’administration de base de données. 
  
## <a name="tuning-a-workload"></a>Paramétrage d’une charge de travail
L'Assistant Paramétrage du moteur de base de données peut servir à trouver la conception de base de données physique qui permet d'obtenir les meilleures performances des requêtes sur les bases de données et les tables que vous avez sélectionnées pour le paramétrage.  

1.  Copiez un exemple d’instruction [SELECT](../../t-sql/queries/select-examples-transact-sql.md) et collez l’instruction dans l’éditeur de requête de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Enregistrez le fichier sous le nom **MyScript.sql** dans un répertoire où il est facile de le retrouver. Vous trouverez ci-dessous un exemple qui fonctionne sur la base de données AdventureWorks2017.  

 ```sql
 Use [Adventureworks2017]; -- may need to modify database name to match database
 GO
 SELECT DISTINCT pp.LastName, pp.FirstName 
 FROM Person.Person pp JOIN HumanResources.Employee e
 ON e.BusinessEntityID = pp.BusinessEntityID WHERE pp.BusinessEntityID IN 
 (SELECT SalesPersonID 
 FROM Sales.SalesOrderHeader
 WHERE SalesOrderID IN 
 (SELECT SalesOrderID 
 FROM Sales.SalesOrderDetail
 WHERE ProductID IN 
 (SELECT ProductID 
 FROM Production.Product p 
 WHERE ProductNumber = 'BK-M68B-42')));
 GO
 ```

  ![Enregistrer une requête SQL](media/dta-tutorials/dta-save-query.png)
  
2.  Démarrez l'Assistant Paramétrage du moteur de base de données. Sélectionnez l'**Assistant Paramétrage de base de données** dans le menu **Outils** de SQL Server Management Studio (SSMS).  Pour plus d'informations, consultez [Lancement de l’Assistant Paramétrage du moteur de base de données](lesson-1-basic-navigation-in-database-engine-tuning-advisor.md#launch-database-tuning-advisor). Connectez-vous à votre serveur SQL dans la boîte de dialogue **Se connecter au serveur** .  
  
3.  Dans l’onglet **Général** du volet droit de l’interface utilisateur graphique de l’Assistant Paramétrage du moteur de base de données, tapez **MySession** dans la zone **Nom de session**. 
  
4.  Sélectionnez **Fichier** pour votre **Charge de travail**, puis sélectionnez l’icône de jumelles pour **Rechercher un fichier de charge de travail**. Localisez le fichier **MyScript.sql** enregistré à l’étape 1.  

   ![Recherchez le script qui a été enregistré précédemment](media/dta-tutorials/dta-script.png)
  
5.  Sélectionnez AdventureWorks2017 dans la liste **Base de données pour l'analyse de la charge de travail**, sélectionnez AdventureWorks2017 dans la grille **Sélectionnez les bases de données et tables à analyser** et sélectionnez **Enregistrer le journal de paramétrage**. **Base de données pour l’analyse de la charge de travail** spécifie la première base de données à laquelle l’Assistant Paramétrage du moteur de base de données se connecte lors du paramétrage d’une charge de travail. Dès que le paramétrage commence, l'Assistant Paramétrage du moteur de base de données se connecte aux bases de données spécifiées par les instructions `USE DATABASE` contenues dans la charge de travail.  

  ![Options DTA pour base de données](media/dta-tutorials/dta-select-db.png)
  
6.  Cliquez sur l’onglet **Options de paramétrage** . Dans le cadre de cet exercice, il ne vous est pas demandé de définir des options de paramétrage, mais prenez un moment pour passer en revue les options de paramétrage par défaut. Appuyez sur F1 pour afficher l'aide de cette page à onglets. Cliquez sur **Options avancées** pour afficher des options de paramétrage supplémentaires. Cliquez sur **Aide** dans la boîte de dialogue **Options de paramétrage avancées** pour obtenir des informations sur les options de paramétrage affichées ici. Cliquez sur **Annuler** pour fermer la boîte de dialogue **Options de paramétrage avancées** sans désactiver les options sélectionnées par défaut.  

  ![Options de paramétrage DTA](media/dta-tutorials/dta-tuning-options.png)
  
7.  Dans la barre d'outils, cliquez sur le bouton **Démarrer l'analyse** . Pendant que l’Assistant Paramétrage du moteur de base de données analyse la charge de travail, vous pouvez contrôler l’état de l’analyse sous l’onglet **Progression** . Quand le paramétrage est terminé, l’onglet **Recommandations** s’affiche.  
  
    Si vous recevez une erreur relative à l’heure et à la date d’arrêt du paramétrage, vérifiez l’heure précisée pour l’option **Arrêter à** sous l’onglet **Options de paramétrage** principal. Vérifiez que l’heure et la date spécifiées dans **Arrêter à** sont ultérieures à la date et à l’heure actuelles et, si besoin est, modifiez-les.  

  ![Démarrez l’analyse DTA](media/dta-tutorials/dta-start-analysis.png)

  
8.  Une fois l’analyse terminée, enregistrez votre recommandation sous la forme d’un script [!INCLUDE[tsql](../../includes/tsql-md.md)] en cliquant sur **Enregistrer les recommandations** dans le menu **Actions** . Dans la boîte de dialogue **Enregistrer sous** , accédez au répertoire dans lequel vous souhaitez enregistrer le script de recommandations et tapez le nom de fichier **MyRecommendations**.  

  ![Enregistrer les recommandations DTA](media/dta-tutorials/dta-save-recommendations.png)

## <a name="view-tuning-recommendations"></a>Afficher les recommandations de paramétrage
  
1.  Sous l’onglet **Recommandations** , utilisez la barre de défilement en bas de la page à onglet pour afficher toutes les colonnes **Recommandations d’index** . Chaque ligne représente un objet de base de données (index ou vues indexées) que l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] recommande de supprimer ou de créer. Faites défiler l’écran vers la colonne la plus à droite et sélectionnez une **Définition**. [!INCLUDE[ssDE](../../includes/ssde-md.md)] L’Assistant Paramétrage du moteur de base de données affiche la fenêtre **Aperçu de script SQL** , dans laquelle vous pouvez voir le script [!INCLUDE[tsql](../../includes/tsql-md.md)] qui permet de créer ou de supprimer l’objet de base de données sur cette ligne. Cliquez sur **Terminer** pour fermer la fenêtre de prévisualisation.  
  
    Si vous avez du mal à trouver une **Définition** contenant un lien, décochez la case **Afficher les objets existants** en bas de la page à onglet pour diminuer le nombre de lignes affichées. Quand vous décochez cette case, l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] affiche uniquement les objets pour lesquels il a émis une recommandation. Cochez la case **Afficher les objets existants** pour visualiser tous les objets de base de données existant dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Utilisez la barre de défilement à droite de la page à onglet pour visualiser tous les objets.

  ![Recommandation d’index DTA](media/dta-tutorials/dta-recommendation.png)  
  
2.  Cliquez avec le bouton droit dans la grille du volet **Recommandations d’index** . Le menu contextuel qui s'affiche permet de sélectionner et de désélectionner des recommandations. Il permet également de modifier la police du texte de la grille.  
 
   ![Menu de sélection pour la recommandation d’index](media/dta-tutorials/dta-index-recommendation-options.png)
  
3.  Dans le menu **Actions** , cliquez sur **Enregistrer les recommandations** pour enregistrer toutes les recommandations dans un seul script [!INCLUDE[tsql](../../includes/tsql-md.md)] . Attribuez à ce script le nom **MySessionRecommendations.sql**.  
  
    Ouvrez le script MySessionRecommendations.sql dans l'Éditeur de requête de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour en afficher le contenu. Vous pouvez appliquer ces recommandations à la base de données exemple [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] en exécutant le script dans l'Éditeur de requête, mais ne le faites pas. Fermez le script dans l'Éditeur de requête sans l'exécuter.  
  
    L’autre méthode pour appliquer les recommandations consiste à cliquer sur **Appliquer les recommandations** dans le menu **Actions** de l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] , mais n’appliquez pas ces recommandations maintenant.  
  
4.  Si plusieurs recommandations sont fournies sous l’onglet **Recommandations** , supprimez certaines lignes contenant des objets de base de données dans la grille **Recommandations d’index** .  
  
5.  Dans le menu **Actions** , cliquez sur **Évaluer les recommandations**. [!INCLUDE[ssDE](../../includes/ssde-md.md)] L'Assistant Paramétrage du moteur de base de données crée une nouvelle session de paramétrage qui permet d'évaluer un sous-ensemble des recommandations d'origine de la session MySession.  
  
6.  Tapez **EvaluateMySession (ÉvaluerMaSession)** pour la session dont le nom figure dans **Nom de session**, puis cliquez sur le bouton **Démarrer l’analyse** de la barre d’outils. Vous pouvez répéter les étapes 2 et 3 pour cette nouvelle session de paramétrage afin d'en visualiser les recommandations.  
  
### <a name="summary"></a>Résumé  
L'évaluation d'un sous-ensemble de recommandations de paramétrage peut s'avérer nécessaire si vous pensez devoir modifier des options de paramétrage après l'exécution d'une session. Cela peut être le cas, par exemple, si vous configurez l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour qu’il tienne compte des vues indexées quand vous spécifiez des options de paramétrage pour une session, mais décidez après la génération des recommandations de réutiliser les vues indexées. Vous pouvez utiliser ensuite l’option **Évaluer les recommandations** du menu **Actions** pour que l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] réévalue la session sans tenir compte des vues indexées. Quand vous utilisez l’option **Évaluer les recommandations** , les recommandations générées sont appliquées hypothétiquement à la structure de création physique courante pour parvenir à la structure de création physique de la deuxième session de paramétrage.  
  
Il est possible de visualiser davantage d’informations sur les résultats du paramétrage sous l’onglet **Rapports** , décrit dans la tâche suivante de la présente leçon.  

## <a name="view-tuning-reports"></a>Affichage des rapports de paramétrage
Bien qu'il soit utile d'afficher les scripts utilisables pour implémenter les résultats du paramétrage, l'Assistant Paramétrage du moteur de base de données fournit également de nombreux rapports utiles que vous pouvez afficher. Ces rapports fournissent des informations sur les structures de création physiques existantes dans la base de données que vous paramétrez et sur les structures recommandées. Pour afficher les rapports de paramétrage, cliquez sur l'onglet **Rapports** comme décrit dans l'exercice qui suit.


1. Sélectionnez l’onglet **Rapports** dans l’Assistant Paramétrage de base de données. 
2. Dans le volet **Résumé de paramétrage** , vous pouvez visualiser les informations relatives à cette session de paramétrage. Utilisez la barre de défilement pour visualiser tout le contenu du volet. Notez les informations des zones **Pourcentage d'amélioration attendu** et **Espace occupé par la recommandation**. Il est possible de limiter l'espace utilisé par la recommandation lorsque vous définissez les options de paramétrage. Sous l'onglet **Options de paramétrage** , sélectionnez **Options avancées**. Activez **Définir une quantité d'espace max. pour les recommandations** et spécifiez, en mégaoctets, l'espace maximal qu'une configuration recommandée peut utiliser. Utilisez le bouton **Précédent** dans l'aide de votre navigateur pour revenir au didacticiel. 

    ![Résumé de paramétrage DTA](media/dta-tutorials/dta-tuning-summary.png)
  
3.  Dans le volet **Rapports de paramétrage** , sélectionnez **Rapport de coût d'instruction** dans la liste **Sélectionnez un rapport** . Si vous souhaitez disposer de davantage d'espace pour afficher le rapport, faites glisser le bord du volet **Moniteur de session** vers la gauche. Chaque instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qui s'exécute sur une table de votre base de données est associée à un coût de performance. Ce coût de performance peut être réduit par la création d'index efficaces sur les colonnes qui font souvent l'objet d'accès dans une table. Ce rapport montre le pourcentage d'amélioration estimé entre le coût de départ pour l'exécution d'une instruction dans la charge de travail et le coût si la recommandation de paramétrage est appliquée. Notez que la quantité d'informations contenues dans le rapport est fonction de la longueur et de la complexité de la charge de travail.  

    ![Rapport DTA : coût d'instruction](media/dta-tutorials/dta-statement-cost.png)
  
4.  Cliquez avec le bouton droit sur le volet **Rapport de coût d’instruction** dans la grille et cliquez sur **Exporter vers le fichier**. Enregistrez le rapport sous **MyReport**. Une extension .xml est ajoutée automatiquement au nom du fichier. Vous pouvez ouvrir le fichier MyReport.xml dans votre éditeur XML favoris ou dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour afficher le contenu du rapport.  
  
5.  Revenez à l’onglet **Rapports** de l’Assistant Paramétrage du moteur de base de données et recliquez avec le bouton droit sur **Rapport de coût d’instruction** . Passez en revue les autres options disponibles. Notez que vous pouvez modifier la police du rapport affiché. Si vous modifiez la police ici, la police est également modifiée dans les autres pages à onglet.  
  
6.  Sélectionnez d'autres rapports dans la liste **Sélectionnez un rapport** pour vous familiariser avec eux.  
  
### <a name="summary"></a>Résumé  
Vous avez parcouru l'onglet **Rapports** de l'interface utilisateur graphique de l'Assistant Paramétrage du moteur de base de données pour la session de paramétrage MySession. Vous pouvez suivre ces mêmes étapes pour parcourir les rapports générés pour la session de paramétrage EvaluateMySession (ÉvaluerMaSession). Double-cliquez sur **EvaluateMySession** dans le volet **Moniteur de session** pour commencer.  


 ## <a name="next-lesson"></a>Leçon suivante  
[Leçon 3 : Utilisation de l’utilitaire de ligne de commande DTA](../../tools/dta/lesson-3-using-the-dta-command-prompt-utility.md)  
   
  
