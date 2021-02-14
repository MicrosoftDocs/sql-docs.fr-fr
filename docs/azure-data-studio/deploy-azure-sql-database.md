---
title: Déployer Azure SQL Database par le biais d’un notebook
description: Ce tutoriel montre comment créer une base de données Azure SQL.
author: ninarn
ms.author: ninarn
ms.reviewer: alayu, markingmyname
ms.topic: tutorial
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.custom: ''
ms.date: 10/16/2020
ms.openlocfilehash: 8bac1e0d10208283e40843288ab7dbe7d042b8ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052210"
---
# <a name="create-an-azure-sql-database-using-azure-data-studio"></a>Créer une base de données Azure SQL à l’aide d’Azure Data Studio

Vous pouvez créer une base de données Azure SQL en utilisant Azure Data Studio par le biais de l’Assistant Déploiement et de notebooks.

## <a name="pre-requisites"></a>Conditions préalables

 - [Azure Data Studio](download-azure-data-studio.md) est installé
 - Un compte et un abonnement Azure actifs. Si vous n’en avez pas, [créez un compte gratuit](https://azure.microsoft.com/free/).

## <a name="use-the-deployment-wizard"></a>Utiliser l’Assistant Déploiement

Effectuez les étapes suivantes pour utiliser l’Assistant Déploiement, qui vous aidera à configurer les paramètres requis dans une interface utilisateur simple.

Tout d’abord, recherchez et sélectionnez Azure SQL Database dans l’Assistant Déploiement.

 1. Dans Azure Data Studio, sélectionnez la viewlet Connexions dans le volet de navigation gauche.
 2. Sélectionnez le bouton **...** en haut du panneau Connexions, puis choisissez **Nouveau déploiement**.
 3. Dans l’Assistant Déploiement, sélectionnez la vignette **Azure SQL Database** et cochez la case d’acceptation des conditions générales.
 4. Dans la liste déroulante Type de ressource, sélectionnez ce que vous souhaitez créer : une base de données, un serveur de base de données ou un pool élastique. Si vous ne disposez pas de bases de données SQL dans Azure, nous vous recommandons de commencer par créer une base de données.
 5. Sélectionnez **Créer sur le Portail Azure** si vous avez choisi de créer un serveur ou un pool, ou **Sélectionner** si vous avez choisi de créer une base de données.

Si vous avez choisi de créer une base de données, procédez comme suit.

 1. Connectez-vous à votre compte Azure si ce n’est déjà fait. Vous pouvez actualiser votre connexion si vous rencontrez des problèmes sur cette page de l’Assistant.
 2. Sélectionnez l’abonnement et le serveur de votre choix. Sélectionnez ensuite **Suivant**.
 3. Entrez un nom de base de données.
 4. Entrez un nom de règle de pare-feu et la plage d’adresses IP de votre ordinateur client local exécutant Azure Data Studio. Cela vous permet de vous connecter au serveur et à la base de données à partir d’ADS juste après sa création.
 5. Sélectionnez **Suivant**.
 6. Passez en revue les paramètres que vous avez entrés, puis sélectionnez **Exécuter un script sur un notebook**.

Une fois le notebook ouvert, vous pouvez examiner le contenu et le code, et apporter des modifications si vous le souhaitez. En particulier, vous pouvez modifier les paramètres de calcul et de stockage si vous avez des préférences spécifiques en matière de performances ou de coûts, autres que les paramètres par défaut. Toutefois, n’oubliez pas que les modifications que vous apportez au notebook peuvent entraîner des erreurs de validation.

La dernière étape consiste à sélectionner **Tout exécuter** pour exécuter toutes les cellules du notebook. Vous disposerez alors d’une base de données SQL Database complète et fonctionnelle à laquelle vous pourrez vous connecter pour effectuer des requêtes à partir d’ADS.

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez créé votre base de données, serveur ou pool, vous pouvez [vous connecter à votre base de données et l’interroger](quickstart-sql-database.md) dans Azure Data Studio.
