---
title: Erreurs courantes de script R
description: Cet article documente plusieurs erreurs de script courantes susceptibles de se produire lors de l’exécution de code R dans SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 05/31/2018
ms.topic: troubleshooting
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 0f9f086ec09830e65f85ca2df5232e8b401cb9eb
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186412"
---
# <a name="common-r-scripting-errors-in-sql-server"></a>Erreurs courantes de script R dans SQL Server
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Cet article documente plusieurs erreurs de script courantes susceptibles de se produire lors de l’exécution de code R dans SQL Server. Cette liste n’est pas exhaustive. Il existe en effet de nombreux packages, et les erreurs peuvent varier entre les versions d’un même package.

## <a name="valid-script-fails-in-t-sql-or-in-stored-procedures"></a>Échec de script valide dans T-SQL ou dans les procédures stockées

Avant de wrapper votre code R dans une procédure stockée, nous vous recommandons de l’exécuter dans un environnement de développement intégré (IDE) externe ou dans l’un des outils R tels que RTerm ou RGui. Ces méthodes vous permettent de tester et de déboguer le code en utilisant les messages d’erreur détaillés que retourne R.

Cependant, il arrive parfois que du code qui fonctionne parfaitement dans un utilitaire ou un IDE externe ne s’exécute pas dans une procédure stockée ou un contexte de calcul SQL Server. Dans ce cas, vous devez éliminer un certain nombre de problèmes avant d’en déduire que le package ne fonctionne pas dans SQL Server.

1. Vérifiez si Launchpad est en cours d’exécution.

2. Examinez les messages pour voir si les données d’entrée ou de sortie contiennent des colonnes avec des types de données incompatibles ou non pris en charge. Par exemple, les requêtes sur une base de données SQL retournent souvent des GUID ou des RowGUID qui ne sont pas pris en charge. Pour plus d’informations, consultez [Types de données et bibliothèques R](../r/r-libraries-and-data-types.md).

3. Passez en revue les pages d’aide des fonctions R individuelles pour déterminer si tous les paramètres sont pris en charge pour le contexte de calcul SQL Server. Pour obtenir de l’aide sur ScaleR, utilisez les commandes d’aide R inline ou consultez les [informations de référence sur le package](/r-server/r-reference/revoscaler/revoscaler).

Si le runtime R fonctionne mais que votre script retourne des erreurs, nous vous recommandons d’essayer de déboguer le script dans un environnement de développement R dédié comme Outils R pour Visual Studio.

Nous vous recommandons également de passer en revue le script et de le réécrire un peu afin de corriger tout problème lié aux types de données susceptible de se produire quand vous déplacez des données entre R et le moteur de base de données. Pour plus d’informations, consultez [Types de données et bibliothèques R](../r/r-libraries-and-data-types.md).

Vous pouvez également utiliser le package sqlrutils pour placer votre script R dans une procédure stockée, ce format pouvant être plus facilement consommé. Pour plus d'informations, consultez les pages suivantes :
* [Package sqlrutils](../r/ref-r-sqlrutils.md)
* [Créer une procédure stockée à l’aide de sqlrutils](/machine-learning-server/r-reference/sqlrutils/how-to-create-a-stored-procedure-from-r)

## <a name="script-returns-inconsistent-results"></a>Le script retourne des résultats incohérents

Les scripts R peuvent retourner des valeurs différentes dans un contexte SQL Server pour plusieurs raisons :

- La conversion de type implicite est effectuée automatiquement sur certains types de données, quand les données sont passées entre SQL Server et R. Pour plus d’informations, consultez [Types de données et bibliothèques R](../r/r-libraries-and-data-types.md).

- Déterminez si le nombre de bits est un facteur. Par exemple, les résultats d’opérations mathématiques pour les bibliothèques à virgule flottante 32 bits et 64 bits sont souvent différents.

- Déterminez si des valeurs NaN ont été produites dans une opération. Cela peut invalider les résultats.

- Les petites différences peuvent être amplifiées quand vous prenez une réciproque d’un nombre proche de zéro.

- Le cumul des erreurs d’arrondi peut entraîner par exemple des valeurs inférieures à zéro et non égales à zéro.

## <a name="implied-authentication-for-remote-execution-via-odbc"></a>Authentification implicite pour l’exécution à distance avec ODBC

Si vous vous connectez à l’ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour exécuter des commandes R à l’aide des fonctions **RevoScaleR**, vous pouvez obtenir une erreur lors de l’utilisation d’appels ODBC qui écrivent des données sur le serveur. Cette erreur se produit uniquement quand vous utilisez l’authentification Windows.

Elle est due au fait que les comptes de travail créés pour R Services ne sont pas autorisés à se connecter au serveur. Les appels ODBC ne peuvent donc pas être exécutés en votre nom. Le problème ne se produit pas avec les connexions SQL dans la mesure où les informations d’identification sont passées explicitement du client R à l’instance SQL Server, puis à ODBC. Toutefois, l’utilisation de connexions SQL est moins sécurisée que l’authentification Windows.

Pour passer vos informations d’identification Windows de manière sécurisée à partir d’un script lancé à distance, SQL Server doit émuler vos informations d’identification. Le terme « _authentification implicite_ » désigne ce processus. Pour que cela fonctionne, les comptes de travail qui exécutent des scripts R ou Python sur l’ordinateur SQL Server doivent disposer des autorisations appropriées.

1. Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] en tant qu’administrateur sur l’instance où vous souhaitez exécuter le code R.

2. Exécutez le script suivant. Veillez à modifier le nom du groupe d’utilisateurs, si vous avez modifié la valeur par défaut, ainsi que le nom de l’ordinateur et celui de l’instance.

    ```sql
    USE [master]
    GO
    
    CREATE LOGIN [computername\\SQLRUserGroup] FROM WINDOWS WITH
    DEFAULT_DATABASE=[master], DEFAULT_LANGUAGE=[language]
    GO
    ```

## <a name="avoid-clearing-the-workspace-while-youre-running-r-in-a-sql-compute-context"></a>Évitez d’effacer l’espace de travail quand vous exécutez R dans un contexte de calcul SQL

Bien que l’effacement de l’espace de travail soit courant dans la console R, cette opération peut avoir des conséquences imprévues dans un contexte de calcul SQL.

`revoScriptConnection` est un objet dans l’espace de travail R qui contient des informations sur une session R appelée à partir de SQL Server. Toutefois, si votre code R inclut une commande pour effacer l’espace de travail (comme `rm(list=ls())`), toutes les informations sur la session et les autres objets dans l’espace de travail R sont également effacées.

Pour contourner ce problème, évitez l’effacement global de variables et d’autres objets pendant que vous exécutez du code R dans SQL Server. Vous pouvez supprimer des variables spécifiques à l’aide de la fonction **remove** :

```R
remove('name1', 'name2', ...)
```

Si plusieurs variables doivent être supprimées,nous vous suggérons d’enregistrer les noms des variables temporaires dans une liste puis d’effectuer périodiquement des garbage collections sur la liste.



## <a name="next-steps"></a>Étapes suivantes

[Résolution des problèmes connus dans Machine Learning Services](machine-learning-troubleshooting-overview.md)

[Collecte de données pour la résolution des problèmes de machine learning](data-collection-ml-troubleshooting-process.md)

[Installer SQL Server Machine Learning Services](../install/sql-machine-learning-services-windows-install.md)

[Résoudre les problèmes de connexion au moteur de base de données](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md)