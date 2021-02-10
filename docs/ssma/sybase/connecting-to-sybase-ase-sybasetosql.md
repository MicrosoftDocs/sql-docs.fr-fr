---
title: Connexion à SAP ASE (SybaseToSQL) | Microsoft Docs
description: Découvrez comment vous connecter à un serveur Adaptive Server pour migrer une base de données SAP Adaptive Server Enterprise (ASE) vers SQL Server ou Azure SQL Database.
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connecting to Sybase
ms.assetid: a45a2330-9175-4c9e-af38-ef920e350614
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 7afb893d194b874da4cbb6482df4ba8a16e8c0a7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056864"
---
# <a name="connecting-to-sap-ase-sybasetosql"></a>Connexion à SAP ASE (SybaseToSQL)

Pour migrer des bases de données SAP Adaptive Server Enterprise (ASE) vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, vous devez vous connecter au serveur Adaptive Server qui contient les bases de données que vous souhaitez migrer. Lorsque vous vous connectez, SSMA obtient les métadonnées relatives à toutes les bases de données sur le serveur Adaptive Server et affiche les métadonnées de la base de données dans le volet de l’Explorateur de métadonnées Sybase. SSMA stocke les informations relatives au serveur de base de données, mais ne stocke pas les mots de passe.  
  
Votre connexion à l’ASE reste active jusqu’à ce que vous fermiez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter à ASE si vous souhaitez une connexion active au serveur.  
  
Les métadonnées relatives au serveur adaptatif ne sont pas mises à jour automatiquement. Au lieu de cela, si vous souhaitez mettre à jour les métadonnées dans l’Explorateur de métadonnées Sybase, vous devez mettre à jour manuellement les métadonnées, comme décrit dans la section « actualisation des métadonnées Sybase ASE » plus loin dans cette rubrique.  
  
## <a name="required-ase-permissions"></a>Autorisations ASE requises

Le compte utilisé pour se connecter à l’ASE doit avoir au moins un accès **public** à la base de données master et à toutes les bases de données sources à migrer vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. En outre, pour sélectionner des autorisations sur les tables en cours de migration, l’utilisateur doit disposer des autorisations SELECT sur les tables système suivantes :  
  
- [source_db] objets .dbo.sys  
- [source_db] colonnes .dbo.sys  
- [source_db] .dbo.sysdes utilisateurs  
- types de .dbo.sys[source_db]  
- [source_db] contraintes .dbo.sys  
- [source_db] .dbo.sysdes commentaires  
- [source_db] index .dbo.sys  
- [source_db] références .dbo.sys  
- Bases de données master.dbo.sys  
  
## <a name="establishing-a-connection-to-ase"></a>Établissement d’une connexion à ASE

Quand vous vous connectez à un serveur Adaptive Server, SSMA lit les métadonnées de la base de données sur le serveur de base de données, puis ajoute ces métadonnées au fichier projet. Ces métadonnées sont utilisées par SSMA lorsqu’il convertit les objets vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure syntaxe, et lorsqu’il migre des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. Vous pouvez parcourir ces métadonnées dans le volet de l’Explorateur de métadonnées Sybase et consulter les propriétés des objets de base de données individuels.  
  
> [!IMPORTANT]  
> Avant d’essayer de vous connecter au serveur de base de données, assurez-vous que le serveur de base de données est en cours d’exécution et peut accepter des connexions.  
  
**Pour se connecter à Sybase ASE**
  
1. Dans le menu **fichier** , sélectionnez **se connecter à Sybase**.  
  
   Si vous vous êtes connecté précédemment à Sybase, le nom de la commande sera **reconnecté à Sybase**.  
  
2. Dans la zone **fournisseur** , sélectionnez l’un des fournisseurs installés sur l’ordinateur pour vous connecter à Sybase Server.  
  
3. Dans la zone **mode** , sélectionnez mode **standard** ou **mode avancé**.  
  
   Utilisez le mode standard pour spécifier le nom du serveur, le port, le nom d’utilisateur et le mot de passe. Utilisez le mode avancé pour fournir une chaîne de connexion. Ce mode est généralement utilisé uniquement pour le dépannage ou l’utilisation du support technique.  
  
4. Si vous sélectionnez le **mode standard**, indiquez les valeurs suivantes :  
  
    1. Dans la zone **nom du serveur** , entrez ou sélectionnez le nom ou l’adresse IP du serveur de base de données.  
    2. Si le serveur de base de données n’est pas configuré pour accepter les connexions sur le port par défaut (5000), entrez le numéro de port utilisé pour les connexions Sybase dans la zone **port du serveur** .  
    3. Dans la zone **nom d’utilisateur** , entrez un compte Sybase disposant des autorisations nécessaires.  
    4. Dans la zone **mot de passe** , entrez le mot de passe du nom d’utilisateur spécifié.  
  
5. Si vous sélectionnez le **mode avancé**, indiquez une chaîne de connexion dans la zone **chaîne de connexion** .  
  
    Voici des exemples de chaînes de connexion différentes :  
  
    1. **Chaînes de connexion pour le fournisseur de OLE DB Sybase :**  
  
        Pour Sybase ASE OLE DB 12,5, un exemple de chaîne de connexion est le suivant :  
  
        `Server Name=sybserver;User ID=MyUserID;Password=MyP@$$word;Provider=Sybase.ASEOLEDBProvider;`  
  
        Pour Sybase ASE OLE DB 15, un exemple de chaîne de connexion est le suivant :  
  
        `Server=sybserver;User ID=MyUserID;Password=MyP@$$word;Provider= ASEOLEDB;Port=5000;`  
  
    2. **Chaîne de connexion pour le fournisseur ODBC Sybase :**  
  
       `Driver=Adaptive Server Enterprise;Server=sybserver;uid=MyUserID;pwd=MyP@$$word;Port=5000;`  
  
    3. **Chaîne de connexion pour le fournisseur Sybase ADO.NET :**  
  
       `Server=sybserver;Port=5000;uid=MyUserID;pwd=MyP@$$word;`  
  
    Pour plus d’informations, consultez [se connecter à Sybase &#40;SybaseToSQL&#41;](../../ssma/sybase/connect-to-sybase-sybasetosql.md).  
  
## <a name="reconnecting-to-sybase-ase"></a>Reconnexion à Sybase ASE

Votre connexion au serveur de base de données reste active jusqu’à ce que vous fermiez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter si vous souhaitez une connexion active au serveur adaptatif. Vous pouvez travailler hors connexion jusqu’à ce que vous souhaitiez mettre à jour des métadonnées, charger des objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure et migrer des données.  
  
## <a name="refreshing-sybase-ase-metadata"></a>Actualisation des métadonnées de Sybase ASE

Les métadonnées relatives aux bases de données ASE ne sont pas automatiquement actualisées. Les métadonnées de l’Explorateur de métadonnées de Sybase sont un instantané des métadonnées lors de la première connexion au serveur Adaptive Server, ou la dernière fois que vous avez actualisé manuellement les métadonnées. Vous pouvez mettre à jour manuellement les métadonnées d’une base de données unique, d’un schéma de base de données unique ou de toutes les bases de données.  
  
**Pour actualiser les métadonnées**
  
1. Assurez-vous que vous êtes connecté au serveur Adaptive Server.  
  
2. Dans l’Explorateur de métadonnées Sybase, activez la case à cocher en regard de la base de données ou du schéma de base de données que vous souhaitez mettre à jour.  
  
3. Cliquez avec le bouton droit sur bases de données ou sur la base de données individuelle ou le schéma de base de données, puis sélectionnez **Actualiser dans la base de données**.  
  
4. Si vous êtes invité à vérifier l’objet actif, cliquez sur **Oui**.  
  
## <a name="next-step"></a>étape suivante  
  
- L’étape suivante du processus de migration consiste à [se connecter à une instance de SQL Server](connecting-to-sql-server-sybasetosql.md)  /  [se connectant à une instance de SQL Azure](connecting-to-azure-sql-db-sybasetosql.md)  
  
## <a name="see-also"></a>Voir aussi

[Migration de bases de données Sybase ASE vers SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
