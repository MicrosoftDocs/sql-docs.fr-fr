---
title: Connexion à la base de données DB2 (DB2ToSQL) | Microsoft Docs
description: Découvrez comment vous connecter à une instance cible de la base de données DB2 pour migrer des bases de données DB2. SSMA obtient des métadonnées sur tous les schémas DB2.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 5eb5801d-f0c3-4127-97c0-0b1ef49f4844
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b7a2bd0434e865e3748861b94b21d44615e07ed5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081610"
---
# <a name="connecting-to-db2-database-db2tosql"></a>Connexion à la base de données DB2 (DB2ToSQL)

Pour migrer des bases de données DB2 vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez vous connecter à la base de données DB2 que vous souhaitez migrer. Quand vous vous connectez, SSMA obtient des métadonnées sur tous les schémas DB2, puis les affiche dans le volet de l’Explorateur de métadonnées DB2. SSMA stocke les informations relatives au serveur de base de données, mais ne stocke pas les mots de passe.

Votre connexion à la base de données reste active jusqu’à ce que vous fermiez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter si vous souhaitez une connexion active à la base de données.

Les métadonnées relatives à la base de données DB2 ne sont pas automatiquement mises à jour. Au lieu de cela, si vous souhaitez mettre à jour les métadonnées dans l’Explorateur de métadonnées DB2, vous devez les mettre à jour manuellement. Pour plus d’informations, consultez la section « actualisation des métadonnées DB2 » plus loin dans cette rubrique.

## <a name="required-db2-permissions"></a>Autorisations DB2 requises

Autorisation de l’utilisateur définit la liste des commandes et des objets qui sont disponibles pour un utilisateur. Cette liste permet de contrôler les actions des utilisateurs. Dans DB2, il existe des groupes de privilèges prédéterminés pour l’autorisation, à la fois au niveau de l’instance et au niveau d’une base de données DB2. Cela permet à SSMA d’obtenir des métadonnées à partir de schémas appartenant à l’utilisateur qui se connecte. Pour obtenir les métadonnées des objets dans d’autres schémas, puis convertir les objets dans ces schémas, le compte doit disposer des autorisations suivantes :

- L’accès au schéma pour la migration de schéma est normalement accordé à PUBLIC, sauf si le mot clé Restrict a été utilisé dans CREATe
- L’accès aux données pour la migration des données nécessite DATAACCESS

## <a name="establishing-a-connection-to-db2"></a>Établissement d’une connexion à DB2

Lorsque vous vous connectez à une base de données, SSMA lit les métadonnées de la base de données, puis ajoute ces métadonnées au fichier projet. Ces métadonnées sont utilisées par SSMA lorsqu’elle convertit des objets en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe, et lorsqu’elle migre des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez parcourir ces métadonnées dans le volet Explorateur de métadonnées DB2 et consulter les propriétés des objets de base de données individuels.  

> [!IMPORTANT]
> Avant d’essayer de vous connecter, assurez-vous que le serveur de base de données est en cours d’exécution et peut accepter des connexions.

**Pour se connecter à DB2**

1. Dans le menu **fichier** , sélectionnez **se connecter à DB2**.

   Si vous vous êtes connecté précédemment à DB2, le nom de la commande sera **reconnecté à DB2**.

2. Dans la zone **fournisseur** , vous verrez le **fournisseur OLE DB** qui est actuellement le seul fournisseur d’accès client DB2.

3. Dans la zone **responsable** , vous pouvez sélectionner **DB2 pour zOs**, **DB2 pour LUW** ou **DB2 pour i**

4. Dans la zone **mode** , sélectionnez **mode standard** ou **mode chaîne de connexion**.

   Utilisez le mode standard pour spécifier le nom du serveur et le port. Utilisez le mode nom du service pour spécifier le nom du service DB2 manuellement. Utilisez le mode chaîne de connexion pour fournir une chaîne de connexion complète.

5. Si vous sélectionnez le **mode standard**, indiquez les valeurs suivantes :

   - Dans la zone **nom du serveur** , entrez ou sélectionnez le nom ou l’adresse IP du serveur de base de données.
   - Si le serveur de base de données n’est pas configuré pour accepter les connexions sur le port par défaut (1521), entrez le numéro de port utilisé pour les connexions DB2 dans la zone **port du serveur** .
   - Dans la zone **port du serveur** , entrez le numéro de port TCP/IP.
   - Dans la zone **catalogue initial** , entrez le nom de la base de données.
   - Dans la zone **nom d’utilisateur** , entrez un compte DB2 disposant des autorisations nécessaires.
   - Dans la zone **mot de passe** , entrez le mot de passe du nom d’utilisateur spécifié.

6. Si vous sélectionnez le **mode de chaîne de connexion**, indiquez une chaîne de connexion dans la zone chaîne de **connexion** .

   L’exemple suivant illustre une chaîne de connexion OLE DB :

   `Provider=OraOLEDB.DB2;Data Source=MyDB2DB;User Id=myUsername;Password=myPassword;`

   L’exemple suivant montre une chaîne de connexion du client DB2 qui utilise la sécurité intégrée :
  
   `Data Source=MyDB2DB;Integrated Security=yes;`

   Pour plus d’informations, consultez [se connecter à Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/connect-to-oracle-oracletosql.md).
  
## <a name="reconnecting-to-db2"></a>Reconnexion à DB2

Votre connexion au serveur de base de données reste active jusqu’à ce que vous fermiez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter si vous souhaitez une connexion active à la base de données. Vous pouvez travailler hors connexion jusqu’à ce que vous souhaitiez mettre à jour des métadonnées, charger des objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et migrer des données.

## <a name="refreshing-db2-metadata"></a>Actualisation des métadonnées DB2

Les métadonnées relatives à la base de données DB2 ne sont pas automatiquement actualisées. Dans l’Explorateur de métadonnées DB2, les métadonnées sont un instantané des métadonnées lorsque vous vous êtes connecté pour la première fois ou lors de la dernière actualisation manuelle des métadonnées. Vous pouvez mettre à jour manuellement les métadonnées de tous les schémas, un seul schéma ou des objets de base de données individuels.

**Pour actualiser les métadonnées**

1. Assurez-vous que vous êtes connecté à la base de données.
2. Dans l’Explorateur de métadonnées DB2, activez la case à cocher en regard de chaque schéma ou objet de base de données que vous souhaitez mettre à jour.
3. Cliquez avec le bouton droit sur **schémas**, ou sur l’objet de schéma ou de base de données, puis sélectionnez **Actualiser à partir de la base de données**.

   Si vous ne disposez pas d’une connexion active, SSMA affiche la boîte de dialogue **connexion à DB2** pour vous permettre de vous connecter.
  
4. Dans la boîte de dialogue actualiser à partir de la base de données, spécifiez les objets à actualiser.
   - Pour actualiser un objet, cliquez sur le champ **actif** adjacent à l’objet jusqu’à ce qu’une flèche s’affiche.
   - Pour empêcher l’actualisation d’un objet, cliquez sur le champ **actif** adjacent à l’objet jusqu’à ce qu’un **X** apparaisse.
   - Pour actualiser ou refuser une catégorie d’objets, cliquez sur le champ **actif** en regard du dossier de catégorie.

     Pour afficher les définitions du codage en couleurs, cliquez sur le bouton **légende** .

5. [!INCLUDE[click OK](../../includes/clickok-md.md)]

## <a name="next-step"></a>étape suivante

- L’étape suivante du processus de migration consiste à [se connecter à SQL Server](./connecting-to-sql-server-db2tosql.md).

## <a name="see-also"></a>Voir aussi

- [Migration de bases de données DB2 vers SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)