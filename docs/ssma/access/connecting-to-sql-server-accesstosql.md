---
title: Connexion à SQL Server (AccessToSQL) | Microsoft Docs
description: Découvrez comment vous connecter à une instance cible de SQL Database pour migrer des bases de données Access. SSMA obtient des métadonnées sur les bases de données dans SQL Database.
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- authentication
- instance of SQL Server
- metadata, refreshing
- ports
- refreshing metadata
- spaces in database names
- special characters
- SQL Server
- SQL Server, connecting
- SQL Server, connecting to
- SQL Server, reconnecting
ms.assetid: f84cf007-ddf1-4396-a07c-3e0729abc769
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 59583057641bb167aae197dd9fae3bf6e5833302
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076180"
---
# <a name="connecting-to-sql-server-accesstosql"></a>Connexion à SQL Server (AccessToSQL)

Pour migrer des bases de données Access vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez vous connecter à l’instance cible du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Quand vous vous connectez, SSMA obtient les métadonnées relatives aux bases de données dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et affiche les métadonnées de la base de données dans **SQL Server Explorateur de métadonnées**. SSMA stocke des informations sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle vous êtes connecté, mais ne stocke pas les mots de passe.

Votre connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reste active jusqu’à ce que vous fermiez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si vous souhaitez une connexion active au serveur. Vous pouvez travailler hors connexion jusqu’à ce que vous chargeant des objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et migriez les données.

Les métadonnées relatives à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne sont pas synchronisées automatiquement. Au lieu de cela, pour mettre à jour les métadonnées dans SQL Server Explorateur de métadonnées, vous devez mettre à jour manuellement les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] métadonnées. Pour plus d’informations, consultez la section « synchronisation des métadonnées de SQL Server » plus loin dans cette rubrique.

## <a name="required-sql-server-permissions"></a>Autorisations de SQL Server requises

Le compte utilisé pour se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiert des autorisations différentes en fonction des actions effectuées par le compte :

- Pour convertir des objets Access en [!INCLUDE[tsql](../../includes/tsql-md.md)] syntaxe, pour mettre à jour des métadonnées à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou pour enregistrer la syntaxe convertie dans des scripts, le compte doit avoir l’autorisation de se connecter à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- Pour charger des objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le compte doit être membre du rôle de base de données **db_ddladmin** .

- Pour migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le compte doit être membre du rôle de base de données **db_owner** .

## <a name="establishing-a-sql-server-connection"></a>Établissement d’une connexion SQL Server

Avant de convertir des objets de base de données Access en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe, vous devez établir une connexion à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle vous souhaitez migrer les bases de données Access.

Lorsque vous définissez les propriétés de connexion, vous spécifiez également la base de données dans laquelle les objets et les données seront migrés. Vous pouvez personnaliser ce mappage au niveau de la base de données Access après vous être connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [mappage des bases de données source et cible](mapping-source-and-target-databases-accesstosql.md).

> [!IMPORTANT]
> Avant de vous connecter à, assurez-vous [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d’exécution et peut accepter des connexions.

Pour se connecter au [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :

1. Dans le menu **fichier** , sélectionnez **se connecter à SQL Server**.
   Si vous vous êtes connecté précédemment à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le nom de la commande sera **reconnecté à SQL Server**.

2. Dans la zone **nom du serveur** , entrez ou sélectionnez le nom de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
   - Si vous vous connectez à l’instance par défaut sur l’ordinateur local, vous pouvez entrer `localhost` ou un point ( `.` ).
   - Si vous vous connectez à l’instance par défaut sur un autre ordinateur, entrez le nom de l’ordinateur.
   - Si vous vous connectez à une instance nommée, entrez le nom de l’ordinateur, une barre oblique inverse et le nom de l’instance. Par exemple : `MyServer\MyInstance`.
   - Pour vous connecter à une instance utilisateur active de [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)] , connectez-vous à l’aide du protocole de canaux nommés et en spécifiant le nom du canal, par exemple `\\.\pipe\sql\query` . Pour plus d’informations, consultez la documentation de [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)].

3. Si votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est configurée pour accepter les connexions sur un port autre que celui par défaut, entrez le numéro de port utilisé pour les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexions dans la zone **port du serveur** . Pour l’instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le numéro de port par défaut est 1433. Pour les instances nommées, SSMA essaiera d’obtenir le numéro de port auprès du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Browser.

4. Dans la zone **base de données** , entrez le nom de la base de données cible pour la migration des données et des objets.
   Cette option n’est pas disponible lors de la reconnexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
   Le nom de la base de données cible ne peut pas contenir d’espaces ou de caractères spéciaux. Par exemple, vous pouvez migrer des bases de données Access vers une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données nommée `abc` . Toutefois, vous ne pouvez pas migrer des bases de données Access vers une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données nommée `a b-c` .
   Vous pouvez personnaliser ce mappage par base de données une fois que vous êtes connecté. Pour plus d’informations, consultez [mappage des bases de données source et cible](mapping-source-and-target-databases-accesstosql.md) .

5. Dans le menu déroulant **authentification** , sélectionnez le type d’authentification à utiliser pour la connexion. Pour utiliser le compte Windows actuel, sélectionnez **authentification Windows**. Pour utiliser une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion, sélectionnez **SQL Server l’authentification**, puis entrez un nom d’utilisateur et un mot de passe.

6. Pour une connexion sécurisée, la case à cocher **chiffrer la connexion** et la case à cocher **TrustServerCertificate** s’ajoutent. Cette **case à cocher est activée uniquement** lorsque la case **chiffrer la connexion** est cochée. Lorsque l’option **chiffrer la connexion** est activée (true) et la valeur **TrustServerCertificate** est désactivée (false), valide le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] certificat SSL. La validation du certificat de serveur est une partie de la négociation SSL qui garantit qu'il s'agit du serveur correct avec lequel établir une connexion. Pour ce faire, un certificat doit être installé côté client et du côté serveur.

7. Cliquez sur **Connecter**.

> [!IMPORTANT]
> Si vous pouvez vous connecter à une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , par rapport à la version choisie lors de la création du projet de migration, la conversion des objets de base de données est déterminée par la version cible du projet et non par la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle vous êtes connecté.

## <a name="synchronizing-sql-server-metadata"></a>Synchronisation des métadonnées de SQL Server

Si les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schémas changent après la connexion, vous pouvez synchroniser les métadonnées avec le serveur.

Pour synchroniser SQL Server métadonnées, **SQL Server l’Explorateur de métadonnées**, cliquez avec le bouton droit sur **bases de données**, puis sélectionnez **synchroniser avec la base de données**.

## <a name="reconnecting-to-sql-server"></a>Reconnexion à SQL Server

Votre connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reste active jusqu’à ce que vous fermiez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si vous souhaitez une connexion active au serveur. Vous pouvez travailler hors connexion jusqu’à ce que vous chargeant des objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et migriez les données.

La procédure de reconnexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est identique à celle de la procédure d’établissement d’une connexion.

## <a name="next-steps"></a>Étapes suivantes

Si vous souhaitez personnaliser le mappage entre les bases de données source et cible, consultez [mappage des bases de données source et cible](mapping-source-and-target-databases-accesstosql.md) . dans le cas contraire, l’étape suivante consiste à convertir les objets de base de données en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe à l’aide de l' [objet Convert Database Objects](converting-access-database-objects-accesstosql.md).

## <a name="see-also"></a>Voir aussi

[Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)
