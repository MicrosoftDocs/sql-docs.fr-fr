---
title: Scripts des tests unitaires SQL Server
description: Familiarisez-vous avec les scripts de tests unitaires SQL Server. En savoir plus sur les scripts pré-test, de test et post-test ainsi que les scripts communs TestInitialize et TestCleanup.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 80c5cf62-a9c9-4e9d-8c6f-8eed50a595a7
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: eb725a491eb87a0116413a786c514a0ede98361f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066944"
---
# <a name="scripts-in-sql-server-unit-tests"></a>Scripts des tests unitaires SQL Server

Chaque test unitaire SQL Server contient une action unique d'avant test, de test, et d'après test. Chacune de ces actions contient à son tour les éléments suivants :  
  
-   un script Transact\-SQL qui s'exécute sur une base de données ;  
  
-   zéro ou plusieurs conditions de test qui évaluent les résultats retournés par l'exécution du script.  
  
Le script de test Transact\-SQL de l'action de test est le seul composant que vous devez inclure dans chaque test unitaire SQL Server. Outre le script de test lui-même, vous souhaiterez peut-être également spécifier des conditions de test pour vérifier si le script de test a retourné la valeur ou l'ensemble de valeurs attendues. L'action de test utilise ou modifie un objet spécifique dans la base de données, puis évalue cette modification.  
  
Pour chaque action de test, incluez une action d'avant test et une action d'après test. Semblable à l'action de test, chaque action d'avant test et chaque action d'après test contient un script Transact\-SQL et zéro ou plusieurs conditions de test. Utilisez une action d'avant test pour veiller à ce que la base de données soit dans un état qui permet l'exécution de l'action de test et le retour de résultats significatifs. Par exemple, utilisez un action d'avant test pour vérifier qu'une table contient des données avant que le script de test n'exécute une opération sur ces données. Après que l'action d'avant test a préparé la base de données et l'action de test a retourné des résultats significatifs, l'action d'après test peut être utilisée pour retourner la base de données dans l'état où elle se trouvait avant exécution de l'action d'avant test. Dans certains cas, utilisez l'action d'après test pour valider les résultats de l'action de test. En effet, l'action d'après test peut posséder des privilèges de base de données supérieurs à ceux de l'action de test. Pour plus d’informations, consultez [Vue d’ensemble des chaînes de connexion et des autorisations](../ssdt/overview-of-connection-strings-and-permissions.md).  
  
Outre ces trois actions, il existe également deux scripts de test (appelés scripts courants), exécutés avant et après une série de tests unitaires SQL Server. Par conséquent, jusqu'à cinq scripts Transact\-SQL peuvent être exécutés pendant l'exécution d'un seul test unitaire SQL Server. Seul le script Transact\-SQL contenu dans l'action de test est requis ; les scripts courants et les scripts d'avant test et d'après test sont facultatifs.  
  
Le tableau suivant fournit une liste complète des scripts associés à tout test unitaire SQL Server.  
  
|**Action**|**Type de script**|**Description**|  
|--------------|-------------------|-------------------|  
|TestInitialize|Script courant (initialisation)|(Facultatif) Ce script précède toutes les actions d'avant test et d'après test dans le test unitaire. Le script TestInitialize s'exécute avant chaque test unitaire dans une de test donnée. Ce script s'exécute en utilisant le contexte de privilèges.|  
|Avant le test|Script de test|(Facultatif) Ce script fait partie du test unitaire. Le script d'avant test s'exécute avant l'action de test dans un test unitaire. Ce script s'exécute en utilisant le contexte de privilèges.|  
|Test|Script de test|(Obligatoire) Ce script fait partie du test unitaire. Ce script peut, par exemple, exécuter une procédure stockée qui obtient, insère ou met à jour les valeurs d'une table. Ce script s'exécute en utilisant le contexte d'exécution.|  
|Après le test|Script de test|(Facultatif) Ce script fait partie du test unitaire. Le script d'après test s'exécute après un test unitaire individuel. Ce script s'exécute en utilisant le contexte de privilèges.|  
|TestCleanup|Script courant (nettoyage)|(Facultatif) Ce script suit le test unitaire. Le script TestCleanup s'exécute après tous les tests unitaires dans une classe de test donnée. Ce script s'exécute en utilisant le contexte de privilèges.|  
  
Pour plus d'informations sur les différents contextes de sécurité dans lesquels s'exécutent ces scripts, consultez [Vue d'ensemble des chaînes de connexion et des autorisations](../ssdt/overview-of-connection-strings-and-permissions.md) et la section Autorisations de test unitaire dans [Autorisations nécessaires pour SQL Server Data Tools](../ssdt/required-permissions-for-sql-server-data-tools.md).  
  
## <a name="order-in-which-scripts-are-run"></a>Ordre dans lequel les scripts sont exécutés  
Il est important de comprendre l'ordre dans lequel chaque script s'exécute. Bien que vous ne puissiez pas modifier cet ordre, vous pouvez décider des scripts que vous souhaitez exécuter. L'illustration suivante présente la sélection des scripts que vous pouvez utiliser dans une série de tests qui contient deux tests unitaires SQL Server et indique l'ordre dans lequel ils s'exécutent :  
  
![Tests de deux unités de base de données](../ssdt/media/twodatabaseunittests.png "Tests de deux unités de base de données")  
  
> [!NOTE]  
> Si un déploiement de projet de base de données SQL Server a été configuré, il a lieu au début de la série de tests, sous la chaîne de connexion de contexte privilégié. Pour plus d’informations, consultez [Procédure : configurer l’exécution de test unitaire SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md).  
  
## <a name="initialization-and-cleanup-scripts"></a>Scripts d'initialisation et de nettoyage  
Dans le Concepteur de test unitaire SQL Server, les scripts TestInitialize et TestCleanup sont appelés scripts courants. L'exemple précédent suppose que les deux tests unitaires font partie de la même classe de test. Par conséquent, ils partagent les mêmes scripts TestInitialize et TestCleanup. Pour tous les tests unitaires d'une classe unique de test, cela est toujours le cas. Toutefois, si votre série de tests contient des tests unitaires de différentes classes de test, les scripts courants de la classe de test associée seront exécutés avant et après l'exécution des tests unitaires.  
  
Si vous écrivez des tests unitaires uniquement à l'aide du Concepteur de test unitaire SQL Server, vous n'êtes peut-être pas familiarisé avec le concept de classe de test. Chaque fois que vous créez un test unitaire en ouvrant le menu **Test** et en cliquant sur **Nouveau test**, SQL Server Data Tools génère une classe de test. Les classes de test apparaissent dans l'**Explorateur de solutions** avec le nom du test que vous avez spécifié, suivi d'une extension .cs ou .vb. Dans chaque classe de test, les différents tests unitaires sont stockés en tant que méthodes de test. Toutefois, quel que soit le nombre de méthodes de test (autrement dit, de tests unitaires), chaque classe de test peut avoir zéro ou un script TestInitialize et TestCleanup.  
  
Utilisez le script TestInitialize pour préparer la base de données de test, et utilisez le script TestCleanup pour retourner la base de données de test dans un état connu. Par exemple, utilisez TestInitialize pour créer une procédure stockée d'assistance que vous exécuterez ultérieurement, dans le script de test, pour tester une autre procédure stockée.  
  
## <a name="pre-test-and-post-test-scripts"></a>Scripts d'avant test et d'après test  
Les scripts associés aux actions d'avant test et d'après test sont susceptibles de varier d'un test unitaire au suivant. Utilisez ces scripts pour générer des modifications incrémentielles dans la base de données, puis pour nettoyer ces modifications.  
  
## <a name="see-also"></a>Voir aussi  
[Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Utilisation de conditions de test dans les tests unitaires SQL Server](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)  
  
