---
title: Analyser les performances de script
description: Découvrez comment les statistiques client, les plans d’exécution et Code Analysis dans SQL Server Data Tools peuvent aider à améliorer les performances des requêtes, des procédures stockées et des scripts.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: f4bbdd31-12a5-4c57-b0fe-1c6683820f11
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 5f8d17fa156cd144a87b2dc387564cd93556d8da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077190"
---
# <a name="analyze-script-performance"></a>Analyser les performances de script

Utilisez les outils fournis par SQL Server Data Tools pour déterminer si vous pouvez améliorer les performances de votre requête, de vos procédures stockées ou de vos scripts. Par exemple, en surveillant les statistiques du client, telles que les temps de réponse des requêtes les plus fréquentes, vous pouvez déterminer s'il faut modifier les requêtes ou les index des tables. Ces statistiques peuvent inclure le temps d'exécution du client, le profil de requête et les paquets/octets envoyés et reçus.  
  
En outre, certains problèmes de performances sont mieux traités via l'analyse de l'application, des requêtes et des mises à jour que l'application envoie à la base de données, et de la façon dont ces requêtes et ces mises à jour interagissent avec le schéma de la base de données. Les plans d’exécution affichent graphiquement les méthodes d’extraction de données choisies par l’optimiseur de requête SQL Server, et montrent le coût d’exécution d’instructions et de requêtes en particulier. Ils peuvent ainsi aider à comprendre comment SQL Server traitera la requête SQL et à déterminer ce qui ralentit les performances.  
  
## <a name="using-client-statistics"></a>Utilisation des statistiques du client  
Lorsque vous exécutez un script ou une requête dans l’Éditeur Transact\-SQL, vous pouvez choisir de recueillir des statistiques client, notamment sur le profil d’application, le réseau et la durée de l’exécution. Ces mesures permettent d'évaluer l'efficacité de votre script, ou d'effectuer des test d'évaluation de différents scripts.  
  
Pour activer/désactiver la collecte de statistiques client, pointez sur **Éditeur Transact\-SQL** dans le menu **Données** lorsque l’Éditeur Transact\-SQL est ouvert et cliquez sur **Paramètres d’exécution**, puis sur **Inclure les statistiques client**. Vous pouvez également cliquer sur le bouton **Inclure les statistiques client** (le cinquième en partant de la droite) de la barre d’outils Éditeur Transact\-SQL, ou cliquer avec le bouton droit dans l’Éditeur Transact\-SQL et sélectionner **Paramètres d’exécution** et **Inclure les statistiques client**. Notez que pour collecter des statistiques pour une requête, vous devez activer cette fonctionnalité avant de l'exécuter.  
  
Si les statistiques client sont activées, l’onglet **Statistiques** s’affiche à côté de l’onglet **Message** lors de l’exécution de la requête. Si elles sont désactivées, l’onglet **Statistiques** n’apparaît pas. Les statistiques provenant d'exécutions de requêtes successives sont répertoriées avec les valeurs moyennes.  
  
Pour plus d’informations sur les statistiques collectées, consultez [Volet Interroger les statistiques de fenêtre](https://msdn.microsoft.com/library/aa216969(SQL.80).aspx) et la section [Onglet Statistiques client](/previous-versions/visualstudio/visual-studio-2010/aa833205(v=vs.100)) de cette rubrique.  
  
## <a name="using-execution-plans"></a>Utilisation des plans d'exécution  
Les plans d'exécution affichent la façon dont le moteur de base de données parcourt les tables et utilise les index pour accéder aux données d'une requête ou de toute autre instruction DML (par exemple, une mise à jour) et les traiter. Cette approche graphique s'avère très utile pour la compréhension des caractéristiques de performances d'une requête.  
  
Ouvrez un script Transact\-SQL contenant les requêtes à analyser dans l’éditeur Transact\-SQL. Vous pourrez alors mettre en surbrillance le code que vous souhaitez vérifier et choisir d’afficher un plan d’exécution estimé en cliquant sur le bouton **Afficher le plan d’exécution estimé** dans la barre d’outils de l’éditeur. Cliquer sur **Afficher le plan d’exécution estimé** n’entraîne pas l’exécution des lots ou des requêtes Transact\-SQL. En revanche, le script est analysé et le plan d'exécution de requête que le moteur de base de données utiliserait probablement si les requêtes étaient réellement exécutées s'affiche.  
  
Une fois le script analysé ou exécuté, cliquez sur l’onglet **Plan d’exécution** pour voir une représentation graphique du résultat du plan d’exécution.  
  
Le résultat du plan d'exécution graphique se lit de droite à gauche et de haut en bas. Chaque requête du traitement analysé est affichée, de même que le coût de chaque requête sous la forme d'un pourcentage du coût total du traitement. Pour afficher des informations supplémentaires, comme le coût et le fonctionnement de chaque étape, placez le curseur sur les [icônes des opérateurs logiques et physiques](/previous-versions/sql/sql-server-2008-r2/ms175913(v=sql.105)) sur le plan graphique.  
  
Pour modifier l’affichage du plan d’exécution, cliquez avec le bouton droit sur le **Plan d’exécution**, puis sélectionnez **Zoom avant**, **Zoom arrière**, **Zoom personnalisé** ou **Zoom pour ajuster**. **Zoom avant** et **Zoom arrière** permettent respectivement d’agrandir et de réduire l’affichage du plan d’exécution suivant des pourcentages fixes. **Zoom personnalisé** vous permet de définir votre propre facteur de zoom, par exemple 80 %.  **Zoom pour ajuster** ajuste le plan d’exécution au volet de résultats.  
  
Les plans d'exécution peuvent être enregistrés et rouverts ultérieurement à titre d'examen. Pour cela, cliquez avec le bouton droit sur le **Plan d’exécution** et sélectionnez **Enregistrer le plan d’exécution en tant que**. Après cela, vous pouvez ouvrir le plan dans Visual Studio, comme n'importe quel autre fichier.  
  
## <a name="using-code-analysis"></a>Utilisation de l'analyse du code  
Vous pouvez utiliser l'analyse du code pour découvrir d'éventuels problèmes dans vos scripts, tels que les problèmes de conception, d'attribution de nom et de performances.  Les règles pour les projets de base de données sont organisées en ensembles de règles prédéfinis qui ciblent des zones spécifiques, et vous pouvez activer ou désactiver une règle dans l'onglet **Analyse du code** de la page de propriétés **Propriétés du projet** . Dans le même onglet, vous pouvez spécifier que l'analyse du code soit exécutée automatiquement chaque fois qu'un projet est généré, ou si les avertissements doivent être traités comme des erreurs.  
  
Pour utiliser l’analyse du code manuellement, cliquez avec le bouton droit sur votre projet dans **l’Explorateur de solutions** et sélectionnez **Exécuter l’analyse du code**. Les avertissements d'analyse du code s'affichent dans la fenêtre **Liste d'erreurs** . Vous pouvez double-cliquer sur un avertissement pour accéder au code source comportant le problème. Pour afficher des informations supplémentaires et les corrections possibles d’un avertissement, utilisez le menu contextuel **Afficher de l’aide sur l’erreur**.  
  
Pour plus d’informations sur l’analyse du code, consultez [Analyse du code de la base de données pour améliorer la qualité du code](/previous-versions/visualstudio/visual-studio-2010/dd172133(v=vs.100)).  
