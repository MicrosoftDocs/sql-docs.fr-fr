---
title: Créer des tests unitaires SQL Server pour des fonctions, des déclencheurs ou des procédures stockées
description: Découvrez comment utiliser l’Explorateur d’objets SQL Server pour créer un test unitaire SQL Server à partir d’une fonction de base de données, d’un déclencheur ou d’une procédure stockée.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: bda57c10-a1ab-4a1a-8a71-42085a3cb793
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2f99946dfb56e3fc1e3e67f42ca93ff5daa11cab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018269"
---
# <a name="how-to-create-sql-server-unit-tests-for-functions-triggers-and-stored-procedures"></a>Procédure : Créer des tests unitaires SQL Server pour des fonctions, des déclencheurs ou des procédures stockées

Écrivez des tests unitaires qui évaluent les modifications apportées à un objet de base de données. Toutefois, SQL Server Data Tools inclut la prise en charge supplémentaire de la création des tests de fonctions de base de données, des déclencheurs et des procédures stockées à partir d'un nœud de projet de base de données dans l'Explorateur d'objets SQL Server. Les stubs de code Transact\-SQL peuvent être générés automatiquement pour que vous puissiez les personnaliser.  
  
### <a name="to-create-a-sql-server-unit-test-from-a-function-trigger-or-stored-procedure"></a>Pour créer un test unitaire SQL Server d'une fonction, d'un déclencheur ou d'une procédure stockée  
  
1.  Consultez [Procédure pas à pas : création et exécution d'un test unitaire SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md) pour obtenir un exemple d'ajout de test unitaire pour une procédure stockée, dans la section « Pour créer un test unitaire SQL Server pour les procédures stockées ».  
  
    La condition de test Non concluant est la condition par défaut qui est ajoutée à chaque test. Cette condition de test est incluse pour indiquer que la vérification du test n'a pas été implémentée. Supprimez cette condition de test de votre test après avoir ajouté d'autres conditions de test. Pour plus d’informations, consultez [Procédure : ajouter des conditions de test à des tests unitaires SQL Server](../ssdt/how-to-add-test-conditions-to-sql-server-unit-tests.md).  
  
## <a name="see-also"></a>Voir aussi  
[Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Procédure : créer un test unitaire SQL Server vide](../ssdt/how-to-create-an-empty-sql-server-unit-test.md)  
  
