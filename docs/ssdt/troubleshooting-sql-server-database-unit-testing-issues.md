---
title: Dépannage des problèmes liés aux tests unitaires de base de données SQL Server
description: Affichez des conseils de résolution des problèmes que vous pouvez rencontrer avec les tests unitaires SQL Server, tels que les échecs de délai d’attente et le déploiement de bases de données sur des cibles inattendues.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: cf4c9cd1-7e73-4c3b-922a-68b9247e7b33
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 0bd87bb75255ca50ceaed8e46c356555cdedf799
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066864"
---
# <a name="troubleshooting-sql-server-database-unit-testing-issues"></a>Dépannage des problèmes liés aux tests unitaires de base de données SQL Server

Vous pouvez rencontrer les problèmes présentés dans cette rubrique lorsque vous exécutez des tests unitaires SQL Server sur une base de données :  
  
-   [Modifications des tests unitaires et du fichier App.Config ignorées lors de l'exécution de tests unitaires](#UnitTestingAndAppConfigChanges)  
  
-   [Déploiement de base de données vers une cible inattendue lors de l'exécution de tests unitaires](#DatabaseDeploymentInUnitTests)  
  
-   [Délais d'expiration lors de l'exécution de tests unitaires de base de données](#TimeoutsDuringUnitTests)  
  
## <a name="unit-testing-and-appconfig-changes-ignored-when-you-run-unit-tests"></a><a name="UnitTestingAndAppConfigChanges"></a>Modifications des tests unitaires et du fichier App.Config ignorées lors de l'exécution de tests unitaires  
Si vous modifiez le fichier App.Config dans le projet de test, vous devez recréer le projet de test avant que les modifications prennent effet. Ces modifications incluent celles apportées au fichier App.Config en utilisant la boîte de dialogue **Configuration de test de SQL Server**. Si vous ne recréez pas le projet de test, les modifications ne sont pas appliquées lorsque vous exécutez les tests unitaires.  
  
## <a name="database-deployment-to-unexpected-target-when-you-run-unit-tests"></a><a name="DatabaseDeploymentInUnitTests"></a>Déploiement de base de données vers une cible inattendue lors de l'exécution de tests unitaires  
Si vous déployez une base de données à partir d'un projet de base de données lorsque vous exécutez des tests unitaires, la base de données est déployée en utilisant les informations de la chaîne de connexion spécifiée dans la configuration des tests unitaires. Les informations de connexion spécifiées dans les propriétés de débogage du projet de base de données ne sont pas utilisées pour cette tâche, ce qui vous permet d'exécuter des tests unitaires SQL Server par rapport à différentes instances de la même base de données.  
  
## <a name="timeouts-when-you-run-database-unit-tests"></a><a name="TimeoutsDuringUnitTests"></a>Délais d'expiration lors de l'exécution de tests unitaires  
Si les tests unitaire de base de données échouent en raison d'un délai d'attente, vous pouvez augmenter le délai d'attente en mettant à jour le fichier app.config dans le projet de test. Le délai d'attente de connexion, défini sur la chaîne de connexion, indique le temps d'attente lorsque le test unitaire se connecte au serveur. Le délai d'expiration de la commande, qui doit être défini directement dans le fichier app.config indique le temps d'attente lorsque le test unitaire exécute le script Transact\-SQL. Si vous rencontrez des problèmes liés aux tests unitaires de longue durée, essayez d'augmenter la valeur de délai d'attente de la commande dans l'élément de contexte approprié. Par exemple, pour indiquer un délai d'expiration de la commande de 120 secondes pour l'élément **PrivilegedContext**, mettez à jour le fichier app.config comme suit :  
  
```  
<SqlUnitTesting_VS2010>  
    <DatabaseDeployment DatabaseProjectFileName="..\..\..\..\..\..\Visual Studio 2010\Projects\Database10\Database10\AdventureWorks.sqlproj"  
        Configuration="Debug" />  
    <DataGeneration ClearDatabase="true" />  
    <ExecutionContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(LocalDB)\Projects;Initial Catalog=AdventureWorks_Test;Integrated Security=True;Pooling=False"  
        CommandTimeout="30" />  
    <PrivilegedContext Provider="System.Data.SqlClient" ConnectionString="Data Source=(LocalDB)\Projects;Initial Catalog=AdventureWorks_Test;Integrated Security=True;Pooling=False"  
        CommandTimeout="120" />  
</SqlUnitTesting_VS2010>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Procédure : créer des tests unitaires SQL Server pour des fonctions, des déclencheurs ou des procédures stockées](../ssdt/how-to-create-unit-tests-for-functions-triggers-stored-procedures.md)  
[Procédure : configurer l’exécution de test unitaire SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md)  
  
