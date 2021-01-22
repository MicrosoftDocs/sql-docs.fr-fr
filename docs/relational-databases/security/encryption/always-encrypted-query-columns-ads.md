---
description: Interroger des colonnes en utilisant Always Encrypted avec Azure Data Studio
title: Interroger des colonnes en utilisant Always Encrypted avec Azure Data Studio | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 93bb5c30eeb9cdc6f10c3b71d0e4f70f3f8c6477
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534418"
---
# <a name="query-columns-using-always-encrypted-with-azure-data-studio"></a>Interroger des colonnes en utilisant Always Encrypted avec Azure Data Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Cet article explique comment interroger des colonnes chiffrées avec [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) en utilisant [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md). Avec Azure Data Studio, vous pouvez :
- Récupérer des valeurs de chiffrement stockées dans des colonnes chiffrées. 
- Récupérer des valeurs de texte brut stockées dans des colonnes chiffrées.  
- Envoyez des valeurs en texte en clair ciblant des colonnes chiffrées (par exemple, dans des instructions `INSERT` ou `UPDATE` et en tant que paramètre de recherche de clauses `WHERE` dans des instructions `SELECT`). 

## <a name="retrieving-ciphertext-values-stored-in-encrypted-columns"></a>Récupération de valeurs de chiffrement stockées dans des colonnes chiffrées    
Cette section décrit comment récupérer des données stockées dans des colonnes chiffrées sous forme de texte chiffré.

### <a name="steps"></a>Étapes
1. Vérifiez que vous avez désactivé Always Encrypted pour la connexion de base de données de la fenêtre de requête à partir de laquelle vous exécutez une requête `SELECT` récupérant des valeurs de texte chiffré. Consultez la section [Activation et désactivation d’Always Encrypted pour une connexion de base de données](#enabling-and-disabling-always-encrypted-for-a-database-connection) ci-dessous.     
1. Exécutez votre requête `SELECT`. Les données récupérées à partir des colonnes chiffrées seront retournées comme valeurs binaires (chiffrées).   

### <a name="example"></a>Exemple
En supposant que `SSN` est une colonne chiffrée dans la table `Patients` , la requête ci-dessous récupère les valeurs de chiffrement binaires, si Always Encrypted est désactivé pour la connexion de base de données.   

![Capture d’écran de la requête SELECT * FROM [dbo].[Patients] et des résultats de la requête affichés sous forme de valeurs de texte chiffré binaire.](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-ciphertext.png)
 
## <a name="retrieving-plaintext-values-stored-in-encrypted-columns"></a>Récupération de valeurs de texte brut stockées dans des colonnes chiffrées    
Cette section décrit comment récupérer des données stockées dans des colonnes chiffrées sous forme de texte chiffré.

### <a name="prerequisites"></a>Prérequis
- Azure Data Studio version 17.1 ou ultérieure.
- Vérifiez que vous pouvez accéder aux clés principales de colonne et aux métadonnées relatives aux clés protégeant les colonnes sur lesquelles vous exécutez votre requête. Pour plus d’informations, consultez [Autorisations pour interroger des colonnes chiffrées](#permissions-for-querying-encrypted-columns) ci-dessous.
- Les clés principales de colonne doivent être stockées dans Azure Key Vault ou dans le magasin de certificats Windows. Azure Data Studio ne prend pas en charge d’autres magasins de clés.

### <a name="steps"></a>Étapes
1.  Activez Always Encrypted pour la connexion de base de données de la fenêtre de requête à partir de laquelle vous exécutez une requête `SELECT` récupérant et déchiffrant vos données. Ceci indique au fournisseur de données [Microsoft .NET pour SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md) (utilisé par Azure Data Studio) de déchiffrer les colonnes chiffrées dans le jeu de résultats de la requête. Consultez la section [Activation et désactivation d’Always Encrypted pour une connexion de base de données](#enabling-and-disabling-always-encrypted-for-a-database-connection) ci-dessous.
1.  Exécutez votre requête `SELECT`. Les données récupérées à partir des colonnes chiffrées sont retournées sous forme de valeurs en texte clair des types de données d’origine.
 
### <a name="example"></a>Exemple
En supposant que le SSN est une colonne `Patients` chiffrée dans la table, la requête, illustrée ci-dessous, renvoie les valeurs en texte brut, si Always Encrypted est activé pour la connexion de base de données et si vous avez accès à la clé principale de colonne configurée pour la colonne `SSN`.   

![Capture d’écran de la requête SELECT * FROM [dbo].[Patients] et des résultats de la requête affichés sous forme de valeurs de texte brut.](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-plaintext.png)
 
## <a name="sending-plaintext-values-targeting-encrypted-columns"></a>Envoi de valeurs en texte brut ciblant des colonnes chiffrées       
Cette section décrit comment exécuter une requête qui envoie des valeurs ciblant une colonne chiffrée. Par exemple, une requête qui insère, met à jour ou filtre par une valeur stockée dans une colonne chiffrée :

### <a name="prerequisites"></a>Prérequis
- Azure Data Studio version 18.1 ou ultérieure.
- Vérifiez que vous pouvez accéder aux clés principales de colonne et aux métadonnées relatives aux clés protégeant les colonnes sur lesquelles vous exécutez votre requête. Pour plus d’informations, consultez [Autorisations pour interroger des colonnes chiffrées](#permissions-for-querying-encrypted-columns) ci-dessous.
- Les clés principales de colonne doivent être stockées dans Azure Key Vault ou dans le magasin de certificats Windows. Azure Data Studio ne prend pas en charge d’autres magasins de clés.

### <a name="steps"></a>Étapes
1. Activez Always Encrypted pour la connexion de base de données de la fenêtre de requête à partir de laquelle vous exécutez une requête `SELECT` récupérant et déchiffrant vos données. Cela indique au Fournisseur de données [Microsoft .NET pour SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md) (utilisé par Azure Data Studio) de chiffrer les paramètres de requête ciblant des colonnes chiffrées et déchiffrer les résultats récupérés à partir des colonnes chiffrées. Consultez la section [Activation et désactivation d’Always Encrypted pour une connexion de base de données](#enabling-and-disabling-always-encrypted-for-a-database-connection) ci-dessous. 
1. Activez le paramétrage d’Always Encrypted depuis la fenêtre de requête. Voir la section [Paramétrage pour Always Encrypted](#parameterization-for-always-encrypted) ci-dessous pour plus d’informations.
1. Déclarez une variable Transact-SQL et initialisez-la avec une valeur à envoyer (insérer, mettre à jour ou filtrer par) à la base de données. 
1. Exécutez votre requête d’envoi de la valeur de la variable Transact-SQL à la base de données. Azure Data Studio convertira la variable à un paramètre de requête et chiffrera sa valeur avant de l’envoyer à la base de données.   

### <a name="example"></a>Exemple
En supposant que `SSN` est une colonne `char(11)` chiffrée dans la table `Patients`, le script ci-dessous tentera de trouver une ligne contenant `'795-73-9838'` dans la colonne SSN. Les résultats sont renvoyés si Always Encrypted est activé pour la connexion à la base de données et que le paramétrage de Always Encrypted est activé pour la fenêtre de requête et si vous avez accès à la clé principale de colonne configurée pour la colonne `SSN`.   

![Capture d’écran de la requête DECLARE @SSN char(11) = '795-73-9838' SELECT * FROM [dbo].[Patients] WHERE [SSN] = @SSN et des résultats de la requête.](../../../relational-databases/security/encryption/media/always-encrypted-ads-query-parameters.png)

## <a name="permissions-for-querying-encrypted-columns"></a>Autorisations pour interroger des colonnes chiffrées

Pour exécuter des requêtes et déchiffrer des données stockées dans les colonnes chiffrées, vous avez besoin des autorisations **VIEW ANY COLUMN MASTER KEY DEFINITION** et **VIEW ANY COLUMN ENCRYPTION KEY DEFINITION** dans la base de données.

Outre les autorisations ci-dessus, pour déchiffrer des résultats de requête ou pour chiffrer les paramètres de requête (générés par le paramétrage de variables Transact-SQL), vous devez également accéder à la clé principale de colonne protégeant les colonnes cibles :

- **Magasin de certificats - Ordinateur local :** Vous devez avoir un accès en **Lecture** au certificat utilisé comme clé principale de colonne, ou être l’administrateur de l’ordinateur.   
- **Azure Key Vault :** Vous avez besoin des autorisations **get**, **unwrapKey** et **verify** sur le coffre contenant la clé principale de colonne.

Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="enabling-and-disabling-always-encrypted-for-a-database-connection"></a>Activation et désactivation d’Always Encrypted pour une connexion de base de données   

Quand vous vous connectez à une base de données dans Azure Data Studio, vous pouvez activer ou désactiver Always Encrypted pour la connexion de base de données. Par défaut, Always Encrypted est désactivé. 

L’activation d’Always Encrypted pour une connexion de base de données indique au fournisseur de données [Microsoft.NET pour SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md), utilisé par Azure Data Studio, de tenter d’effectuer ces actions de manière transparente :   
-   Déchiffrer les valeurs qui sont extraites des colonnes chiffrées et retournées dans les résultats de la requête.   
-   Chiffrer les valeurs des variables paramétrées Transact-SQL qui ciblent des colonnes de base de données chiffrées.   

Si vous n’activez pas Always Encrypted pour une connexion, le fournisseur de données Microsoft .NET pour SQL Server n’essaiera pas de chiffrer les paramètres de requête ou de déchiffrer les résultats.

Vous pouvez activer ou désactiver Always Encrypted lorsque vous vous connectez à une base de données. Pour obtenir des informations générales sur la façon de se connecter à une base de données, consultez :
- [Démarrage rapide : Se connecter et interroger SQL Server à l’aide d’Azure Data Studio](../../../azure-data-studio/quickstart-sql-server.md)
- [Démarrage rapide : Utiliser Azure Data Studio pour se connecter et interroger une base de données Azure SQL](../../../azure-data-studio/quickstart-sql-database.md)

Pour activer (désactiver) Always Encrypted :
1. Dans la boîte de dialogue **Connexion**, cliquez sur **Avancé...** .
2. Pour activer Always Encrypted pour la connexion, définissez le champ **Always Encrypted** sur **Activé**. Pour désactiver Always Encrypted, laissez la valeur du champ **Always Encrypted** vide ou définissez-la sur **Désactivé**.
3. Cliquez sur **OK** pour fermer les **Propriétés avancées**.

![Brève vidéo présentant les étapes permettant d’activer Always Encrypted pour la connexion.](../../../relational-databases/security/encryption/media/always-encrypted-ads-connect.gif)

Pour exécuter des instructions qui tirent parti d’une enclave sécurisée côté serveur quand vous utilisez [Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md), vous devez, en plus d’activer Always Encrypted pour la connexion, spécifier un protocole et une URL d’attestation d’enclave. Pour plus d’informations, consultez les [prérequis pour l’exécution d’instructions T-SQL utilisant des enclaves dans Azure Data Studio](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-t-sql-statements-using-enclaves-in-azure-data-studio).

> [!TIP]
> Pour activer ou désactiver l’option Always Encrypted pour une fenêtre de requête existante, cliquez sur **Déconnecter** puis sur **Connecter** et suivez les étapes ci-dessus pour vous reconnecter à votre base de données avec les valeurs souhaitées du champ **Always Encrypted**. 

> [!NOTE] 
> Le bouton **Changer la connexion** dans une fenêtre de requête ne prend pas actuellement en charge le basculement entre Always Encrypted activé et désactivé.

## <a name="parameterization-for-always-encrypted"></a>Paramétrage pour Always Encrypted

Le paramétrage d’Always Encrypted est une fonctionnalité d’Azure Data Studio 18.1 qui convertit automatiquement les variables Transact-SQL en paramètres de requête (instances de [SqlParameter Class](/dotnet/api/microsoft.data.sqlclient.sqlparameter)). Cela permet au fournisseur de données [Microsoft.NET Framework pour SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md) sous-jacent de détecter les données ciblant les colonnes chiffrées et de chiffrer ces données avant de les envoyer à la base de données.
  
Sans le paramétrage, le fournisseur de données .Microsoft.NET Framework transmet chaque instruction créée dans la fenêtre de requête en tant que requête non paramétrée. Si la requête contient des littéraux ou des variables Transact-SQL qui ciblent des colonnes chiffrées, le fournisseur de données .NET Framework pour SQL Server ne peut pas les détecter et les chiffrer avant d’envoyer la requête à la base de données. Par conséquent, la requête échoue en raison d’une incompatibilité de type (entre la variable littérale en texte brut Transact-SQL et la colonne chiffrée). Par exemple, sans paramétrage, la requête suivante échoue, supposant que la colonne `SSN` est chiffrée.   

```sql
DECLARE @SSN CHAR(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN
```

### <a name="enabling-and-disabling-parameterization-for-always-encrypted"></a>Activation/désactivation du paramétrage d’Always Encrypted

Le paramétrage d’Always Encrypted est désactivé par défaut.

Pour activer/désactiver le paramétrage d’Always Encrypted :

1. Sélectionnez **Fichier** > **Préférences** > **Paramètres** (**Code** > **Préférences** > **Paramètres** sur Mac).
2. Accédez à **Données** > **Microsoft SQL Server**.
3. Sélectionnez ou désélectionnez **Activer le paramétrage d’Always Encrypted**.
4. Fermez la fenêtre **Paramètres**.

![Brève vidéo présentant comment activer/désactiver le paramétrage d’Always Encrypted.](../../../relational-databases/security/encryption/media/always-encrypted-ads-parameterization.gif)

> [!NOTE]
> Le paramétrage d’Always Encrypted fonctionne uniquement dans les requêtes qui utilisent des connexions de base de données avec Always Encrypted activé (voir [Activation et désactivation d’Always Encrypted pour une base de données](#enabling-and-disabling-always-encrypted-for-a-database-connection)). Aucune variable Transact-SQL n’est paramétrée si la fenêtre de requête utilise une connexion de base de données sans Always Encrypted activé.

### <a name="how-parameterization-for-always-encrypted-works"></a>Fonctionnement du paramétrage d’Always Encrypted

Si le paramétrage d'Always Encrypted et Always Encrypted sont tous deux activés pour une fenêtre de requête, Azure Data Studio tentera de paramétrer les variables Transact-SQL répondant aux conditions préalables suivantes :

- Variables déclarées et initialisées dans la même instruction (initialisation inline). Les variables déclarées avec des instructions `SET` séparées ne seront pas paramétrées.
- Variables initialisées avec un littéral unique. Les variables initialisées avec des expressions contenant des opérateurs ou des fonctions ne seront pas paramétrées.

Voici des exemples de variables paramétrées par Azure Data Studio.

```sql
DECLARE @SSN char(11) = '795-73-9838';
   
DECLARE @BirthDate date = '19990104';
DECLARE @Salary money = $30000;
```

Voici quelques exemples de variables qu’Azure Data Studio ne tentera pas de paramétrer :

```sql
DECLARE @Name nvarchar(50); --Initialization separate from declaration
SET @Name = 'Abel';

DECLARE @StartDate date = GETDATE(); -- a function used instead of a literal

DECLARE @NewSalary money = @Salary * 1.1; -- an expression used instead of a literal
```
 
Pour qu’une tentative de paramétrage aboutisse :   
- Le type de littéral utilisé pour l’initialisation de la variable à paramétrer doit correspondre au type de la déclaration de la variable.   
- Si le type déclaré de la variable est un type de date ou d’heure, la variable doit être initialisée à l’aide d’une chaîne à l’aide d’un des formats suivants conformes à la norme ISO 8601.   

Voici des exemples de déclarations de variables Transact-SQL générant des erreurs de paramétrage :

```sql
DECLARE @BirthDate date = '01/04/1999' -- unsupported date format   
   
DECLARE @Number int = 1.1 -- the type of the literal does not match the type of the variable   
```

Azure Data Studio utilise la technologie Intellisense pour vous indiquer quelles variables peuvent être paramétrées avec succès et quelles tentatives de paramétrage échouent (et pourquoi).   

Une déclaration de variable pouvant être paramétrée avec succès est marquée avec un trait de soulignement d’avertissement dans la fenêtre de requête. Si vous placez le curseur sur une instruction de déclaration marquée avec un trait de soulignement de message, vous verrez les résultats du processus de paramétrage, y compris les valeurs des propriétés clés de l’objet [SqlParameter](/dotnet/api/microsoft.data.sqlclient.sqlparameter) résultant (sur lequel la variable est mappée) : [SqlDbType](/dotnet/api/microsoft.data.sqlclient.sqlparameter.dbtype), [Size](/dotnet/api/microsoft.data.sqlclient.sqlparameter.size), [Precision](/dotnet/api/microsoft.data.sqlclient.sqlparameter.precision), [Scale](/dotnet/api/microsoft.data.sqlclient.sqlparameter.scale), [SqlValue](/dotnet/api/microsoft.data.sqlclient.sqlparameter.sqlvalue). Vous pouvez également voir la liste complète de toutes les variables correctement paramétrées dans l’affichage **Problème**. Pour ouvrir l’affichage **Problèmes**, sélectionnez **Afficher** > **Problèmes**.    



Si Azure Data Studio a tenté de paramétrer une variable, mais que le paramétrage a échoué, la déclaration de la variable sera marquée avec un soulignement d’erreur. Si vous placez le curseur sur l’instruction de déclaration marquée avec un soulignement d’erreur, vous obtiendrez des informations sur l’erreur. Vous pouvez également voir la liste complète des erreurs de paramétrage pour toutes les variables dans l’affichage **Problèmes**.

 
> [!NOTE]
> Étant donné que Always Encrypted prend en charge un sous-ensemble limité de conversions de type, dans de nombreux cas, il est nécessaire que le type de données d’une variable Transact-SQL soit identique au type de colonne de base de données ciblée. Par exemple, en supposant que le type de la colonne `SSN` dans la `Patients` table est `char(11)`, la requête ci-après échoue, dans la mesure où le type de la variable `@SSN`, qui est `nchar(11)`, ne correspond pas au type de la colonne.   

```sql
DECLARE @SSN nchar(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN;
```

```output
Msg 402, Level 16, State 2, Line 5   
The data types char(11) encrypted with (encryption_type = 'DETERMINISTIC', 
encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', 
column_encryption_key_database_name = 'Clinic') collation_name = 'Latin1_General_BIN2' 
and nchar(11) encrypted with (encryption_type = 'DETERMINISTIC', 
encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', 
column_encryption_key_database_name = 'Clinic') are incompatible in the equal to operator.
```

> [!NOTE]
> Sans paramétrage, la requête entière, y compris les conversions de type, est traitée à l’intérieur de SQL Server/Azure SQL Database. Une fois le paramétrage activé, certaines conversions de type sont effectuées par le fournisseur de données Microsoft .NET pour SQL Server à l’intérieur d’Azure Data Studio. En raison des différences entre le système Microsoft.NET et le système SQL Server (par exemple, précision différente de certains types, comme float), une requête exécutée avec le paramétrage activé peut produire des résultats différents de ceux d’une requête exécutée sans le paramétrage activé. 

## <a name="next-steps"></a>Étapes suivantes
- [Développer des applications avec Always Encrypted](always-encrypted-client-development.md)


## <a name="see-also"></a>Voir aussi
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)