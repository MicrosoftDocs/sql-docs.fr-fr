---
title: Exécuter des fichiers de script Transact-SQL à l'aide de sqlcmd
description: Découvrez comment utiliser sqlcmd pour exécuter un fichier de script Transact-SQL. Ce fichier peut contenir des instructions Transact-SQL, des commandes sqlcmd et des variables de script.
ms.custom: seo-lt-2019
ms.date: 07/15/2016
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- transact sql scripts
ms.assetid: 90067eb8-ca3e-44e8-bb1a-bf7d1a359423
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: edfa425e7cceba9264ae640239039105f6b366cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346659"
---
# <a name="sqlcmd---run-transact-sql-script-files"></a>sqlcmd - Exécuter des fichiers de script Transact-SQL
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 Utilisez **sqlcmd** pour exécuter un fichier de script Transact-SQL. Un fichier de script Transact-SQL est un fichier texte qui peut contenir une combinaison d’instructions Transact-SQL, de commandes **sqlcmd** et de variables de script.  

## <a name="create-a-script-file"></a>Créer un fichier de script  
 Pour créer un fichier de script Transact-SQL simple à l’aide du Bloc-notes, procédez comme suit :  
  
1.  Cliquez sur **Démarrer**, pointez sur **Tous les programmes**, sur **Accessoires**, puis cliquez sur **Bloc-notes**.  
  
2.  Copiez et collez le code Transact-SQL suivant dans le Bloc-notes :  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT p.FirstName + ' ' + p.LastName AS 'Employee Name',  
    a.AddressLine1, a.AddressLine2 , a.City, a.PostalCode   
    FROM Person.Person AS p   
       INNER JOIN HumanResources.Employee AS e   
            ON p.BusinessEntityID = e.BusinessEntityID  
        INNER JOIN Person.BusinessEntityAddress bea   
            ON bea.BusinessEntityID = e.BusinessEntityID  
        INNER JOIN Person.Address AS a   
            ON a.AddressID = bea.AddressID;  
    GO  
    ```  
  
3.  Enregistrez le fichier sous **myScript.sql** sur le lecteur C.  
  
## <a name="run-the-script-file"></a>Exécuter le fichier de script  
  
1.  Ouvrir une fenêtre d’invite de commandes.  
  
2.  Dans la fenêtre d’invite de commandes, tapez : **sqlcmd -S myServer\instanceName -i C:\myScript.sql**  
  
3.  Appuyez sur Entrée.  
  
 La liste des noms et des adresses des employés de la société [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] apparaît dans la fenêtre d'invite de commandes.  

## <a name="save-the-output-to-a-text-file"></a>Enregistrer la sortie dans un fichier texte
  
1.  Ouvrir une fenêtre d’invite de commandes.  
  
2.  Dans la fenêtre d’invite de commandes, tapez : **sqlcmd -S myServer\instanceName -i C:\myScript.sql -o C:\EmpAdds.txt**  
  
3.  Appuyez sur Entrée.  
  
 Aucun résultat n'est retourné dans la fenêtre d'invite de commandes. Le résultat est au contraire envoyé dans le fichier EmpAdds.txt. Vous pouvez vérifier ce résultat en ouvrant le fichier EmpAdds.txt.  
  
## <a name="see-also"></a>Voir aussi  
 [Démarrer l’utilitaire sqlcmd](./sqlcmd-start-the-utility.md)   
 [Utilitaire sqlcmd](../../tools/sqlcmd-utility.md)  
  
