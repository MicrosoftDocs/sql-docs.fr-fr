---
description: Exécution du script SQL Carnet d’adresses
title: Exécution du script SQL du carnet d’adresses | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- address book application scenario [ADO]
- RDS scenarios [ADO]
ms.assetid: 409b3f8b-0ced-4867-acbe-b245dcdf6702
author: rothja
ms.author: jroth
ms.openlocfilehash: 8e36c60e0c3e56ab0c159c362e98c2280e6c5426
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036389"
---
# <a name="running-the-address-book-sql-script"></a>Exécution du script SQL Carnet d’adresses
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 Vous devez utiliser l’utilitaire de ligne de commande ISQL/Query Analyzer ou le gestionnaire de SQL Server Entreprise pour exécuter le script SQL inclus (Sampleemp. Sql) qui :  
  
-   Crée une nouvelle base de données, AddrBookDB, sur l’appareil par défaut.  
  
-   Établit une connexion à la base de données, AddrBookDB.  
  
-   Crée une table Employee.  
  
-   Remplit la table avec des exemples de données.  
  
-   Exécute une instruction SELECT simple pour vérifier le remplissage de la table de base de données.  
  
-   Configure un compte d’utilisateur appelé « adcdemo » avec le mot de passe « adcdemo ».  
  
#### <a name="to-run-the-sampleempsql-script-in-microsoft-sql-server-65"></a>Pour exécuter le script Sampleemp. SQL dans Microsoft SQL Server 6,5  
  
1.  Cliquez sur **Démarrer**, pointez sur **programmes**, puis sur **Microsoft SQL Server 6,5**. Cliquez sur **SQL Enterprise Manager**.  
  
2.  Dans le menu **Outils** , cliquez sur **outil de requête SQL**.  
  
3.  Cliquez sur **charger le script SQL** et accédez à c:\Platform SDK\Samples\DataAccess\RDS\AddressBook.  
  
4.  Sélectionnez le fichier Sampleemp. Sql. Cliquez sur **Ouvrir**.  
  
5.  Cliquez sur le bouton **exécuter la requête** (flèche verte dans la barre d’outils).  
  
6.  Après l’exécution, fermez les fenêtres **requête** et **Enterprise Manager** .  
  
#### <a name="to-run-the-sampleempsql-script-in-microsoft-sql-server-70"></a>Pour exécuter le script Sampleemp. SQL dans Microsoft SQL Server 7,0  
  
1.  Cliquez sur **Démarrer**, pointez sur **programmes**, puis sur **Microsoft SQL Server 7,0**. Cliquez sur **Enterprise Manager**.  
  
2.  Assurez-vous que le SQL Server que vous souhaitez utiliser est sélectionné dans la liste des serveurs inscrits dans Enterprise Manager.  
  
3.  Dans le menu **Outils** , cliquez sur **SQL Server analyseur de requêtes**.  
  
4.  Cliquez sur le bouton **charger le script SQL** (ouvrir le dossier dans la barre d’outils) et accédez à c:\Platform SDK\Samples\DataAccess\RDS\AddressBook.  
  
5.  Sélectionnez le fichier Sampleemp. Sql. Cliquez sur **Ouvrir**.  
  
6.  Cliquez sur le bouton **exécuter la requête** (flèche verte dans la barre d’outils) ou sur **F5**.  
  
7.  Après l’exécution, fermez la **requête**, l' **Analyseur de requêtes** et les fenêtres **Enterprise Manager** .  
  
## <a name="see-also"></a>Voir aussi  
 [Exécution de l’exemple d’application Carnet d’adresses](./running-the-address-book-sample-application.md)