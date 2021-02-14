---
title: Chaînes de connexion et autorisations
description: En savoir plus sur les chaînes de connexion, les comptes et les autorisations dont vous avez besoin pour exécuter des tests unitaires SQL Server. Consultez comment configurer les chaînes de connexion.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: ceff114e-a738-46ad-9785-b6647a2247f9
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 70387a9e18888a47522613b7a76a2483aec0d287
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080810"
---
# <a name="overview-of-connection-strings-and-permissions"></a>Vue d'ensemble des chaînes de connexion et des autorisations

Pour exécuter des tests unitaires SQL Server, vous devez vous connecter à un serveur de base de données en utilisant une ou deux chaînes de connexion spécifiques. Chaque chaîne de connexion représente un compte disposant des autorisations spécifiques nécessaires pour effectuer une tâche ou un ensemble de tâches dans un script particulier dans le cadre du test. Vous pouvez spécifier ces chaînes dans la boîte de dialogue **Configuration de test SQL Server** ou en modifiant manuellement le fichier app.config de votre projet de test.  
  
## <a name="connection-strings"></a>Chaînes de connexion  
Dans la boîte de dialogue **Configuration de test SQL Server**, vous pouvez spécifier des chaînes de connexion pour chacun des comptes suivants.  
  
> [!NOTE]  
> Le contexte d’exécution et le contexte privilégié ne diffèrent que si l’authentification SQL Server est utilisée. Si vous utilisez l'authentification Windows, les mêmes informations d'identification sont utilisées pour les deux chaînes de connexion.  
  
-   Contexte d'exécution (obligatoire) : compte d'utilisateur utilisé pour exécuter le script de test. Cette chaîne de connexion doit avoir les mêmes informations d'identification que celles que doivent avoir les utilisateurs. Ceci est important, car cela garantit que les autorisations appropriées ont été appliquées à la base de données. Pour plus d’informations, consultez [Procédure : configurer l’exécution de test unitaire SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md).  
  
    Dans le fichier app.config de votre projet de test, il s'agit de l'élément `ExecutionContext`.  
  
-   Contexte de privilèges (facultatif) : compte dans la même base de données qui dispose d'autorisations plus élevées pour exécuter l'action d'avant test, l'action d'après test et les scripts TestInitialize et TestCleanup. Ces scripts définissent l'état de la base de données et pour l'action d'après test, peuvent être utilisés pour valider les objets de la base de données. Cette chaîne de connexion est également utilisée pour déployer les modifications de base de données et générer des données.  
  
    Dans le fichier app.config de votre projet de test, il s'agit de l'élément `PrivilegedContext`. Si vos tests unitaires SQL Server exécutent seulement le script de test, vous n’avez pas à spécifier de contexte privilégié.  
  
Les chaînes spécifiées dans la boîte de dialogue de configuration du projet sont enregistrées dans le fichier app.config de votre projet de test. Vous pouvez également modifier ce fichier directement et régénérer le projet, après quoi les nouvelles valeurs apparaissent dans la boîte de dialogue.  
  
## <a name="windows-authentication-versus-sql-server-authentication"></a>Comparaison entre l’authentification Windows et l’authentification SQL Server  
Lorsque vous spécifiez des chaînes de connexion, vous devez choisir entre l'utilisation de l'authentification Windows et l'authentification SQL. Une des raisons qui amènent à choisir l’authentification Windows est qu’elle prend mieux en charge l’utilisation de tests par une équipe que l’authentification SQL Server. Si vous choisissez l’authentification SQL Server, les chaînes de connexion sont chiffrées à l’aide de l’API de protection des données (DPAPI), selon les informations d’identification de l’utilisateur. Cela signifie que les tests de ce projet de test seront exécutés uniquement pour vous, et non pour les membres de l'équipe qui obtiennent les tests via le système de contrôle de code source après que vous les avez archivés. Pour exécuter des tests dans ce projet de test, les autres membres de votre équipe devront reconfigurer le projet de test à l'aide de leurs propres informations d'identification. Pour cela, ils doivent modifier la copie du fichier app.config ou utiliser la boîte de dialogue de configuration du projet.  
  
## <a name="permissions"></a>Autorisations  
Le script de test s'exécute au niveau d'autorisation du contexte d'exécution, qui est identique à celui appliqué pour les commandes de l'utilisateur qui sont exécutées sur la base de données lors d'une utilisation standard. L'action d'avant test, d'après test et les scripts TestInitialize et TestCleanup s'exécutent au niveau d'autorisation du contexte de privilèges.  
  
En raison de la connexion à niveau d'autorisation plus élevé utilisée pour le script d'action d'avant test, vous pouvez effectuer la validation dans celui-ci. Dans ce script, vous pouvez également exécuter des commandes de script qui testent les autorisations. Pour plus d’informations sur les autorisations, consultez la section des tests unitaires SQL Server de [Autorisations requises pour SQL Server Data Tools](../ssdt/required-permissions-for-sql-server-data-tools.md).  
  
## <a name="see-also"></a>Voir aussi  
[Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Scripts des tests unitaires SQL Server](../ssdt/scripts-in-sql-server-unit-tests.md)  
[Fichiers de tests unitaires SQL Server](../ssdt/sql-server-unit-test-files.md)  
  
