---
title: Fichiers de tests unitaires SQL Server
description: Découvrez les fichiers qui composent un test d’unité SQL Server, tel que le fichier de code source, le fichier de ressources, le fichier de configuration et le fichier d’installation.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: cee093c9-b97d-4fb0-b80f-806d071259dc
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 48a8ba38c1af97f868162bdbc955006dbf646f55
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066894"
---
# <a name="sql-server-unit-test-files"></a>Fichiers de tests unitaires SQL Server

Comme les tests unitaires de code managé, les tests unitaires SQL Server résident dans des projets de test. Affichez les éléments qui composent un test unitaire SQL Server dans la hiérarchie d'un projet de test de l'**Explorateur de solutions**.  
  
Un test unitaire SQL Server est constitué de plusieurs éléments contenus dans plusieurs fichiers. Le tableau suivant décrit les fichiers qui interagissent pour constituer un test unitaire SQL Server.  
  
|**File**|**Description**|  
|------------|-------------------|  
|.cs ou .vb|Ce fichier de code source contient une classe décorée avec l'attribut [TestClass]. Cette classe contient une seule méthode de test pour les tests unitaires SQL Server à relation contenant-contenu. Ces méthodes sont décorées avec l'attribut [TestMethod].<br /><br />Chaque méthode de test contient le code approprié pour utiliser le script de test Transact\-SQL. Ce code est généré lorsque les méthodes de test sont créées, et vous pouvez le modifier.<br /><br />**REMARQUE :** si vous double-cliquez sur ce fichier dans l’**Explorateur de solutions**, la classe de test s’ouvre dans le Concepteur de test unitaire SQL Server. Pour ouvrir le fichier .cs ou .vb et afficher son code source, dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le fichier, puis cliquez sur **Afficher le code**.|  
|.resx|Ce fichier de ressources contient des scripts Transact\-SQL pour tous les tests figurant dans le fichier .cs ou .vb associé. Ce groupe de scripts inclut le script de prétest, le script de test et le script post-test. Le fichier de ressources contient le XML, que vous pouvez modifier. Le fichier de ressources est compilé dans l'assembly de test.<br /><br />Vous devez coder les scripts Transact\-SQL à l'aide du **Concepteur de test unitaire SQL Server**. Pour plus d’informations sur les scripts qui sont utilisés dans les tests unitaires SQL Server, consultez [Scripts dans les Tests unitaires SQL Server](../ssdt/scripts-in-sql-server-unit-tests.md).|  
|app.config|Ce fichier stocke les chaînes de connexion de base de données pour le projet de test, en plus des autres paramètres de configuration de test unitaire SQL Server, tels que le délai d'attente de commande. Pour plus d'informations, consultez [Scripts des tests unitaires SQL Server](../ssdt/scripts-in-sql-server-unit-tests.md).|  
|SQLDatabaseSetup.cs ou SQLDatabaseSetup.vb|Ce fichier contient une classe qui prépare l'environnement de test de tous les tests unitaires SQL Server du projet de test. Selon les paramètres de configuration dans le fichier app.config, un projet de base de données SQL Server peut être déployé dans la base de données de test.|  
  
## <a name="see-also"></a>Voir aussi  
[Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Vérifier le code de la base de données à l’aide de tests unitaires SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
[Scripts des tests unitaires SQL Server](../ssdt/scripts-in-sql-server-unit-tests.md)  
  
