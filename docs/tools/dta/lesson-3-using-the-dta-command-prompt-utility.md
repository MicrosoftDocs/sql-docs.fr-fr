---
title: Utilisation de l’utilitaire de ligne de commande dta
description: En savoir plus sur la fonctionnalité offerte par l’utilitaire d’invite de commande des données en plus de celles fournies par l’Assistant Paramétrage du moteur de base de données SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], tutorials
ms.assetid: 30f27f4d-8852-4b12-ba62-57f63e496f1d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 064d7fc2d47cbd4f688d113f60be6080ca90fee1
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642589"
---
# <a name="lesson-3-using-the-dta-command-prompt-utility"></a>Leçon 3 : Utilisation de l’utilitaire de ligne de commande dta
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
L’utilitaire en ligne de commande **dta** offre une fonctionnalité supplémentaire par rapport à celles fournies par l’Assistant Paramétrage du moteur de base de données.  
  
Vous pouvez utiliser vos outils XML favoris pour créer des fichiers d'entrée pour l'utilitaire en utilisant le schéma XML de l'Assistant Paramétrage du moteur de base de données. Ce schéma est installé lorsque vous installez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et il se trouve dans le dossier : C:\Program Files (x86)\Microsoft SQL Server\110\Tools\Binn\schemas\sqlserver\2004\07\dta\dtaschema.xsd.  
  
Le schéma XML de l'Assistant Paramétrage du moteur de base de données est également disponible en ligne sur ce [site Web de Microsoft](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409).  
  
Ce schéma offre plus de souplesse pour définir les options de paramétrage. Par exemple, il permet d'effectuer des analyses de scénarios. Pour réaliser des analyses de scénarios, vous devez spécifier un ensemble de structures de création physiques existantes ou hypothétiques pour la base de données que vous souhaitez paramétrer, puis l'analyser avec l'Assistant Paramétrage du moteur de base de données pour déterminer si cette structure de création physique hypothétique améliorera les performances de traitement des requêtes. Ce type d'analyse offre l'avantage de pouvoir évaluer la nouvelle configuration sans avoir à supporter la charge induite par une mise en œuvre effective. Si votre structure de création physique hypothétique ne permet pas d'obtenir les améliorations souhaitées en matière de performances, il est facile de la modifier et de l'analyser de nouveau jusqu'à ce que vous ayez atteint la configuration qui produit les résultats requis.  
  
De plus, en utilisant le schéma XML de l’Assistant Paramétrage du moteur de base de données et l’utilitaire en ligne de commande **dta** , vous pouvez intégrer la fonctionnalité de l’Assistant Paramétrage du moteur de base de données aux scripts et l’utiliser avec d’autres outils de création de base de données.  
  
Cette leçon ne couvre pas l'utilisation de la fonction d'entrée XML de l'Assistant Paramétrage du moteur de base de données.  
  
Au cours de cette tâche, vous allez démarrer l’utilitaire **dta** et afficher son aide, puis vous allez l’utiliser afin de paramétrer une charge de travail à partir de l’invite de commandes. La charge de travail utilisée est la charge, MyScript.sql, que vous avez créée dans l’exercice sur l’interface utilisateur graphique de l’Assistant Paramétrage du moteur de base de données : [Paramétrage d’une charge de travail](lesson-2-using-database-engine-tuning-advisor.md#tuning-a-workload)  
  
Ce tutoriel utilise l’exemple de base de données AdventureWorks2017. Pour des raisons de sécurité, les exemples de bases de données ne sont pas installés par défaut. Pour installer les exemples de bases de données, consultez [Installation des exemples SQL Server et des exemples de bases de données](../../samples/adventureworks-install-configure.md).  
  
Les tâches suivantes sont destinées à vous guider pour ouvrir une invite de commandes, démarrer l’utilitaire en ligne de commande **dta** , afficher son aide sur la syntaxe et paramétrer la charge de travail simple, MyScript.sql, que vous avez créée dans le cadre de l’exercice : [Paramétrage d’une charge de travail](./lesson-2-using-database-engine-tuning-advisor.md).  

## <a name="prerequisites"></a>Prérequis 

Pour suivre ce tutoriel, vous avez besoin de SQL Server Management Studio, de l’accès à un serveur qui exécute SQL Server et d’une base de données AdventureWorks.

- Installez [SQL Server 2017 Developer Edition.](https://www.microsoft.com/sql-server/sql-server-downloads)
- Téléchargez [l’exemple de base de données AdventureWorks2017.](../../samples/adventureworks-install-configure.md)


Les instructions de restauration de bases de données dans SSMS se trouvent ici : [Restaurer une base de données.](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)

  >[!NOTE]
  > Ce tutoriel est destiné aux utilisateurs familiarisés avec l’utilisation de SQL Server Management Studio et les tâches de base d’administration de base de données. 

## <a name="access-dta-command-prompt-utility-help-menu"></a>Accéder au menu d’aide de l’utilitaire de ligne de commande DTA
  
  
1.  Dans le menu **Démarrer** , pointez sur **Tous les programmes**, sur **Accessoires**, puis cliquez sur **Invite de commandes**.  
  
2.  À l’invite de commandes, tapez ce qui suit, puis appuyez sur Entrée :  
  
    ```  
    dta -? | more  
    ```  
  
    La section `| more` de cette commande est facultative. Sachez cependant que son utilisation permet de parcourir l'aide sur la syntaxe de l'utilitaire. Appuyez sur Entrée pour faire défiler le texte de l'aide ligne par ligne ou sur Espace pour le faire défiler page par page.  

  ![Utilisation de l’aide avec l’utilitaire DTA cmd](media/dta-tutorials/dta-cmd-help.png)

## <a name="tune-simple-workload-using-the-dta-command-prompt-utility"></a>Paramétrer une charge de travail simple à l'aide de l'utilitaire de ligne de commande DTA  


  
1.  À l'invite de commandes, indiquez le chemin du répertoire dans lequel vous avez stocké le fichier MyScript.sql.  
  
2.  À l'invite de commandes, tapez la commande suivante et appuyez sur Entrée pour l'exécuter et démarrer la session de paramétrage (notez que l'utilitaire respecte la distinction entre les majuscules et les minuscules lorsqu'il analyse les commandes) :  
  
    ```  
    dta -S YourServerName\YourSQLServerInstanceName -E -D AdventureWorks2012 -if MyScript.sql -s MySession2 -of MySession2OutputScript.sql -ox MySession2Output.xml -fa IDX_IV -fp NONE -fk NONE  
    ```  
  
    où `-S` spécifie le nom de votre serveur et l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] est installée. Le paramètre `-E` spécifie que vous souhaitez utiliser une connexion approuvée à l'instance, ce qui est approprié si vous utilisez un compte de domaine Windows pour vous connecter. Le paramètre `-D` spécifie la base de données à paramétrer, `-if` le fichier de la charge de travail, `-s` le nom de la session, `-of` le fichier dans lequel l'outil doit enregistrer le script [!INCLUDE[tsql](../../includes/tsql-md.md)] de recommandations et enfin, le paramètre `-ox` indique le fichier dans lequel l'outil doit enregistrer les recommandations au format XML. Les trois derniers commutateurs spécifient les options de paramétrage comme suit : `-fa IDX_IV` spécifie que l'Assistant Paramétrage du moteur de base de données doit considérer uniquement l'ajout des index (cluster et non cluster) et des vues indexées ; `-fp NONE` spécifie qu'aucune stratégie de partition ne doit être prise en compte au cours de l'analyse et `-fk NONE` indique qu'aucune structure de création physique existante dans la base de données ne doit être conservée lorsque l'Assistant Paramétrage du moteur de base de données effectue ses recommandations.  

  ![utilisation de CMD avec DTA](media/dta-tutorials/dta-cmd.png)
  
3.  Lorsque l'Assistant Paramétrage du moteur de base de données a terminé de paramétrer la charge de travail, il affiche un message signalant que votre session de paramétrage s'est terminée avec succès. Vous pouvez afficher les résultats du paramétrage. Pour cela, utilisez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour ouvrir les fichiers MySession2OutputScript.sql et RMySession2Output.xml. Une autre méthode consiste à ouvrir la session de paramétrage MySession2 dans l’interface de l’Assistant Paramétrage du moteur de base de données et à afficher les recommandations et les rapports en procédant comme aux exercices [Affichage des recommandations pour le paramétrage](./lesson-2-using-database-engine-tuning-advisor.md) et [Affichage des rapports de paramétrage](./lesson-2-using-database-engine-tuning-advisor.md).  
  
 
## <a name="after-you-finish-this-tutorial"></a>Fin du didacticiel  
Une fois les leçons du didacticiel terminées, reportez-vous aux rubriques suivantes pour plus d'informations sur l'Assistant Paramétrage du moteur de base de données :  
  
-   [Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/database-engine-tuning-advisor.md) : cette rubrique propose des descriptions de la façon d’effectuer des tâches avec cet outil. 
-   [Utilitaire dta](../../tools/dta/dta-utility.md) : cette rubrique propose des documents de référence sur l’utilitaire en ligne de commande et le fichier XML facultatif que vous pouvez utiliser pour contrôler le fonctionnement de l’utilitaire.  
  
Pour revenir au début du tutoriel, voir [Tutoriel : Assistant Paramétrage du moteur de base de données](../../tools/dta/tutorial-database-engine-tuning-advisor.md).  
  
## <a name="see-also"></a>Voir aussi  
[Didacticiels sur le moteur de base de données](../../relational-databases/database-engine-tutorials.md)  
