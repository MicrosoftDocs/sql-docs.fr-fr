---
title: Interroger des colonnes en utilisant Always Encrypted avec SQL Server Management Studio | Microsoft Docs
description: Découvrez comment interroger des colonnes dans Always Encrypted à l’aide de SQL Server Management Studio. Récupérez des valeurs de chiffrement stockées dans des colonnes chiffrées.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ff7924bf1262ee29977d16548b6548c112bc8b5d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345449"
---
# <a name="query-columns-using-always-encrypted-with-sql-server-management-studio"></a>Interroger des colonnes en utilisant Always Encrypted avec SQL Server Management Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Cet article explique comment interroger des colonnes chiffrées avec [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) en utilisant [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md). Avec SSMS, vous pouvez :
- Récupérer des valeurs de chiffrement stockées dans des colonnes chiffrées. 
- Récupérer des valeurs de texte brut stockées dans des colonnes chiffrées.   
- Envoyez des valeurs en texte en clair ciblant des colonnes chiffrées (par exemple, dans des instructions `INSERT` ou `UPDATE` et en tant que paramètre de recherche de clauses `WHERE` dans des instructions `SELECT`).

## <a name="retrieving-ciphertext-values-stored-in-encrypted-columns"></a>Récupération de valeurs de chiffrement stockées dans des colonnes chiffrées    
L’exécution de requêtes SELECT qui récupèrent le texte chiffré des données stockées dans des colonnes chiffrées (sans déchiffrer les données) ne nécessite pas que vous ayez accès aux clés principales de colonne protégeant les données. Pour récupérer des valeurs d’une colonne chiffrée en tant que texte chiffré dans SSMS :

1. Vérifiez que vous pouvez accéder aux métadonnées relatives aux clés protégeant les colonnes sur lesquelles vous exécutez votre requête. Même si vous n’avez pas besoin d’accéder aux clés principales de colonne réelles, vous avez besoin d’autorisations au niveau de la base de données pour afficher les objets de métadonnées de clé principale de colonne et de clé de chiffrement de colonne dans la base de données. Pour plus d’informations, consultez [Autorisations pour interroger des colonnes chiffrées](#permissions-for-querying-encrypted-columns) ci-dessous.
1. Vérifiez que vous avez désactivé Always Encrypted pour la connexion de base de données de la fenêtre de l’éditeur de requête à partir de laquelle vous exécutez une requête `SELECT` récupérant des valeurs de texte chiffré. Consultez la section [Activation et désactivation d’Always Encrypted pour une connexion de base de données](#en-dis) ci-dessous.     
1. Exécutez votre requête `SELECT`. Les données récupérées à partir des colonnes chiffrées seront retournées comme valeurs binaires (chiffrées).   

### <a name="example"></a>Exemple
En supposant que `SSN` est une colonne chiffrée dans la table `Patients` , la requête ci-dessous récupère les valeurs de chiffrement binaires, si Always Encrypted est désactivé pour la connexion de base de données.   

![Capture d’écran de la requête SELECT [SSN] FROM [dbo].[Patients] et des résultats de la requête affichés sous forme de valeurs de texte chiffré binaire.](../../../relational-databases/security/encryption/media/always-encrypted-ciphertext.png)
 
## <a name="retrieving-plaintext-values-stored-in-encrypted-columns"></a>Récupération de valeurs de texte brut stockées dans des colonnes chiffrées    
Pour récupérer des valeurs à partir d’une colonne chiffrée en tant que texte brut (pour déchiffrer les valeurs) :   
1. Vérifiez que vous pouvez accéder aux clés principales de colonne et aux métadonnées relatives aux clés protégeant les colonnes sur lesquelles vous exécutez votre requête. Pour plus d’informations, consultez [Autorisations pour interroger des colonnes chiffrées](#permissions-for-querying-encrypted-columns) ci-dessous.
1.  Vérifiez que vous avez activé Always Encrypted pour la connexion de base de données de la fenêtre de l’éditeur de requête à partir de laquelle vous exécutez une requête `SELECT` récupérant et déchiffrant vos données. Ceci indique au fournisseur de données .NET Framework pour SQL Server (utilisé par SSMS) de déchiffrer les colonnes chiffrées dans le jeu de résultats de la requête. Consultez la section [Activation et désactivation d’Always Encrypted pour une connexion de base de données](#en-dis) ci-dessous.
1.  Exécutez votre requête `SELECT`. Les données récupérées à partir des colonnes chiffrées sont retournées sous forme de valeurs en texte clair des types de données d’origine.
 
### <a name="example"></a>Exemple
En supposant que le SSN est une colonne `char(11)` chiffrée dans la table `Patients` , la requête, illustrée ci-dessous, renvoie les valeurs en texte brut, si Always Encrypted est activé pour la connexion de base de données et si vous avez accès à la clé principale de colonne configurée pour la colonne `SSN` .   

![Capture d’écran de la requête SELECT [SSN] FROM [Clinic].[dbo].[Patients] et des résultats de la requête affichés sous forme de valeurs de texte brut.](../../../relational-databases/security/encryption/media/always-encrypted-plaintext.png)
 
## <a name="sending-plaintext-values-targeting-encrypted-columns"></a>Envoi de valeurs en texte brut ciblant des colonnes chiffrées       
Pour exécuter une requête qui envoie une valeur ciblant une colonne chiffrée, par exemple une requête qui insère, met à jour ou filtre une valeur stockée dans une colonne chiffrée :

1. Vérifiez que vous pouvez accéder aux clés principales de colonne et aux métadonnées pour les clés protégeant les colonnes sur lesquelles votre requête s’exécute. Pour plus d’informations, consultez [Autorisations pour interroger des colonnes chiffrées](#permissions-for-querying-encrypted-columns) ci-dessous.
1.  Vérifiez que vous avez activé Always Encrypted pour la connexion de base de données de la fenêtre de l’éditeur de requête à partir de laquelle vous exécutez une requête `SELECT` récupérant et déchiffrant vos données. Ceci indique au fournisseur de données .NET Framework pour SQL Server (utilisé par SSMS) de déchiffrer les colonnes chiffrées dans le jeu de résultats de la requête. Consultez la section [Activation et désactivation d’Always Encrypted pour une connexion de base de données](#en-dis) ci-dessous.
1. Vérifiez que le paramétrage pour Always Encrypted est activé pour la fenêtre de l’éditeur de requête. (Nécessite au moins SSMS version 17.0.) Déclarez une variable Transact-SQL et initialisez-la avec une valeur à envoyer (insérer, mettre à jour ou filtrer par) à la base de données. Voir la section [Paramétrage pour Always Encrypted](#param) ci-dessous pour plus d’informations.

1. Exécutez votre requête d’envoi de la valeur de la variable Transact-SQL à la base de données. SSMS convertira la variable à un paramètre de requête et chiffrera sa valeur avant de l’envoyer à la base de données.   

### <a name="example"></a>Exemple
En supposant que `SSN` est une colonne `char(11)` chiffrée dans la table `Patients` , le script ci-dessous tente de trouver une ligne contenant `'795-73-9838'` dans la colonne SSN pour retourner la valeur de la colonne `LastName` , à condition qu’Always Encrypted soit activé pour la connexion de base de données, que le paramétrage pour Always Encrypted soit activé pour la fenêtre de l’éditeur de requête et que vous ayez accès à la clé principale de colonne configurée pour la colonne `SSN` .   

![Capture d’écran de la requête DECLARE @SSN CHAR(11) = '795-73-9838' SELECT [LastName] FROM [dbo].[Patients] WHERE [SSN] = @SSN et des résultats de la requête.](../../../relational-databases/security/encryption/media/always-encrypted-patients.png)

## <a name="permissions-for-querying-encrypted-columns"></a>Autorisations pour interroger des colonnes chiffrées

Pour exécuter des requêtes sur des colonnes chiffrées, y compris des requêtes qui extraient des données en texte chiffré, vous avez besoin des autorisations `VIEW ANY COLUMN MASTER KEY DEFINITION` et `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` dans la base de données.

Outre les autorisations ci-dessus, pour déchiffrer des résultats de requête ou pour chiffrer les paramètres de requête (générés par le paramétrage de variables Transact-SQL), vous devez également accéder à la clé principale de colonne protégeant les colonnes cibles :

- **Magasin de certificats - Ordinateur local** Vous devez avoir un accès `Read` au certificat utilisé comme clé principale de colonne, ou être l’administrateur de l’ordinateur.   
- **Azure Key Vault** : Vous avez besoin des autorisations `get`, `unwrapKey` et `verify` sur le coffre contenant la clé principale de colonne.
- **Fournisseur du magasin de clés (KSP)**  : les autorisations et les informations d’identification nécessaires qu’il vous est demandé d’entrer quand vous utilisez un magasin de clés ou une clé, dépendent du magasin et de la configuration du fournisseur KSP.   
- **Fournisseur de services de chiffrement (CSP)**  : les autorisations et les informations d’identification nécessaires qu’il vous est demandé d’entrer quand vous utilisez un magasin de clés ou une clé, dépendent du magasin et de la configuration du fournisseur CSP.

Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="enabling-and-disabling-always-encrypted-for-a-database-connection"></a><a name="en-dis"></a> Activation et désactivation d’Always Encrypted pour une connexion de base de données   
Quand vous vous connectez à une base de données dans SSMS, vous pouvez activer ou désactiver les Always Encrypted pour la connexion de base de données. Par défaut, Always Encrypted est désactivé. 

L’activation d’Always Encrypted pour une connexion de base de données indique au fournisseur de données .NET Framework pour SQL Server, utilisé par SQL Server Management Studio, de tenter d’effectuer ces actions de manière transparente :   
-   Déchiffrer les valeurs qui sont extraites des colonnes chiffrées et retournées dans les résultats de la requête.   
-   Chiffrer les valeurs des variables paramétrées Transact-SQL qui ciblent des colonnes de base de données chiffrées.   

Si vous n’activez pas Always Encrypted pour une connexion, le fournisseur .NET Framework pour SQL Server utilisé par SSMS n’essaie pas de chiffrer les paramètres de requête ou de déchiffrer les résultats.

Vous pouvez activer ou désactiver Always Encrypted quand vous créez une connexion ou quand vous modifiez une connexion existante avec la boîte de dialogue **Se connecter au serveur**. 

Pour activer (désactiver) Always Encrypted :
1. Ouvrez la boîte de dialogue **Se connecter au serveur** (pour plus d’informations, consultez, [Se connecter à une instance SQL Server](../../../ssms/quickstarts/ssms-connect-query-sql-server.md#connect-to-a-sql-server-instance)).
1. Cliquez sur **Options >>** .
1. Si vous utilisez SSMS 18 ou ultérieur :
    1. Sélectionnez l’onglet **Always Encrypted**.
    1. Pour activer Always Encrypted, sélectionnez **Activer Always Encrypted (chiffrement de colonne)** . Pour désactiver Always Encrypted, vérifiez que **Activer Always Encrypted (chiffrement de colonne)** n’est pas sélectionné.
1. Si vous utilisez SSMS 17 ou antérieur :
    1. Sélectionnez l’onglet **Propriétés supplémentaires**.
    1. Pour activer Always Encrypted, tapez `Column Encryption Setting = Enabled`. Pour désactiver Always Encrypted, spécifiez `Column Encryption Setting = Disabled` ou supprimez la valeur de **Paramètre de chiffrement de colonne** dans l’onglet **Propriétés supplémentaires** (sa valeur est **Désactivé**).   
 1. Cliquez sur **Connecter**.

Pour exécuter des instructions qui tirent parti d’une enclave sécurisée côté serveur quand vous utilisez [Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md), vous devez spécifier une URL d’attestation d’enclave, en plus d’activer Always Encrypted pour la connexion. Pour plus d’informations, consultez les [prérequis pour l’exécution d’instructions T-SQL utilisant des enclaves dans SSMS](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-t-sql-statements-using-enclaves-in-ssms).

> [!TIP]
> Pour activer/désactiver Always Encrypted pour une fenêtre de l’éditeur de requête existante :   
> 1.    Cliquez avec le bouton droit n’importe où dans la fenêtre de l’éditeur de requête.
> 2.    Sélectionnez **Connexion** > **Changer la connexion...** . Ceci va ouvrir la boîte de dialogue **Se connecter au serveur** pour la connexion actuelle de la fenêtre de l’éditeur de requête. 
> 2.    Activez ou désactivez Always Encrypted en suivant les étapes ci-dessus, puis cliquez sur **Se connecter**.  
   
## <a name="parameterization-for-always-encrypted"></a><a name="param"></a>Paramétrage pour Always Encrypted   
 
Le paramétrage d’Always Encrypted est une fonctionnalité de SQL Server Management Studio qui convertit automatiquement les variables Transact-SQL en paramètres de requête (instances de [SqlParameter Class](/dotnet/api/system.data.sqlclient.sqlparameter)). (Nécessite au moins SSMS version 17.0.) Cela permet au fournisseur de données .NET Framework pour SQL Server sous-jacent de détecter les données ciblant les colonnes chiffrées et de chiffrer ces données avant de les envoyer à la base de données. 
  
Sans le paramétrage, le fournisseur de données .NET Framework transmet chaque instruction créée dans l’éditeur de requête en tant que requête non paramétrée. Si la requête contient des littéraux ou des variables Transact-SQL qui ciblent des colonnes chiffrées, le fournisseur de données .NET Framework pour SQL Server ne peut pas les détecter et les chiffrer avant d’envoyer la requête à la base de données. Par conséquent, la requête échoue en raison d’une incompatibilité de type (entre la variable littérale en texte brut Transact-SQL et la colonne chiffrée). Par exemple, sans paramétrage, la requête suivante échoue, supposant que la colonne `SSN` est chiffrée.   

```sql
DECLARE @SSN NCHAR(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN
```

### <a name="enabling-and-disabling-parameterization-for-always-encrypted"></a>Activation/désactivation du paramétrage d’Always Encrypted

Le paramétrage d’Always Encrypted est désactivé par défaut.

Pour activer/désactiver le paramétrage d’Always Encrypted pour la fenêtre active de l’éditeur de requête :

1. Sélectionnez **Requête** dans le menu principal.
2. Sélectionnez **Options de requête…** .
3. Accédez à **Exécution** > **Avancé**.
4. Sélectionnez ou désélectionnez **Activer le paramétrage d’Always Encrypted**.
5. Cliquez sur **OK**.

Pour activer/désactiver le paramétrage d’Always Encrypted pour de prochaines fenêtres d’éditeur de requête :

1. Sélectionnez **Outils** dans le menu principal.
2. Sélectionnez **Options...** .
3. Accédez à **Exécution de la requête** > **SQL Server** > **Avancé**.
4. Sélectionnez ou désélectionnez **Activer le paramétrage d’Always Encrypted**.
5. Cliquez sur **OK**.

Si vous exécutez une requête dans une fenêtre de l’éditeur de requête qui utilise une connexion de base de données avec Always Enabled activé, mais que le paramétrage n’est pas activé pour la fenêtre de l’éditeur de requête, vous serez invité à l’activer.

> [!NOTE]
> Le paramétrage d’Always Encrypted fonctionne seulement dans les fenêtres de l’éditeur de requête qui utilisent des connexions de base de données avec Always Encrypted activé (consultez [Activation et désactivation du paramétrage d’Always Encrypted](#enabling-and-disabling-parameterization-for-always-encrypted)). Aucune variable Transact-SQL n’est paramétrée si la fenêtre d’éditeur de requête utilise une connexion de base de données sans Always Encrypted activé.

### <a name="how-parameterization-for-always-encrypted-works"></a>Fonctionnement du paramétrage d’Always Encrypted

Si le paramétrage d’Always Encrypted et le comportement Always Encrypted dans la connexion de base de données sont tous deux activés pour une fenêtre de l’éditeur de requête, SQL Server Management Studio tentera de paramétrer des variables Transact-SQL répondant aux conditions préalables suivantes :

- Variables déclarées et initialisées dans la même instruction (initialisation inline). Les variables déclarées avec des instructions `SET` séparées ne seront pas paramétrées.
- Variables initialisées avec un littéral unique. Les variables initialisées avec des expressions contenant des opérateurs ou des fonctions ne seront pas paramétrées.

Voici des exemples de variables paramétrées par SQL Server Management Studio.

```sql
DECLARE @SSN char(11) = '795-73-9838';
   
DECLARE @BirthDate date = '19990104';
DECLARE @Salary money = $30000;
```

Et voici quelques exemples de variables que SQL Server Management Studio ne tentera pas de paramétrer :

```sql
DECLARE @Name nvarchar(50); --Initialization separate from declaration
SET @Name = 'Abel';

DECLARE @StartDate date = GETDATE(); -- a function used instead of a literal

DECLARE @NewSalary money = @Salary * 1.1; -- an expression used instead of a literal
```
 
Pour qu’une tentative de paramétrage aboutisse :   
- Le type de littéral utilisé pour l’initialisation de la variable à paramétrer doit correspondre au type de la déclaration de la variable.   
- Si le type déclaré de la variable est un type de date ou d’heure, la variable doit être initialisée à l’aide d’une chaîne dans l’un des [formats suivants conformes à la norme ISO 8601](../../../t-sql/functions/cast-and-convert-transact-sql.md#date-and-time-styles).    

Voici des exemples de déclarations de variables Transact-SQL générant des erreurs de paramétrage :   
```sql
DECLARE @BirthDate date = '01/04/1999' -- unsupported date format   
   
DECLARE @Number int = 1.1 -- the type of the literal does not match the type of the variable   
```
SQL Server Management Studio utilise la technologie Intellisense pour vous indiquer quelles variables peuvent être paramétrées avec succès et quelles tentatives de paramétrage échouent (et pourquoi).   

Une déclaration de variable pouvant être paramétrée avec succès est marquée avec un trait de soulignement d’avertissement dans l’éditeur de requête. Si vous placez le curseur sur une instruction de déclaration marquée avec un trait de soulignement d’avertissement, vous verrez les résultats du processus de paramétrage, y compris les valeurs des propriétés clés de l’objet [SqlParameter](/dotnet/api/system.data.sqlclient.sqlparameter) résultant (sur lequel la variable est mappée) : [SqlDbType](/dotnet/api/system.data.sqlclient.sqlparameter.sqldbtype), [Size](/dotnet/api/system.data.sqlclient.sqlparameter.size), [Precision](/dotnet/api/system.data.sqlclient.sqlparameter.precision), [Scale](/dotnet/api/system.data.sqlclient.sqlparameter.scale), [SqlValue](/dotnet/api/system.data.sqlclient.sqlparameter.sqlvalue). Vous pouvez également voir la liste complète de toutes les variables correctement paramétrées dans l’onglet **Avertissement** de l’affichage **Liste d’erreurs** . Pour ouvrir l’affichage **Liste d’erreurs** , sélectionnez **Affichage** à partir du menu principal, puis sélectionnez **Liste d’erreurs**.    

Si SQL Server Management Studio a tenté de paramétrer une variable, mais que le paramétrage a échoué, la déclaration de la variable sera marquée avec un soulignement d’erreur. Si vous placez le curseur sur l’instruction de déclaration marquée avec un soulignement d’erreur, vous obtiendrez des informations sur l’erreur. Vous pouvez également voir la liste complète des erreurs de paramétrage pour toutes les variables dans l’onglet **Erreur** de l’affichage **Liste d’erreurs** . Pour ouvrir l’affichage **Liste d’erreurs** , sélectionnez **Affichage** à partir du menu principal, puis sélectionnez **Liste d’erreurs**.   

La capture d’écran ci-dessous montre un exemple de six déclarations de variable. SQL Server Management Studio a paramétré avec succès les trois premières variables. Les trois dernières variables n’ont pas respecté les conditions préalables pour le paramétrage, et par conséquent, SQL Server Management Studio n’a pas tenté de les paramétrer (les déclarations ne sont pas marquées d’une quelconque façon).

![Capture d’écran montrant un exemple de six déclarations de variables avec trois correctement paramétrées et trois échecs ainsi que les messages d’avertissement associés.](../../../relational-databases/security/encryption/media/always-encrypted-parameter-warnings.png)
 
Un autre exemple ci-dessous montre deux variables qui répondent aux conditions préalables pour le paramétrage, mais la tentative de paramétrage a échoué, car les variables sont initialisées de manière incorrecte.    
 
![Capture d’écran montrant un exemple de deux déclarations de variables qui finissent par échouer avec les messages d’erreur associés.](../../../relational-databases/security/encryption/media/always-encrypted-error.png)
 
> [!NOTE]
> Étant donné que Always Encrypted prend en charge un sous-ensemble limité de conversions de type, dans de nombreux cas, il est nécessaire que le type de données d’une variable Transact-SQL soit identique au type de colonne de base de données ciblée. Par exemple, en supposant que le type de la colonne `SSN` dans la `Patients` table est `char(11)`, la requête ci-après échoue, dans la mesure où le type de la variable `@SSN` ( `nchar(11)`), ne correspond pas au type de la colonne.   

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
> Sans paramétrage, la requête entière, y compris les conversions de type, est traitée à l’intérieur de SQL Server/Azure SQL Database. Avec un paramétrage activé, certaines conversions de type sont effectuées par .NET Framework au sein de SQL Server Management Studio. En raison des différences entre le système .NET Framework et le système SQL Server (par exemple, précision différente de certains types, comme float), une requête exécutée avec paramétrage activé peut produire des résultats différents de ceux d’une requête exécutée sans paramétrage activé. 

## <a name="next-steps"></a>Étapes suivantes
- [Développer des applications avec Always Encrypted](always-encrypted-client-development.md)


## <a name="see-also"></a>Voir aussi
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)