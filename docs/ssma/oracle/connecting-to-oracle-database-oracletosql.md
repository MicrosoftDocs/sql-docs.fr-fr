---
title: Connexion à Oracle Database (OracleToSQL) | Microsoft Docs
description: Découvrez comment vous connecter à la base de données Oracle pour migrer cette base de données Oracle vers SQL Server. SSMA obtient et affiche des métadonnées sur tous les schémas Oracle.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 06/04/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Refreshing Oracle Metadata
ms.assetid: e276cdbf-3ebc-4ba8-b40d-a7a42befa2b6
ms.author: alexiva
ms.openlocfilehash: 4354a4e2726151177c48c27ce37a74e5b4ac335e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058795"
---
# <a name="connecting-to-oracle-database-oracletosql"></a>Connexion à Oracle Database (OracleToSQL)

Pour migrer des bases de données Oracle vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez vous connecter à la base de données Oracle que vous souhaitez migrer. Quand vous vous connectez, SSMA obtient des métadonnées sur tous les schémas Oracle, puis les affiche dans le volet de l’Explorateur de métadonnées Oracle. SSMA stocke les informations relatives au serveur de base de données, mais ne stocke pas les mots de passe.

Votre connexion à la base de données reste active jusqu’à ce que vous fermiez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter si vous souhaitez une connexion active à la base de données.

Les métadonnées relatives à la base de données Oracle ne sont pas automatiquement mises à jour. Au lieu de cela, si vous souhaitez mettre à jour les métadonnées dans l’Explorateur de métadonnées Oracle, vous devez les mettre à jour manuellement. Pour plus d’informations, consultez la section « actualisation des métadonnées Oracle » plus loin dans cette rubrique.

## <a name="required-oracle-permissions"></a>Autorisations Oracle requises

Au minimum, le compte utilisé pour se connecter à la base de données Oracle doit disposer des autorisations suivantes :

- `CONNECT`  
  Requis pour se connecter (créer une session) à la base de données.

- `SELECT ANY DICTIONARY`  
  Requis pour interroger les tables de dictionnaire système (par exemple, `SYS.MLOG$` ) afin de découvrir tous les objets.

Cela permettra à SSMA de charger tous les objets du schéma appartenant à l’utilisateur qui se connecte. Dans la plupart des scénarios réels, il existe des références entre schémas entre les procédures stockées et SSMA devra être en mesure de découvrir tous les objets référencés pour une conversion réussie. Pour obtenir les métadonnées des objets définis dans d’autres schémas, le compte doit disposer des autorisations supplémentaires suivantes :

- `SELECT ANY TABLE`  
  Requis pour découvrir des tables, des vues, des vues matérialisées et des synonymes dans d’autres schémas.

- `SELECT ANY SEQUENCE`  
  Requis pour découvrir des séquences dans d’autres schémas.

- `CREATE ANY PROCEDURE`  
  Requis pour découvrir PL/SQL pour les procédures, les fonctions et les packages dans d’autres schémas.

- `CREATE ANY TRIGGER`  
  Requis pour découvrir les définitions de déclencheur dans d’autres schémas.

- `CREATE ANY TYPE`  
  Requis pour découvrir les types définis dans d’autres schémas.

Certaines des fonctionnalités SSMA requièrent des autorisations supplémentaires. Par exemple, si vous souhaitez utiliser le [testeur](testing-migrated-database-objects-oracletosql.md) et la fonctionnalité de [gestion des sauvegardes](managing-backups-oracletosql.md) , vous devez accorder à votre utilisateur qui se connecte les éléments suivants :

- `EXECUTE ANY PROCEDURE`  
  Requis pour exécuter les procédures et les fonctions que vous souhaitez tester dans tous les schémas.

- `CREATE ANY TABLE` et `ALTER ANY TABLE`  
  Requis pour créer et modifier des tables temporaires pour le suivi des modifications et les sauvegardes.

- `INSERT ANY TABLE` et `UPDATE ANY TABLE`  
  Requis pour insérer les données de suivi des modifications et de sauvegarde dans des tables temporaires.

- `DROP ANY TABLE`  
  Requis pour supprimer les tables temporaires utilisées pour le suivi des modifications et les sauvegardes.

- `CREATE ANY INDEX` et `ALTER ANY INDEX`  
  Requis pour créer et modifier des index sur des tables temporaires utilisées pour le suivi des modifications et les sauvegardes.

- `DROP ANY INDEX`  
  Requis pour supprimer des index sur des tables temporaires utilisées pour le suivi des modifications et les sauvegardes.

- `CREATE ANY TRIGGER` et `ALTER ANY TRIGGER`  
  Requis pour créer et modifier les déclencheurs temporaires utilisés pour le suivi des modifications.

- `DROP ANY TRIGGER`  
  Requis pour supprimer les déclencheurs temporaires utilisés pour le suivi des modifications.

> [!NOTE]
> Il s’agit d’un jeu générique d’autorisations requises pour que SSMA fonctionne correctement. Si vous souhaitez limiter l’étendue de votre migration à un sous-ensemble de schémas, vous pouvez le faire en accordant les autorisations ci-dessus à l’ensemble limité d’objets, au lieu de `ALL` . Bien que cela soit possible, il peut être très difficile d’identifier correctement toutes les dépendances, empêchant ainsi le fonctionnement correct de SSMA. Il est vivement recommandé de respecter l’ensemble générique défini ci-dessus pour éliminer les éventuels problèmes d’autorisation pendant le processus de migration.

## <a name="establishing-a-connection-to-oracle"></a>Établissement d’une connexion à Oracle

Lorsque vous vous connectez à une base de données, SSMA lit les métadonnées de la base de données, puis ajoute ces métadonnées au fichier projet. Ces métadonnées sont utilisées par SSMA lorsqu’elle convertit des objets en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe, et lorsqu’elle migre des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez parcourir ces métadonnées dans le volet de l’Explorateur de métadonnées Oracle et consulter les propriétés des objets de base de données individuels.

> [!IMPORTANT]
> Avant d’essayer de vous connecter, assurez-vous que le serveur de base de données est en cours d’exécution et peut accepter des connexions.

**Pour se connecter à Oracle**

1. Dans le menu **fichier** , sélectionnez **se connecter à Oracle**.  
   Si vous vous êtes connecté précédemment à Oracle, le nom de la commande sera **reconnecté à Oracle**.
  
2. Dans la zone **fournisseur** , sélectionnez **fournisseur client Oracle** ou **fournisseur OLE DB**, selon le fournisseur installé. La valeur par défaut est client Oracle.

3. Dans la zone **mode** , sélectionnez mode **standard**, **mode TNSNAME** ou **mode chaîne de connexion**.  
   Utilisez le mode standard pour spécifier le nom du serveur et le port. Utilisez le mode nom du service pour spécifier le nom du service Oracle manuellement. Utilisez le mode chaîne de connexion pour fournir une chaîne de connexion complète.

4. Si vous sélectionnez le **mode standard**, indiquez les valeurs suivantes :
   1. Dans la zone **nom du serveur** , entrez ou sélectionnez le nom ou l’adresse IP du serveur de base de données.
   2. Si le serveur de base de données n’est pas configuré pour accepter les connexions sur le port par défaut (1521), entrez le numéro de port utilisé pour les connexions Oracle dans la zone **port du serveur** .
   3. Dans la zone **sid Oracle** , entrez l’identificateur du système.
   4. Dans la zone **nom d’utilisateur** , entrez un compte Oracle disposant des autorisations nécessaires.
   5. Dans la zone **mot de passe** , entrez le mot de passe du nom d’utilisateur spécifié.

5. Si vous sélectionnez le **mode TNSNAME**, indiquez les valeurs suivantes :
   1. Dans la zone **identificateur de connexion** , entrez identificateur de connexion (alias TNS) de la base de données.
   2. Dans la zone **nom d’utilisateur** , entrez un compte Oracle disposant des autorisations nécessaires.
   3. Dans la zone **mot de passe** , entrez le mot de passe du nom d’utilisateur spécifié.
  
6. Si vous sélectionnez le **mode de chaîne de connexion**, indiquez une chaîne de connexion dans la zone chaîne de **connexion** .  
   L’exemple suivant illustre une chaîne de connexion OLE DB :

   `Provider=OraOLEDB.Oracle;Data Source=MyOracleDB;User Id=myUsername;Password=myPassword;`

   L’exemple suivant montre une chaîne de connexion du client Oracle qui utilise la sécurité intégrée :

   `Data Source=MyOracleDB;Integrated Security=yes;`

   Pour plus d’informations, consultez [se connecter à Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/connect-to-oracle-oracletosql.md).

## <a name="reconnecting-to-oracle"></a>Reconnexion à Oracle

Votre connexion au serveur de base de données reste active jusqu’à ce que vous fermiez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter si vous souhaitez une connexion active à la base de données. Vous pouvez travailler hors connexion jusqu’à ce que vous souhaitiez mettre à jour des métadonnées, charger des objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et migrer des données.

## <a name="refreshing-oracle-metadata"></a>Actualisation des métadonnées Oracle

Les métadonnées relatives à la base de données Oracle ne sont pas automatiquement actualisées. Dans l’Explorateur de métadonnées Oracle, les métadonnées sont un instantané des métadonnées lorsque vous vous êtes connecté pour la première fois ou lors de la dernière actualisation manuelle des métadonnées. Vous pouvez mettre à jour manuellement les métadonnées de tous les schémas, un seul schéma ou des objets de base de données individuels.

**Pour actualiser les métadonnées**

1. Assurez-vous que vous êtes connecté à la base de données.

2. Dans l’Explorateur de métadonnées Oracle, activez la case à cocher en regard de chaque schéma ou objet de base de données que vous souhaitez mettre à jour.

3. Cliquez avec le bouton droit sur **schémas**, ou sur l’objet de schéma ou de base de données, puis sélectionnez **Actualiser à partir de la base de données**.  
   Si vous ne disposez pas d’une connexion active, SSMA affiche la boîte de dialogue **connexion à Oracle** pour vous permettre de vous connecter.

4. Dans la boîte de dialogue actualiser à partir de la base de données, spécifiez les objets à actualiser.

   - Pour actualiser un objet, cliquez sur le champ **actif** adjacent à l’objet jusqu’à ce qu’une flèche s’affiche.
   - Pour empêcher l’actualisation d’un objet, cliquez sur le champ **actif** adjacent à l’objet jusqu’à ce qu’un **X** apparaisse.
   - Pour actualiser ou refuser une catégorie d’objets, cliquez sur le champ **actif** en regard du dossier de catégorie.

   Pour afficher les définitions du codage en couleurs, cliquez sur le bouton **légende** .

5. [!INCLUDE[clickOK](../../includes/clickok-md.md)]

## <a name="next-steps"></a>Étapes suivantes

L’étape suivante du processus de migration consiste à [se connecter à une instance de SQL Server](connecting-to-sql-server-oracletosql.md).

## <a name="see-also"></a>Voir également

[Migration de bases de données Oracle vers SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)
