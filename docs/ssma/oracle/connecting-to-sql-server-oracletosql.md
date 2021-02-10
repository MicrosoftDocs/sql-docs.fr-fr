---
title: Connexion à SQL Server (OracleToSQL) | Microsoft Docs
description: Découvrez comment vous connecter à SQL Server pour migrer une base de données Oracle. SSMA obtient et affiche les métadonnées des bases de données dans SQL Server.
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connecting to SQL Server,Synchronizing SQL Server Metadata
ms.assetid: 1b2a8059-1829-4904-a82f-9c06de1e245f
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: e9dbff4115cf58723d186f1024f06d39c2eab1b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058804"
---
# <a name="connecting-to-sql-server-oracletosql"></a>Connexion à SQL Server (OracleToSQL)

Pour migrer des bases de données Oracle vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez vous connecter à l’instance cible du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Quand vous vous connectez, SSMA obtient les métadonnées relatives à toutes les bases de données dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et affiche les métadonnées de la base de données dans l' **Explorateur de métadonnées SQL Server**. SSMA stocke des informations sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle vous êtes connecté, mais ne stocke pas les mots de passe.

Votre connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reste active jusqu’à ce que vous fermiez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si vous souhaitez une connexion active au serveur. Vous pouvez travailler hors connexion jusqu’à ce que vous chargeant des objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et migriez les données.

Les métadonnées relatives à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne sont pas synchronisées automatiquement. Au lieu de cela, pour mettre à jour les métadonnées dans **SQL Server Explorateur de métadonnées**, vous devez mettre à jour manuellement les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] métadonnées. Pour plus d’informations, consultez la section « synchronisation des métadonnées de SQL Server » plus loin dans cette rubrique.

## <a name="required-sql-server-permissions"></a>Autorisations de SQL Server requises

Le compte utilisé pour se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiert des autorisations différentes en fonction des actions effectuées par le compte :

- Pour convertir des objets Oracle en [!INCLUDE[tsql](../../includes/tsql-md.md)] syntaxe, pour mettre à jour des métadonnées à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou pour enregistrer la syntaxe convertie dans des scripts, le compte doit avoir l’autorisation de se connecter à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- Pour charger des objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le compte doit être membre du rôle de base de données **db_ddladmin** .

- Pour migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le compte doit être :
  - Membre du rôle de base de données **db_owner** , si vous utilisez le moteur de migration de données côté client.
  - Membre du rôle de serveur **sysadmin** , si vous utilisez le moteur de migration de données côté serveur. Cela est nécessaire pour créer l’étape de travail de l' `CmdExec` [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agent pendant la migration des données afin d’exécuter l’outil de copie en bloc SSMA.
    > [!NOTE]
    > [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les comptes proxy de l’agent ne sont pas pris en charge par la migration des données côté serveur.

- Pour exécuter le code généré par SSMA, le compte doit avoir les `EXECUTE` autorisations pour toutes les fonctions définies par l’utilisateur dans le schéma **ssma_oracle** de la base de données cible. Ces fonctions fournissent des fonctionnalités équivalentes des fonctions système Oracle et sont utilisées par les objets convertis.

## <a name="establishing-a-sql-server-connection"></a>Établissement d’une connexion SQL Server

Avant de convertir des objets de base de données Oracle en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe, vous devez établir une connexion à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle vous souhaitez migrer la ou les bases de données Oracle.

Lorsque vous définissez les propriétés de connexion, vous spécifiez également la base de données dans laquelle les objets et les données seront migrés. Vous pouvez personnaliser ce mappage au niveau du schéma Oracle après vous être connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [mappage de schémas Oracle à des schémas de SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/mapping-oracle-schemas-to-sql-server-schemas-oracletosql.md).

> [!IMPORTANT]
> Avant d’essayer de vous connecter à, assurez-vous [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d’exécution et peut accepter des connexions.

Pour vous connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :

1. Dans le menu **fichier** , sélectionnez **se connecter à SQL Server**.
   Si vous vous êtes connecté précédemment à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le nom de la commande sera **reconnecté à SQL Server**.

2. Dans la boîte de dialogue connexion, entrez ou sélectionnez le nom de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
   - Si vous vous connectez à l’instance par défaut sur l’ordinateur local, vous pouvez entrer `localhost` ou un point ( `.` ).
   - Si vous vous connectez à l’instance par défaut sur un autre ordinateur, entrez le nom de l’ordinateur.
   - Si vous vous connectez à une instance nommée sur un autre ordinateur, entrez le nom de l’ordinateur suivi d’une barre oblique inverse, puis du nom de l’instance, par exemple `MyServer\MyInstance` .

3. Si votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est configurée pour accepter les connexions sur un port autre que celui par défaut, entrez le numéro de port utilisé pour les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexions dans la zone **port du serveur** . Pour l’instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le numéro de port par défaut est 1433. Pour les instances nommées, SSMA essaiera d’obtenir le numéro de port auprès du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Browser.

4. Dans la zone **base de données** , entrez le nom de la base de données cible.
   Cette option n’est pas disponible lorsque vous vous reconnectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

5. Dans la zone **authentification** , sélectionnez le type d’authentification à utiliser pour la connexion. Pour utiliser le compte Windows actuel, sélectionnez **authentification Windows**. Pour utiliser une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion, sélectionnez **SQL Server l’authentification**, puis indiquez le nom de connexion et le mot de passe.

6. Pour une connexion sécurisée, deux contrôles sont ajoutés, les cases à cocher **chiffrer la connexion** et **TrustServerCertificate** . La case à cocher **TrustServerCertificate** est visible uniquement lorsque l’option **chiffrer la connexion** est activée. Lorsque l’option **chiffrer la connexion** est activée (true) et la valeur **TrustServerCertificate** est désactivée (false), la validation du certificat SSL est validée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La validation du certificat de serveur est une partie de la négociation SSL qui garantit qu'il s'agit du serveur correct avec lequel établir une connexion. Pour ce faire, un certificat doit être installé côté client et du côté serveur.

7. Cliquez sur **Connecter**.

> [!IMPORTANT]
> Si vous pouvez vous connecter à une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , par rapport à la version choisie lors de la création du projet de migration, la conversion des objets de base de données est déterminée par la version cible du projet et non par la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à laquelle vous êtes connecté.

## <a name="synchronizing-sql-server-metadata"></a>Synchronisation des métadonnées de SQL Server

Les métadonnées relatives aux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données ne sont pas automatiquement mises à jour. Les métadonnées dans **SQL Server Explorateur de métadonnées** sont un instantané des métadonnées lors de la première connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ou la dernière fois que vous avez mis à jour manuellement les métadonnées. Vous pouvez mettre à jour manuellement les métadonnées de toutes les bases de données ou d’une base de données ou d’un objet de base de données unique. Pour synchroniser les métadonnées :

1. Assurez-vous que vous êtes connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

2. Dans **SQL Server Explorateur de métadonnées**, activez la case à cocher en regard de la base de données ou du schéma de base de données que vous souhaitez mettre à jour.
   Par exemple, pour mettre à jour les métadonnées de toutes les bases de données, activez la case à cocher en regard de **bases de données**.

3. Cliquez avec le bouton droit sur **bases** de données, ou sur la base de données individuelle ou le schéma de base de données, puis sélectionnez **synchroniser avec la base de** données.
  
## <a name="next-step"></a>étape suivante

L’étape suivante de la migration dépend des besoins de votre projet :
  
- Pour personnaliser le mappage entre les schémas Oracle et les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données et les schémas, consultez [mappage de schémas Oracle à des schémas de SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/mapping-oracle-schemas-to-sql-server-schemas-oracletosql.md).
- Pour personnaliser les options de configuration des projets, consultez [définition des options de projet &#40;OracleToSQL&#41;](../../ssma/oracle/setting-project-options-oracletosql.md).
- Pour personnaliser le mappage des types de données sources et cibles, consultez [mappage des types de données Oracle et SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/mapping-oracle-and-sql-server-data-types-oracletosql.md).
- Si vous n’avez pas besoin d’effectuer ces tâches, vous pouvez convertir les définitions d’objet de base de données Oracle en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définitions d’objets. Pour plus d’informations, consultez [conversion de schémas Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/converting-oracle-schemas-oracletosql.md).
  
## <a name="see-also"></a>Voir aussi

[Migration de bases de données Oracle vers SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)
