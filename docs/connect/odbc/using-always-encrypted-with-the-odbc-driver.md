---
title: Utilisation d’Always Encrypted avec ODBC Driver
description: Découvrez comment développer des applications ODBC à l’aide d’Always Encrypted et de Microsoft ODBC Driver for SQL Server.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 02e306b8-9dde-4846-8d64-c528e2ffe479
ms.author: v-chojas
author: v-chojas
ms.openlocfilehash: f066c8b1429a11b67cd6fc78fd93eaad1a6fc110
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534708"
---
# <a name="using-always-encrypted-with-the-odbc-driver-for-sql-server"></a>Utilisation d’Always Encrypted avec ODBC Driver for SQL Server
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

### <a name="applicable-to"></a>Applicable à

- ODBC Driver 13.1 for SQL Server
- ODBC Driver 17 for SQL Server

### <a name="introduction"></a>Introduction

Cet article fournit des informations sur la façon de développer des applications ODBC à l’aide d’[Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) ou [Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves.md) et du [Pilote ODBC pour SQL Server](microsoft-odbc-driver-for-sql-server.md).

Always Encrypted permet aux applications clientes de chiffrer des données sensibles et de ne jamais révéler les données ou les clés de chiffrement à SQL Server ou Azure SQL Database. À cette fin, un pilote compatible avec Always Encrypted, comme ODBC Driver for SQL Server, chiffre et déchiffre de manière transparente les données sensibles dans l’application cliente. Le pilote détermine automatiquement les paramètres de requêtes qui correspondent aux colonnes de base de données sensibles (protégées avec Always Encrypted) et chiffre les valeurs de ces paramètres avant de transmettre les données à SQL Server ou Azure SQL Database. De même, il déchiffre de manière transparente les données récupérées dans les colonnes de base de données chiffrées, qui figurent dans les résultats de la requête. Always Encrypted *avec les enclaves sécurisées* étend cette fonctionnalité pour activer des fonctionnalités plus complexes sur les données sensibles tout en préservant la confidentialité des données.

Pour plus d’informations, consultez [Always Encrypted (Moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) et [Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

### <a name="prerequisites"></a>Prérequis

Configurez Always Encrypted dans votre base de données. Pour cela, vous devez mettre en service des clés Always Encrypted et configurer le chiffrement pour les colonnes de base de données sélectionnées. Si vous n’avez pas déjà une base de données dans laquelle est configuré Always Encrypted, suivez les instructions de [Prise en main d’Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted). En particulier, votre base de données doit contenir les définitions de métadonnées pour une clé principale de colonne (CMK), une clé de chiffrement de colonne (CEK) et une table contenant une ou plusieurs colonnes chiffrées à l’aide de cette clé CEK.

Si vous utilisez Always Encrypted avec enclaves sécurisées, consultez [Développer des applications en utilisant Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves-client-development.md) pour connaître les prérequis supplémentaires.

### <a name="enabling-always-encrypted-in-an-odbc-application"></a>Activation d’Always Encrypted dans une application ODBC

Le moyen le plus simple d’activer à la fois le chiffrement des paramètres et le déchiffrement des colonnes chiffrées des jeux de résultats consiste à affecter la valeur **Enabled** au mot clé de chaîne de connexion `ColumnEncryption`. Voici un exemple de chaîne de connexion activant Always Encrypted :

```
SQLWCHAR *connString = L"Driver={ODBC Driver 17 for SQL Server};Server={myServer};Trusted_Connection=yes;ColumnEncryption=Enabled;";
```

Vous pouvez aussi activer Always Encrypted dans la configuration de source de données, à l’aide des mêmes clé et valeur (qui seront remplacées par le paramètre de chaîne de connexion, s’il est présent), ou par programmation avec l’attribut de préconnexion `SQL_COPT_SS_COLUMN_ENCRYPTION`. Procéder de cette façon substitue la valeur définie dans la chaîne de connexion ou la source de données :

```
 SQLSetConnectAttr(hdbc, SQL_COPT_SS_COLUMN_ENCRYPTION, (SQLPOINTER)SQL_COLUMN_ENCRYPTION_ENABLE, 0);
```

Une fois Always Encrypted activé pour la connexion, son comportement peut être ajusté pour des requêtes individuelles. Pour plus d’informations, consultez [Contrôle de l’impact d’Always Encrypted sur les performances](#controlling-the-performance-impact-of-always-encrypted) ci-dessous.

Notez que l’activation d’Always Encrypted ne suffit pas à la réussite du chiffrement ou du déchiffrement ; vous devez également garantir ce qui suit :

- L’application dispose des autorisations de base de données *VIEW ANY COLUMN MASTER KEY DEFINITION* et *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* qui sont nécessaires pour accéder aux métadonnées des clés Always Encrypted dans la base de données. Pour plus d’informations, consultez [Autorisations de base de données](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).

- L’application peut accéder à la clé CMK qui protège les clés CEK pour les colonnes chiffrées interrogées. Cela dépend du fournisseur de magasin de clés qui stocke la clé CMK. Pour plus d’informations, consultez [Utilisation de magasins de clés principales de colonne](#working-with-column-master-key-stores).

### <a name="enabling-always-encrypted-with-secure-enclaves"></a>Activation d’Always Encrypted avec les enclaves sécurisées

> [!NOTE]
> Sur Linux et macOS, la version 1.0.1 ou une version ultérieure d’OpenSSL est nécessaire pour pouvoir utiliser Always Encrypted avec enclaves sécurisées.

À partir de la version 17.4, le pilote prend en charge Always Encrypted avec enclaves sécurisées. Pour activer l’utilisation de l’enclave lors de la connexion à une base de données, définissez la clé DSN `ColumnEncryption`, le mot clé de la chaîne de connexion ou l’attribut de connexion sur la valeur suivante : `<attestation protocol>\<attestation URL>`, où :

- `<attestation protocol>` : spécifie un protocole utilisé pour l’attestation d’enclave.
  - Si vous utilisez [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)] et le service Guardian hôte (SGH), `<attestation protocol>` doit être `VBS-HGS`.
  - Si vous utilisez [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et Microsoft Azure attestation, `<attestation protocol>` doit être `SGX-AAS`.

- `<attestation URL>` : spécifie une URL d’attestation (un point de terminaison de service d’attestation). Vous devez obtenir une URL d’attestation pour votre environnement auprès de votre administrateur de services fédérés d’attestation.

  - Si vous utilisez [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)] et le service Guardian hôte (SGH), consultez [Déterminer et partager l’URL d’attestation SGH](../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Si vous utilisez [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et Microsoft Azure Attestation, consultez [Déterminer l’URL d’attestation de votre stratégie d’attestation](/azure-sql/database/always-encrypted-enclaves-configure-attestation#determine-the-attestation-url-for-your-attestation-policy).


Exemples de chaînes de connexion autorisant les calculs d’enclave pour une connexion de base de données :

- [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)]:
  
   ```
   Driver=ODBC Driver 17 for SQL Server;Server=myServer.myDomain;Database=myDataBase;Trusted_Connection=Yes;ColumnEncryption=VBS-HGS,http://myHGSServer.myDomain/Attestation
   ```

- [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] :
  
   ```
   Driver=ODBC Driver 17 for SQL Server;Server=myServer.database.windows.net;Database=myDataBase;Uid=myUsername;Pwd=myPassword;Encrypt=yes;ColumnEncryption=SGX-AAS,https://myAttestationProvider.uks.attest.azure.net/attest/SgxEnclave
   ```

Si le serveur et le service d’attestation sont configurés correctement, ainsi que les clés CMK et CEK compatibles avec les enclaves des colonnes souhaitées, vous devriez à présent pouvoir exécuter des requêtes qui utilisent l’enclave, par exemple le chiffrement sur place et les calculs enrichis, en plus des fonctionnalités déjà fournies par Always Encrypted. Pour plus d’informations, consultez [Configurer Always Encrypted avec des enclaves sécurisées](../../relational-databases/security/encryption/configure-always-encrypted-enclaves.md).


### <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Récupération et modification des données dans des colonnes chiffrées

Une fois que vous avez activé Always Encrypted sur une connexion, vous pouvez utiliser les API ODBC standard. Les API ODBC peuvent récupérer ou modifier des données dans des colonnes de base de données chiffrées. Les éléments de documentation suivants pourraient vous aider :

- [Exemple de code ODBC](cpp-code-example-app-connect-access-sql-db.md)
- [Guide de référence du programmeur ODBC](../../odbc/reference/odbc-programmer-s-reference.md)

Votre application doit disposer des autorisations de base de données requises, et avoir accès à la clé principale de colonne. Ensuite, le pilote chiffre tous les paramètres de requête qui ciblent des colonnes chiffrées. Il déchiffre également les données récupérées à partir de colonnes chiffrées. Il effectue toutes ces opérations de chiffrement et de déchiffrement sans aucune assistance de la part de votre code source. Pour votre programme, c’est comme si les colonnes n’étaient pas chiffrées.

Si Always Encrypted n’est pas activé, les requêtes ayant des paramètres qui ciblent des colonnes chiffrées échouent. Les données peuvent toujours être récupérées à partir des colonnes chiffrées, tant que la requête n’a aucun paramètre qui cible des colonnes chiffrées. Toutefois, le pilote ne tentera aucun déchiffrement et l’application recevra les données chiffrées binaires (sous la forme de tableaux d’octets).

Le tableau ci-dessous récapitule le comportement des requêtes, selon qu’Always Encrypted est activé ou non :

|Caractéristique de la requête | Always Encrypted est activé et l’application peut accéder aux clés et à leurs métadonnées|Always Encrypted est activé et l’application ne peut pas accéder aux clés et à leurs métadonnées | Always Encrypted est désactivé|
|:---|:---|:---|:---|
| Paramètres ciblant des colonnes chiffrées. | Des valeurs de paramètres sont chiffrées en toute transparence. | Error | Error|
| Récupération des données à partir de colonnes chiffrées, sans paramètres ciblant des colonnes chiffrées.| Les résultats de colonnes chiffrées sont déchiffrés de manière transparente. L’application reçoit des valeurs de colonne en texte clair. | Error | Les résultats des colonnes chiffrées ne sont pas déchiffrés. L’application reçoit des valeurs chiffrées sous la forme de tableaux d’octets.

Les exemples suivants illustrent la récupération et la modification de données dans des colonnes chiffrées. Les exemples partent du principe que la table a le schéma suivant. Notez que les colonnes SSN et BirthDate sont chiffrées.

```
CREATE TABLE [dbo].[Patients](
 [PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] )
 GO
```

#### <a name="data-insertion-example"></a>Exemple d’insertion de données

Cet exemple insère une ligne dans la table Patients. Notez les points suivants :

- L’exemple de code ne contient aucun élément spécifique au chiffrement. Le pilote détecte et chiffre automatiquement les valeurs des paramètres de date et SSN, qui ciblent des colonnes chiffrées. Le chiffrement est donc transparent pour l’application.

- Les valeurs insérées dans les colonnes de base de données, y compris les colonnes chiffrées, sont transmises sous forme de paramètres liés (voir [Fonction SQLBindParameter](../../odbc/reference/syntax/sqlbindparameter-function.md)). L’utilisation de paramètres est facultative lors de l’envoi de valeurs aux colonnes non chiffrées (même si elle est vivement recommandée, car elle contribue à empêcher l’injection SQL), mais elle est nécessaire pour les valeurs qui ciblent des colonnes chiffrées. Si les valeurs insérées dans les colonnes SSN ou BirthDate ont été passées en tant que littéraux incorporés dans l’instruction de requête, la requête échoue car le pilote ne tente pas de chiffrer ou de traiter les littéraux dans les requêtes. Par conséquent, le serveur les rejettera en les considérant comme incompatibles avec les colonnes chiffrées.

- Le type SQL du paramètre inséré dans la colonne SSN est défini sur SQL_CHAR, qui mappe au type de données SQL Server **char** (`rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);`). Si le type du paramètre était défini sur SQL_WCHAR, qui mappe à **nchar**, la requête échouerait, car Always Encrypted ne prend pas en charge les conversions côté serveur de valeurs nchar chiffrées en valeurs char chiffrées. Pour plus d’informations sur les mappages de type de données, consultez [Guide de référence du programmeur ODBC – Annexe D : Types de données](../../odbc/reference/appendixes/appendix-d-data-types.md).

```
    SQL_DATE_STRUCT date;
    SQLLEN cbdate;   // size of date structure  

    SQLCHAR SSN[12];
    strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

    SQLWCHAR* firstName = L"Catherine";
    SQLWCHAR* lastName = L"Abel";
    SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

    // Initialize the date structure  
    date.day = 10;
    date.month = 9;
    date.year = 1996;

    // Size of structures   
    cbdate = sizeof(SQL_DATE_STRUCT);

    SQLRETURN rc = 0;

    string queryText = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?) ";

    rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

    //SSN
    rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);
    //FirstName
    rc = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)firstName, 0, &cbFirstName);
    //LastName
    rc = SQLBindParameter(hstmt, 3, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)lastName, 0, &cbLastName);
    //BirthDate
    rc = SQLBindParameter(hstmt, 4, SQL_PARAM_INPUT, SQL_C_TYPE_DATE, SQL_TYPE_DATE, 10, 0, (SQLPOINTER)&date, 0, &cbdate);

    rc = SQLExecute(hstmt);
```

#### <a name="plaintext-data-retrieval-example"></a>Exemple d’extraction de données en texte clair

L’exemple suivant montre le filtrage de données basé sur des valeurs chiffrées, ainsi que la récupération de données en texte clair à partir de colonnes chiffrées. Notez les points suivants :

- La valeur utilisée dans la clause WHERE pour filtrer la colonne SSN doit être transmise avec SQLBindParameter, afin que le pilote puisse la chiffrer de manière transparente avant de l’envoyer au serveur.

- Toutes les valeurs imprimées par le programme sont en texte clair, car le pilote déchiffre de manière transparente les données récupérées à partir des colonnes SSN et BirthDate.

> [!NOTE]
> Les requêtes ne peuvent effectuer des comparaisons d’égalité sur des colonnes chiffrées que si le chiffrement est déterministe ou que l’enclave sécurisée est activée. Pour plus d’informations, consultez [Sélection d’un chiffrement déterministe ou aléatoire](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

```
SQLCHAR SSN[12];
strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

SQLWCHAR* firstName = L"Catherine";
SQLWCHAR* lastName = L"Abel";
SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

SQLRETURN rc = 0;
string empty = "";
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] " + empty +
    "FROM  [dbo].[Patients]" +
    "WHERE " +
    "[SSN] = ? ";

rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

//SSN
rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);

rc = SQLExecute(hstmt);
HandleDiagnosticRecord(hstmt, SQL_HANDLE_STMT, rc);

SQL_DATE_STRUCT dateVal;
SQLWCHAR firstNameVal[50];
SQLWCHAR lastNameVal[50];
SQLCHAR SSNVal[12];
SQLLEN cbdate;   // size of date structure  

int rowcount = 0;
while (SQL_SUCCEEDED(SQLFetch(hstmt)))
{
    rowcount++;
    SQLGetData(hstmt, 1, SQL_C_CHAR, &SSNVal, 11, &cbSSN);
    SQLGetData(hstmt, 2, SQL_C_WCHAR, &firstNameVal, 50, &cbFirstName);
    SQLGetData(hstmt, 3, SQL_C_WCHAR, &lastNameVal, 50, &cbLastName);
    SQLGetData(hstmt, 4, SQL_C_TYPE_DATE, &dateVal, 10, &cbdate);        
}
```

#### <a name="ciphertext-data-retrieval-example"></a>Exemple d’extraction de données chiffrées

Si Always Encrypted n’est pas activé, une requête peut toujours récupérer des données à partir de colonnes chiffrées, tant qu’aucun de ses paramètres ne ciblent des colonnes chiffrées.

L’exemple suivant illustre la récupération de données chiffrées binaires à partir de colonnes chiffrées. Notez les points suivants :

- Étant donné qu’Always Encrypted n’est pas toujours activé dans la chaîne de connexion, la requête retourne des valeurs SSN et BirthDate chiffrées sous la forme de tableaux d’octets (le programme convertit les valeurs en chaînes).
- Une requête qui récupère des données à partir de colonnes chiffrées lorsqu’Always Encrypted est désactivé peut avoir des paramètres, tant qu’aucun d’eux ne cible une colonne chiffrée. La requête ci-dessus filtre par nom (LastName), ce qui n’est pas chiffré dans la base de données. Si la requête filtre par SSN ou BirthDate, la requête échoue.


```
SQLCHAR SSN[12];
strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

SQLWCHAR* firstName = L"Catherine";
SQLWCHAR* lastName = L"Abel";
SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

SQLRETURN rc = 0;
string empty = "";
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] " + empty +
    "FROM  [dbo].[Patients]" +
    "WHERE " +
    "[LastName] = ?";

rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

//LastName
rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)lastName, 0, &cbLastName);

rc = SQLExecute(hstmt);
HandleDiagnosticRecord(hstmt, SQL_HANDLE_STMT, rc);

SQL_DATE_STRUCT dateVal;
SQLWCHAR firstNameVal[50];
SQLWCHAR lastNameVal[50];
SQLCHAR SSNVal[12];
SQLLEN cbdate;   // size of date structure  

int rowcount = 0;
while (SQL_SUCCEEDED(SQLFetch(hstmt)))
{
    rowcount++;
    SQLGetData(hstmt, 1, SQL_C_CHAR, &SSNVal, 11, &cbSSN);
    SQLGetData(hstmt, 2, SQL_C_WCHAR, &firstNameVal, 50, &cbFirstName);
    SQLGetData(hstmt, 3, SQL_C_WCHAR, &lastNameVal, 50, &cbLastName);
    SQLGetData(hstmt, 4, SQL_C_TYPE_DATE, &dateVal, 10, &cbdate);        
}
```

#### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Éviter les problèmes courants lors de l’interrogation de colonnes chiffrées

Cette section décrit des catégories d’erreurs courantes liées à l’interrogation des colonnes chiffrées à partir d’applications ODBC et fournit des conseils sur la façon de les éviter.

##### <a name="unsupported-data-type-conversion-errors"></a>Erreurs liées à la conversion de types de données non pris en charge

Always Encrypted ne prend en charge que peu de conversions de types de données chiffrées. Pour obtenir la liste détaillée des conversions de types prises en charge, consultez [Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Pour éviter les erreurs de conversion de types de données, veillez à observer les points suivants lors de l’utilisation de SQLBindParameter avec des paramètres ciblant des colonnes chiffrées :

- Le type SQL du paramètre est exactement le même que le type de la colonne cible, ou la conversion du type SQL vers le type de la colonne est prise en charge.

- La précision et l’échelle des paramètres ciblant les colonnes des types de données SQL Server `decimal` et `numeric` sont les mêmes que celles configurées pour la colonne cible.

- La précision des paramètres ciblant les colonnes des types de données SQL Server `datetime2`, `datetimeoffset` ou `time` n’est pas supérieure à celle de la colonne cible dans les requêtes qui modifient les valeurs de la colonne cible.  

##### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Erreurs dues au passage de texte en clair au lieu de valeurs chiffrées

Toute valeur qui cible une colonne chiffrée doit être chiffrée avant d’être envoyée au serveur. Toute tentative d’insertion, de modification ou de filtrage par une valeur en texte clair dans une colonne chiffrée entraîne une erreur. Pour éviter ces erreurs, effectuez les vérifications suivantes :

- Always Encrypted est activé (dans la source de données, la chaîne de connexion, avant d’établir la connexion en définissant l’attribut de connexion `SQL_COPT_SS_COLUMN_ENCRYPTION` pour une connexion spécifique, ou l’attribut d’instruction `SQL_SOPT_SS_COLUMN_ENCRYPTION` pour une instruction spécifique).

- Utilisez SQLBindParameter pour envoyer des données ciblant des colonnes chiffrées. L’exemple ci-dessous illustre une requête qui filtre incorrectement une colonne chiffrée (SSN) à l’aide d’un littéral/d’une constante, au lieu de passer le littéral comme argument à SQLBindParameter.

```
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
```

### <a name="precautions-when-using-sqlsetpos-and-sqlmoreresults"></a>Précautions en cas d’utilisation de SQLSetPos et SQLMoreResults

#### <a name="sqlsetpos"></a>SQLSetPos

L’API `SQLSetPos` permet à une application de mettre à jour des lignes dans un jeu de résultats à l’aide de mémoires tampons qui ont été liées avec SQLBindCol et dans lesquelles des lignes de données ont été extraites précédemment. En raison du comportement de remplissage asymétrique des types de longueur fixe chiffrés, il est possible de modifier de façon inattendue les données de ces colonnes lors de l’exécution de mises à jour sur d’autres colonnes de la ligne. Avec AE, les valeurs de caractères de longueur fixe seront complétées si la valeur est inférieure à la taille de la mémoire tampon.

Pour éviter ce comportement, utilisez l’indicateur `SQL_COLUMN_IGNORE` afin d’ignorer les colonnes qui ne seront pas mises à jour dans le cadre de `SQLBulkOperations` et en cas d’utilisation de `SQLSetPos` pour les mises à jour basées sur curseur.  Toutes les colonnes qui ne sont pas modifiées directement par l’application doivent être ignorées, à la fois pour des raisons de performances et pour éviter la troncation des colonnes qui sont liées à une mémoire tampon *plus petite* que leur taille réelle (DB). Pour plus d’informations, consultez la [référence de fonction SQLSetPos](../../odbc/reference/syntax/sqlsetpos-function.md).

#### <a name="sqlmoreresults--sqldescribecol"></a>SQLMoreResults et SQLDescribeCol

Les programmes d’application peuvent appeler [SQLDescribeCol](../../odbc/reference/syntax/sqldescribecol-function.md) pour retourner des métadonnées concernant les colonnes dans des instructions préparées.  Quand Always Encrypted est activé, le fait d’appeler `SQLMoreResults` *avant* `SQLDescribeCol` provoque un appel de [sp_describe_first_result_set](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md), qui ne retourne pas correctement les métadonnées en clair des colonnes chiffrées. Pour éviter ce problème, appelez `SQLDescribeCol` sur les instructions préparées *avant* d’appeler `SQLMoreResults`.

## <a name="controlling-the-performance-impact-of-always-encrypted"></a>Contrôle de l’impact d’Always Encrypted sur les performances

Always Encrypted étant une technologie de chiffrement côté client, la dégradation des performances s’observe côté client, et non dans la base de données. Outre le coût des opérations de chiffrement et de déchiffrement, les autres sources de dégradation des performances côté client sont les suivantes :

- Allers-retours supplémentaires vers la base de données pour récupérer des métadonnées pour les paramètres de requête.

- Appels au magasin de clés principales de colonne pour accéder à une clé principale de colonne.

Cette section décrit les outils intégrés d’optimisation des performances dans ODBC Driver for SQL Server et comment vous pouvez contrôler l’impact des deux facteurs ci-dessus sur les performances.

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>Contrôle des allers-retours en vue de la récupération des métadonnées pour les paramètres de requête

Si Always Encrypted est activé pour une connexion, le pilote appelle par défaut [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) pour chaque requête paramétrable, en passant l’instruction de requête (sans valeurs de paramètre) à SQL Server. Cette procédure stockée analyse l’instruction de requête afin de savoir si des paramètres doivent être chiffrés. Si c’est le cas, elle retourne pour chaque paramètre des informations relatives au chiffrement qui permettent au pilote de les chiffrer. Ce comportement garantit un haut niveau de transparence à l’application cliente : L’application et le développeur d’applications n’ont pas besoin de connaître les requêtes qui accèdent à des colonnes chiffrées, tant que les valeurs ciblant des colonnes chiffrées sont passées au pilote dans les paramètres.

À partir de la version 17.6, le pilote met également en cache les métadonnées de chiffrement pour les instructions préparées, ce qui améliore les performances en autorisant les appels suivants à `SQLExecute` pour ne pas nécessiter d’aller-retour supplémentaire pour récupérer les métadonnées de chiffrement.

### <a name="per-statement-always-encrypted-behavior"></a>Comportement d’Always Encrypted par instruction

Pour contrôler l’impact sur les performances de la récupération des métadonnées de chiffrement pour les requêtes paramétrables, vous pouvez modifier le comportement d’Always Encrypted pour chaque requête s’il a été activé sur la connexion. De cette façon, `sys.sp_describe_parameter_encryption` est appelé uniquement pour les requêtes dont les paramètres ciblent des colonnes chiffrées. Notez toutefois que, de cette façon, vous réduisez la transparence du chiffrement. Si vous chiffrez des colonnes supplémentaires dans votre base de données, vous devrez peut-être modifier le code de votre application pour l’aligner sur les modifications du schéma.

Pour contrôler le comportement Always Encrypted d’une instruction, appelez SQLSetStmtAttr afin d’affecter l’une des valeurs suivantes à l’attribut d’instruction `SQL_SOPT_SS_COLUMN_ENCRYPTION` :

|Valeur|Description|
|-|-|
|`SQL_CE_DISABLED` (0)|Always Encrypted est désactivé pour l’instruction|
|`SQL_CE_RESULTSETONLY` (1)|Déchiffrement uniquement. Les jeux de résultats et les valeurs de retour sont déchiffrés, et les paramètres ne sont pas chiffrés|
|`SQL_CE_ENABLED` (3) | Always Encrypted est activé et utilisé pour les paramètres et les résultats|

Les nouveaux handles d’instructions créés à partir d’une connexion avec Always Encrypted activé utilisent par défaut SQL_CE_ENABLED. Ceux qui sont créés à partir d’une connexion avec Always Encrypted désactivé utilisent par défaut SQL_CE_DISABLED (et il n’est pas possible d’activer Always Encrypted sur eux.)

Si la plupart des requêtes d’une application cliente accèdent à des colonnes chiffrées, suivez ces recommandations :

- Affectez au mot clé de chaîne de connexion `ColumnEncryption` la valeur `Enabled`.

- Affectez la valeur `SQL_CE_DISABLED` à l’attribut `SQL_SOPT_SS_COLUMN_ENCRYPTION` sur les instructions qui n’accèdent à aucune colonne chiffrée. Cela empêchera à la fois l’appel à `sys.sp_describe_parameter_encryption` et les tentatives de déchiffrement des valeurs du jeu de résultats.
    
- Affectez la valeur `SQL_CE_RESULTSETONLY` à l’attribut `SQL_SOPT_SS_COLUMN_ENCRYPTION` sur les instructions qui n’ont aucun paramètre exigeant un chiffrement, mais qui récupèrent des données à partir de colonnes chiffrées. Cela empêchera l’appel à `sys.sp_describe_parameter_encryption` et le chiffrement des paramètres. Les résultats contenant des colonnes chiffrées continueront à être déchiffrés.

- Utilisez des instructions préparées pour les requêtes qui seront exécutées plusieurs fois. Préparez la requête avec `SQLPrepare` et enregistrez le descripteur d’instruction, puis réutilisez-le avec `SQLExecute` chaque fois qu’il est exécuté. Il s’agit de l’approche recommandée pour les performances, même en l’absence de colonnes chiffrées et permet au pilote de tirer parti des métadonnées mises en cache.

## <a name="always-encrypted-security-settings"></a>Paramètres de sécurité d’Always Encrypted

### <a name="force-column-encryption"></a>Forcer le chiffrement de colonne

Pour appliquer le chiffrement d’un paramètre, définissez le champ de descripteur de paramètre d’implémentation (IPD) `SQL_CA_SS_FORCE_ENCRYPT` par le biais d’un appel à la fonction SQLSetDescField. Si la valeur est différente de zéro, le pilote retourne une erreur quand aucune métadonnée de chiffrement n’est retournée pour le paramètre associé.

```
SQLHDESC ipd;
SQLGetStmtAttr(hStmt, SQL_ATTR_IMP_PARAM_DESC, &ipd, 0, 0);
SQLSetDescField(ipd, paramNum, SQL_CA_SS_FORCE_ENCRYPT, (SQLPOINTER)TRUE, SQL_IS_SMALLINT);   
```

Si SQL Server informe le pilote que le paramètre ne nécessite pas de chiffrement, les requêtes utilisant ce paramètre échouent. Cette propriété fournit une protection supplémentaire contre les attaques au niveau de la sécurité qui impliquent un serveur SQL Server compromis fournissant des métadonnées de chiffrement incorrectes au client, ce qui peut entraîner la divulgation de données.

### <a name="column-encryption-key-caching"></a>Mise en cache des clés de chiffrement de colonne

Afin de réduire le nombre d’appels à un magasin de clés principales de colonne pour déchiffrer les clés de chiffrement de colonne, le pilote met en cache les clés CEK en clair dans la mémoire. Le cache de clés CEK est global pour le pilote et non associé à une connexion spécifique. Après avoir reçu la clé CEK chiffrée à partir des métadonnées de la base de données, le pilote tente d’abord de trouver la clé CEK en clair correspondant à la valeur de clé chiffrée qui se trouve dans le cache. Le pilote appelle le magasin de clés contenant la clé CMK uniquement s’il ne trouve pas la clé CEK en clair correspondante dans le cache.

> [!NOTE]
> Dans ODBC Driver for SQL Server, les entrées dans le cache sont supprimées après un délai d’attente de deux heures. Cela signifie que, pour une clé CEK chiffrée, le pilote ne contacte le magasin de clés qu’une seule fois au cours de la durée de vie de l’application, ou toutes les deux heures, la plus petite des deux valeurs étant retenue.

À compter d’ODBC Driver 17.1 for SQL Server, vous pouvez ajuster le délai d’expiration du cache de clés CEK à l’aide de l’attribut de connexion `SQL_COPT_SS_CEKCACHETTL`, qui spécifie le nombre de secondes pendant lesquelles une clé CEK restera dans le cache. En raison de la nature globale du cache, cet attribut peut être ajusté à partir de n’importe quel handle de connexion valide pour le pilote. Quand la durée de vie du cache est réduite, les clés CEK existantes qui dépasseraient la nouvelle durée de vie sont également supprimées. Si la valeur de durée de vie est 0, aucune clé CEK n’est mise en cache.

### <a name="trusted-key-paths"></a>Chemins de clés approuvés

À compter d’ODBC Driver 17.1 for SQL Server, l’attribut de connexion `SQL_COPT_SS_TRUSTEDCMKPATHS` permet à une application d’exiger que les opérations Always Encrypted utilisent uniquement une liste de clés CMK spécifiée, identifiées par leurs chemins de clé. Par défaut, cet attribut est NULL, ce qui signifie que le pilote accepte n’importe quel chemin de clé. Pour utiliser cette fonctionnalité, définissez `SQL_COPT_SS_TRUSTEDCMKPATHS` de façon à pointer vers une chaîne à caractères larges, délimitée par des caractères Null, se terminant par un caractère Null, qui liste les chemins de clés autorisés. La mémoire vers laquelle pointe cet attribut doit rester valide pendant les opérations de chiffrement ou de déchiffrement utilisant le handle de connexion sur lequel elle est définie. Le pilote vérifiera alors si le chemin de la clé principale de colonne spécifié par les métadonnées du serveur figure dans cette liste (sans respect de la casse). Si le chemin de la clé principale de colonne ne figure pas dans la liste, l’opération échoue. L’application peut changer le contenu de la mémoire vers laquelle cet attribut pointe, afin de modifier sa liste de clés CMK approuvées, sans redéfinir l’attribut.

## <a name="working-with-column-master-key-stores"></a>Utilisation de magasins de clés principales de colonne

Pour chiffrer ou déchiffrer des données, le pilote doit obtenir une clé CEK configurée pour la colonne cible. Les clés CEK sont stockées sous forme chiffrée (on parle alors de clés ECEK) dans les métadonnées de la base de données. Chaque clé CEK a une clé CMK correspondante qui a été utilisée pour la chiffrer. Les [métadonnées de base de données](../../t-sql/statements/create-column-master-key-transact-sql.md) ne stockent pas la clé CMK proprement dite ; elles contiennent uniquement le nom du magasin de clés et des informations que ce dernier peut utiliser pour localiser la clé CMK.

Pour obtenir la valeur de texte en clair d’une clé ECEK, le pilote obtient d’abord les métadonnées relatives à la clé CEK et sa clé CMK correspondante, il utilise ces informations pour contacter le magasin de clés contenant la clé CMK, puis il lui demande de déchiffrer la clé ECEK. Le pilote communique avec un magasin de clés à l’aide d’un fournisseur de magasin de clés.

### <a name="built-in-keystore-providers"></a>Fournisseurs de magasins de clés intégrés

ODBC Driver for SQL Server est fourni avec les fournisseurs de magasins de clés intégrés suivants :

| Nom | Description | Nom du fournisseur (de métadonnées) |Disponibilité|
|:---|:---|:---|:---|
|Azure Key Vault |Stocke les clés CMK dans un coffre de clés Azure | `AZURE_KEY_VAULT` |Windows, macOS, Linux|
|Magasin de certificats Windows|Stocke les clés CMK localement dans le magasin de clés Windows| `MSSQL_CERTIFICATE_STORE`|Windows|

- Vous (ou votre administrateur de base de données) devez vérifier que le nom du fournisseur (configuré dans les métadonnées de clé principale de colonne) est correct et que le chemin de la clé principale de colonne est conforme au format du chemin de clé pour le fournisseur donné. Il est recommandé de configurer les clés à l’aide d’outils tels que SQL Server Management Studio, qui génère automatiquement des noms de fournisseurs et des chemins de clés valides lors de l’émission de l’instruction [CREATE COLUMN MASTER KEY (Transact-SQL)](../../t-sql/statements/create-column-master-key-transact-sql.md) .

- Vous devez vérifier que votre application peut accéder à la clé dans le magasin de clés. Pour cela, vous devrez peut-être accorder à votre application l’accès à la clé et/ou au magasin de clés (selon le magasin de clés) ou effectuer d’autres étapes de configuration spécifiques au magasin de clés. Par exemple, pour accéder à un coffre de clés Azure, vous devez fournir les informations d’identification correctes au magasin de clés.

### <a name="using-the-azure-key-vault-provider"></a>Utilisation d’Azure Key Vault Provider

Azure Key Vault (AKV) est un outil est très pratique qui permet de stocker et de gérer des clés principales de colonne Always Encrypted, en particulier si vos applications sont hébergées dans Azure. ODBC Driver for SQL Server sur Linux, macOS et Windows inclut un fournisseur de magasin de clés principales de colonne intégré pour Azure Key Vault. Pour plus d’informations sur la configuration d’un coffre de clés Azure pour Always Encrypted, consultez [Azure Key Vault – Step by Step](/archive/blogs/kv/azure-key-vault-step-by-step), [Qu’est-ce qu’Azure Key Vault ?](/azure/key-vault/general/overview) et [Créer et stocker des clés principales de colonne (Azure Key Vault)](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md#creating-column-master-keys-in-azure-key-vault).

> [!NOTE]
> Le pilote ODBC prend en charge uniquement l’authentification Azure Key Vault directe auprès d’Azure Active Directory. Si vous utilisez l’authentification Azure Active Directory auprès d’Azure Key Vault et que votre configuration Active Directory exige l’authentification auprès d’un point de terminaison ADFS (Active Directory Federation Services), l’authentification risque d’échouer.
> Sur Linux et macOS, pour les versions 17.2 et ultérieures du pilote, `libcurl` est nécessaire pour utiliser ce fournisseur, mais n’est pas une dépendance explicite dans la mesure où les autres opérations avec le pilote n’en ont pas besoin. Si vous rencontrez une erreur concernant `libcurl`, vérifiez qu’il est installé.

Le pilote prend en charge l’authentification auprès d’Azure Key Vault avec les types d’informations d’identification suivants :

- Nom d’utilisateur/mot de passe : avec cette méthode, les informations d’identification sont le nom d’un utilisateur Azure Active Directory et son mot de passe.

- ID client/secret : avec cette méthode, les informations d’identification sont un ID de client d’application et un secret d’application.

- Identité managée (17.5.2+) – affectée par le système ou l’utilisateur ; consultez [Identités managées pour les ressources Azure](/azure/active-directory/managed-identities-azure-resources/) pour plus d’informations.

Pour autoriser le pilote à utiliser des clés CMK stockées dans Azure Key Vault pour le chiffrement de colonne, utilisez les mots clés de chaîne de connexion uniquement suivants :

|Type d'informations d'identification|<code>KeyStoreAuthentication</code>|<code>KeyStorePrincipalId</code>|<code>KeyStoreSecret</code>|
|-|-|-|-|
|Nom d'utilisateur/mot de passe| `KeyVaultPassword`|Nom d’utilisateur principal|Mot de passe|
|ID client/secret| `KeyVaultClientSecret`|ID client|Secret|
|Identité managée|`KeyVaultManagedIdentity`|ID d’objet (facultatif, affecté par l’utilisateur uniquement)|(non spécifié)|

#### <a name="example-connection-strings"></a>Exemples de chaîne de connexion

Les chaînes de connexion suivantes montrent comment s’authentifier auprès d’Azure Key Vault avec les deux types d’informations d’identification :

**ID client/secret** :

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultClientSecret;KeyStorePrincipalId=<clientId>;KeyStoreSecret=<secret>
```

**Nom d'utilisateur/Mot de passe** :

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultPassword;KeyStorePrincipalId=<username>;KeyStoreSecret=<password>
```

**Identité managée (affectée par le système)**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultManagedIdentity
```

**Identité managée (affectée par l’utilisateur)**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultManagedIdentity;KeyStorePrincipalId=<objectID>
```

Aucune autre modification d’application ODBC n’est requise pour utiliser Azure Key Vault pour le stockage des clés CMK.

> [!NOTE]
> Le pilote contient la liste des points de terminaison AKV auxquels il fait confiance. À partir de la version 17.5.2 du pilote, cette liste est configurable : définissez la propriété `AKVTrustedEndpoints` dans la clé de Registre (Windows) ODBCINST.INI ou ODBC.INI ou dans la section du fichier `odbcinst.ini` ou `odbc.ini` (Linux/macOS) du pilote ou du DNS sous forme de liste délimitée par des points-virgules. En la définissant dans le DSN, elle devient prioritaire par rapport à un paramètre du pilote. Si la valeur commence par un point-virgule, elle étend la liste par défaut ; sinon, elle la remplace. La liste par défaut (à partir de 17.5) est `vault.azure.net;vault.azure.cn;vault.usgovcloudapi.net;vault.microsoftazure.de`.


### <a name="using-the-windows-certificate-store-provider"></a>Avec le fournisseur du magasin de certificats Windows

ODBC Driver for SQL Server sur Windows inclut un fournisseur de magasin de clés CMK intégré pour le Magasin de certificats Windows, nommé `MSSQL_CERTIFICATE_STORE`. (Ce fournisseur n’est pas disponible sur Mac OS ou Linux.) Avec ce fournisseur, la clé CMK est stockée localement sur l’ordinateur client, et aucune configuration supplémentaire par l’application n’est nécessaire pour l’utiliser avec le pilote. Toutefois, l’application doit avoir accès au certificat et à sa clé privée dans le magasin. Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne (Always Encrypted)](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

### <a name="using-custom-keystore-providers"></a>Utilisation de fournisseurs de magasins de clés personnalisés

ODBC Driver for SQL Server prend également en charge les fournisseurs de magasins de clés tiers personnalisés à l’aide de l’interface CEKeystoreProvider. Cela permet à une application de charger, d’interroger et de configurer des fournisseurs de magasin de clés afin que le pilote puisse les utiliser pour accéder aux colonnes chiffrées. Les applications peuvent également interagir directement avec un fournisseur de magasin de clés afin de chiffrer des clés CEK pour le stockage dans SQL Server, et effectuer des tâches au-delà de l’accès aux colonnes chiffrées avec ODBC. Pour plus d’informations, consultez [Fournisseurs de magasins de clés personnalisés](custom-keystore-providers.md).

Deux attributs de connexion sont utilisés pour interagir avec les fournisseurs de magasins de clés personnalisés. Il s'agit de :

- `SQL_COPT_SS_CEKEYSTOREPROVIDER`

- `SQL_COPT_SS_CEKEYSTOREDATA`

Le premier sert à charger et à énumérer les fournisseurs de magasins de clés chargés, tandis que le second autorise les communications avec les fournisseurs d’applications. Ces attributs de connexion peuvent être utilisés à tout moment, avant ou après avoir établi une connexion, puisque l’interaction avec le fournisseur d’application n’implique pas de communication avec SQL Server. Toutefois, le pilote n’ayant pas encore été chargé, si vous définissez et obtenez ces attributs avant de vous connecter, ils seront traités par le Gestionnaire de pilotes et risquent de ne pas générer les résultats attendus.

#### <a name="loading-a-keystore-provider"></a>Chargement d’un fournisseur de magasin de clés

La définition de l’attribut de connexion `SQL_COPT_SS_CEKEYSTOREPROVIDER` permet à une application cliente de charger une bibliothèque de fournisseurs, rendant ainsi utilisables les fournisseurs de magasins de clés contenus dans celle-ci.

```
SQLRETURN SQLSetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER StringLength);
```

| Argument | Description |
|:---|:---|
|`ConnectionHandle`|[Entrée] Handle de connexion. Il doit s’agir d’un handle de connexion valide, mais les fournisseurs chargés par le biais d’un handle de connexion sont accessibles à partir de n’importe quel autre handle dans le même processus.|
|`Attribute`|[Entrée] Attribut à définir : constante `SQL_COPT_SS_CEKEYSTOREPROVIDER`.|
|`ValuePtr`|[Entrée] Pointeur vers une chaîne de caractères se terminant par Null et spécifiant le nom de fichier de la bibliothèque de fournisseurs. Pour SQLSetConnectAttrA, il s’agit d’une chaîne ANSI (multioctet). Pour SQLSetConnectAttrW, il s’agit d’une chaîne Unicode (wchar_t).|
|`StringLength`|[Entrée] Longueur de la chaîne ValuePtr, ou SQL_NTS.|

Le pilote tente de charger la bibliothèque identifiée par le paramètre ValuePtr à l’aide du mécanisme de chargement de bibliothèque dynamique défini par la plateforme (`dlopen()` sur Linux et macOS, `LoadLibrary()` sur Windows), et il ajoute tous les fournisseurs qui y sont définis à la liste des fournisseurs connus du pilote. Les erreurs suivantes peuvent se produire :

| Error | Description |
|:--|:--|
|`CE203`|Impossible de charger la bibliothèque dynamique.|
|`CE203`|Le symbole exporté « CEKeyStoreProvider » est introuvable dans la bibliothèque.|
|`CE203`|Un ou plusieurs fournisseurs dans la bibliothèque sont déjà chargés.|

`SQLSetConnectAttr` retourne les valeurs d’erreur ou de succès habituelles, et des informations supplémentaires sont disponibles pour toutes les erreurs qui se sont produites par le biais du mécanisme de diagnostic ODBC standard.

> [!NOTE]
> Le programmeur d’application doit s’assurer que les fournisseurs personnalisés soient chargés avant qu’une requête ayant besoin d’eux soit envoyée par le biais d’une connexion. L’échec de cette opération entraîne l’erreur :

| Error | Description |
|:--|:--|
|`CE200`|Le fournisseur de magasins de clés %1 est introuvable. Assurez-vous que la bibliothèque du fournisseur de magasins de clés appropriée a été chargée.|

> [!NOTE]
> Les implémenteurs de fournisseurs de magasins de clés doivent éviter l’utilisation de `MSSQL` dans le nom de leurs fournisseurs personnalisés. Ce terme est réservé exclusivement à une utilisation par Microsoft, et peut provoquer des conflits avec des fournisseurs intégrés ultérieurs. L’utilisation de ce terme dans le nom d’un fournisseur personnalisé peut générer un avertissement ODBC.

#### <a name="getting-the-list-of-loaded-providers"></a>Obtention de la liste des fournisseurs chargés

L’obtention de cet attribut de connexion permet à une application cliente d’identifier les fournisseurs de magasins de clés actuellement chargés dans le pilote (y compris les fournisseurs intégrés). Cette opération peut uniquement être effectuée après la connexion.

```
SQLRETURN SQLGetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER BufferLength, SQLINTEGER * StringLengthPtr);
```

| Argument | Description |
|:---|:---|
|`ConnectionHandle`|[Entrée] Handle de connexion. Il doit s’agir d’un handle de connexion valide, mais les fournisseurs chargés par le biais d’un handle de connexion sont accessibles à partir de n’importe quel autre handle dans le même processus.|
|`Attribute`|[Entrée] Attribut à récupérer : constante `SQL_COPT_SS_CEKEYSTOREPROVIDER`.|
|`ValuePtr`|[Sortie] Pointeur vers la mémoire dans laquelle retourner le nom du fournisseur chargé suivant.|
|`BufferLength`|[Entrée] Longueur de l’objet ValuePtr de la mémoire tampon.|
|`StringLengthPtr`|[Sortie] Pointeur vers une mémoire tampon dans laquelle retourner le nombre total d’octets (sans le caractère Null de fin) pouvant être retournés dans \*ValuePtr. Si ValuePtr est un pointeur Null, aucune longueur n’est retournée. Si la valeur d’attribut est une chaîne de caractères et que le nombre d’octets pouvant être retournés est supérieur à BufferLength moins la longueur du caractère Null de fin, les données dans \*ValuePtr sont tronquées à BufferLength moins la longueur du caractère Null de fin, et le caractère Null de fin est inséré par le pilote.|

Pour permettre la récupération de la liste complète, chaque opération Get retourne le nom du fournisseur actuel et incrémente un compteur interne. Une fois que ce compteur atteint la fin de la liste, une chaîne vide ("") est retournée et le compteur est réinitialisé. Les opérations Get suivantes reprennent ensuite à partir du début de la liste.

### <a name="communicating-with-keystore-providers"></a>Communication avec les fournisseurs de magasins de clés

L’attribut de connexion `SQL_COPT_SS_CEKEYSTOREDATA` permet à une application cliente de communiquer avec les fournisseurs de magasins de clés chargés afin entre autres de configurer des paramètres supplémentaires. La communication entre une application cliente et un fournisseur suit un protocole de demande-réponse simple, basé sur des requêtes Get et Set utilisant cet attribut de connexion. La communication est établie uniquement par l’application cliente.

> [!NOTE]
> Étant donné la nature de la réponse de CEKeyStoreProvider aux appels ODBC (SQLGet/SetConnectAttr), l’interface ODBC prend uniquement en charge la définition des données à la résolution du contexte de connexion.

L’application communique avec les fournisseurs de magasins de clés par le biais du pilote via la structure CEKeystoreData :

```
typedef struct CEKeystoreData {
wchar_t *name;
unsigned int dataSize;
char data[];
} CEKEYSTOREDATA;
```

| Argument | Description |
|:---|:---|
|`name`|[Entrée] En cas d’opération Set, nom du fournisseur auquel les données sont envoyées. Ignoré en cas d’opération Get. Chaîne à caractères larges se terminant par le caractère Null.|
|`dataSize`|[Entrée] Taille du tableau de données respectant la structure.|
|`data`|[InOut] En cas d’opération Set, données à envoyer au fournisseur. Il peut s’agir de données arbitraires ; le pilote ne tente pas de les interpréter. En cas d’opération Get, mémoire tampon devant recevoir les données lues à partir du fournisseur.|

#### <a name="writing-data-to-a-provider"></a>Écriture de données vers un fournisseur

Un appel `SQLSetConnectAttr` utilisant l’attribut `SQL_COPT_SS_CEKEYSTOREDATA` écrit un « paquet » de données dans le fournisseur de magasins de clés spécifié.
```
SQLRETURN SQLSetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER StringLength);
```

| Argument | Description |
|:---|:---|
|`ConnectionHandle`| [Entrée] Handle de connexion. Il doit s’agir d’un handle de connexion valide, mais les fournisseurs chargés par le biais d’un handle de connexion sont accessibles à partir de n’importe quel autre handle dans le même processus.|
|`Attribute`|[Entrée] Attribut à définir : constante `SQL_COPT_SS_CEKEYSTOREDATA`.|
|`ValuePtr`|[Entrée] Pointeur vers une structure CEKeystoreData. Le champ de nom de la structure identifie le fournisseur pour lequel les données sont prévues.|
|`StringLength`|[Entrée] Constante SQL_IS_POINTER|

Des informations détaillées supplémentaires sur les erreurs peuvent être obtenues par le biais de [SQLGetDiacRec](../../odbc/reference/syntax/sqlgetdescrec-function.md).

> [!NOTE]
> Le fournisseur peut utiliser le handle de connexion pour associer les données écrites à une connexion spécifique, s’il le souhaite. Cela est utile pour implémenter la configuration par connexion. Il peut également ignorer le contexte de connexion et traiter les données de manière identique quelle que soit la connexion utilisée pour envoyer les données. Pour plus d’informations; consultez [Association de contexte](custom-keystore-providers.md#context-association).

#### <a name="reading-data-from-a-provider"></a>Lecture de données à partir d’un fournisseur

Un appel à `SQLGetConnectAttr` à l’aide de l’attribut `SQL_COPT_SS_CEKEYSTOREDATA` lit un « paquet » de données à partir du fournisseur *dans lequel la dernière écriture a été effectuée*. S’il n’y en a eu aucune, une erreur de séquence de fonction se produit. Nous encourageons les implémenteurs de fournisseurs de magasins de clés à prendre en charge les « écritures factices » de 0 octet comme moyen de sélectionner le fournisseur pour les opérations de lecture sans provoquer d’autres effets secondaires, si cela est pertinent.

```
SQLRETURN SQLGetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER BufferLength, SQLINTEGER * StringLengthPtr);
```

| Argument | Description |
|:---|:---|
|`ConnectionHandle`|[Entrée] Handle de connexion. Il doit s’agir d’un handle de connexion valide, mais les fournisseurs chargés par le biais d’un handle de connexion sont accessibles à partir de n’importe quel autre handle dans le même processus.|
|`Attribute`|[Entrée] Attribut à récupérer : constante `SQL_COPT_SS_CEKEYSTOREDATA`.|
|`ValuePtr`|[Sortie] Pointeur vers une structure CEKeystoreData dans laquelle les données lues à partir du fournisseur sont placées.|
|`BufferLength`|[Entrée] Constante SQL_IS_POINTER|
|`StringLengthPtr`|[Sortie] Pointeur vers une mémoire tampon dans laquelle retourner BufferLength. Si *ValuePtr est un pointeur Null, aucune longueur n’est retournée.|

L’appelant doit s’assurer qu’une mémoire tampon de longueur suffisante suivant la structure CEKEYSTOREDATA est allouée pour l’écriture par le fournisseur. Au retour, son champ dataSize est mis à jour avec la longueur réelle des données lues à partir du fournisseur. Des informations détaillées supplémentaires sur les erreurs peuvent être obtenues par le biais de [SQLGetDiacRec](../../odbc/reference/syntax/sqlgetdescrec-function.md).

Cette interface n’impose aucune exigence supplémentaire sur le format des données transférées entre une application et un fournisseur de magasin de clés. Chaque fournisseur peut définir son propre format de data/protocole, en fonction de ses besoins.

Pour obtenir un exemple d’implémentation de votre propre fournisseur de magasin de clés, consultez [Fournisseurs de magasins de clés personnalisés](custom-keystore-providers.md)

## <a name="limitations-of-the-odbc-driver-when-using-always-encrypted"></a>Limitations du pilote ODBC lors de l’utilisation d’Always Encrypted

### <a name="asynchronous-operations"></a>Opérations asynchrones
Bien que le pilote ODBC autorise l’utilisation des [opérations asynchrones](../../relational-databases/native-client/odbc/creating-a-driver-application-asynchronous-mode-and-sqlcancel.md) avec Always Encrypted, cela a un impact sur les performances quand Always Encrypted est activé. L’appel à `sys.sp_describe_parameter_encryption` pour déterminer les métadonnées de chiffrement pour l’instruction a un caractère bloquant, et obligera le pilote à attendre que le serveur retourne les métadonnées avant de retourner `SQL_STILL_EXECUTING`.

### <a name="retrieve-data-in-parts-with-sqlgetdata"></a>Récupérer une partie des données avec SQLGetData
Avant la version 17 du pilote ODBC pour SQL Server, il n’est pas possible de récupérer une partie des colonnes binaires et des caractères chiffrés avec SQLGetData. Un seul appel de SQLGetData est possible, avec une mémoire tampon de taille suffisante pour contenir la totalité des données de la colonne.

### <a name="send-data-in-parts-with-sqlputdata"></a>Envoyer une partie des données avec SQLPutData
Avant ODBC Driver 17.3 for SQL Server, il n’est pas possible d’envoyer une partie des données à des fins de comparaison ou d’insertion avec SQLPutData. Un seul appel à SQLPutData est possible, avec une mémoire tampon contenant la totalité des données. Pour insérer des données de type long dans des colonnes chiffrées, utilisez l’API de copie en bloc, décrite dans la section suivante, avec un fichier de données d’entrée.

### <a name="encrypted-money-and-smallmoney"></a>Colonnes money et smallmoney chiffrées
Les paramètres ne peuvent pas cibler les colonnes **money** et **smallmoney**, car il n’existe aucun type de données ODBC correspondant spécifiquement à ces types, ce qui provoque des erreurs de conflit de type opérande.

## <a name="bulk-copy-of-encrypted-columns"></a>Copie en bloc de colonnes chiffrées

Les [fonctions de copie en bloc SQL](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md) et l’utilitaire **bcp** sont pris en charge avec Always Encrypted depuis la version 17 du pilote ODBC pour SQL Server. Le texte en clair (chiffré lors de l’insertion et déchiffré lors de la récupération) et le texte chiffré (transféré textuellement) peuvent être insérés et récupérés à l’aide des API de copie en bloc (bcp_&#42;) et de l’utilitaire **bcp**.

- Pour récupérer du texte chiffré au format varbinary(max) (par exemple, pour le chargement en masse dans une autre base de données), connectez-vous sans l’option `ColumnEncryption` (ou en lui donnant la valeur `Disabled`) et effectuez une opération BCP OUT.

- Pour insérer et récupérer du texte clair et laisser le pilote effectuer en toute transparence le chiffrement et le déchiffrement, il suffit de donner la valeur `Enabled` à `ColumnEncryption`. Les fonctionnalités de l’API BCP restent inchangées par ailleurs.

- Pour insérer du texte chiffré au format varbinary(max) (tel qu’il a été récupéré ci-dessus, par exemple), donnez la valeur TRUE à l’option `BCPMODIFYENCRYPTED` et effectuez une opération BCP IN. Pour que les données résultantes soient déchiffrables, la clé CEK de la colonne de destination doit être la même que celle avec laquelle le texte chiffré a été obtenu à l’origine.

Si vous utilisez l’utilitaire **bcp** : pour contrôler le paramètre `ColumnEncryption`, utilisez l’option -D et spécifiez un nom de source de données contenant la valeur souhaitée. Pour insérer du texte chiffré, veillez à ce que le paramètre `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` de l’utilisateur soit activé.

Le tableau suivant fournit un résumé des actions en cas d’opération sur une colonne chiffrée :

|<code>ColumnEncryption</code>|Direction BCP|Description|
|----------------|-------------|-----------|
|`Disabled`|OUT (vers le client)|Récupère le texte chiffré. Le type de données observé est **varbinary(max)** .|
|`Enabled`|OUT (vers le client)|Récupère le texte en clair. Le pilote déchiffre les données de colonne.|
|`Disabled`|IN (vers le serveur)|Insère le texte chiffré. A pour but de déplacer de manière opaque des données chiffrées sans exiger leur déchiffrement. L’opération échoue si l’option `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` n’est pas définie sur l’utilisateur, ou si BCPMODIFYENCRYPTED n’est pas défini sur le handle de connexion. Pour plus d’informations, voir ci-dessous.|
|`Enabled`|IN (vers le serveur)|Insère le texte en clair. Le pilote chiffre les données de colonne.|

### <a name="the-bcpmodifyencrypted-option"></a>Option BCPMODIFYENCRYPTED

Pour empêcher l’altération des données, le serveur n’autorise en principe pas l’insertion de texte chiffré directement dans une colonne chiffrée (toute tentative échouera). Toutefois, pour le chargement en bloc de données chiffrées avec l’API BCP, si l’option `BCPMODIFYENCRYPTED` [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) a la valeur TRUE, il est possible d’insérer directement du texte chiffré, ce qui réduit le risque d’altération des données chiffrées par rapport à l’option `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` sur le compte d’utilisateur. Néanmoins, les clés doivent correspondre aux données, et il est judicieux d’effectuer des vérifications en lecture seule des données insérées après l’insertion en bloc et avant toute utilisation ultérieure.

Pour plus d’informations, consultez [Migrer des données sensibles protégées par Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="always-encrypted-api-summary"></a>Synthèse de l’API Always Encrypted

### <a name="connection-string-keywords"></a>Mots clés de chaîne de connexion

|Nom|Description|  
|----------|-----------------|  
|`ColumnEncryption`|Les valeurs acceptées sont `Enabled`/`Disabled`.<br>`Enabled` : active la fonctionnalité Always Encrypted pour la connexion.<br>`Disabled` : désactive la fonctionnalité Always Encrypted pour la connexion.<br>*le protocole d’attestation*,*l’URL d’attestation* : (version 17.4 et ultérieure) activent Always Encrypted avec enclave sécurisée à l’aide du protocole d’attestation spécifié et de l’URL d’attestation. <br><br>Par défaut, il s’agit de `Disabled`.|
|`KeyStoreAuthentication` | Valeurs valides : `KeyVaultPassword`, `KeyVaultClientSecret` |
|`KeyStorePrincipalId` | Quand `KeyStoreAuthentication` = `KeyVaultPassword`, affectez à cette valeur un nom d’utilisateur principal Azure Active Directory valide. <br>Quand `KeyStoreAuthetication` = `KeyVaultClientSecret`, affectez à cette valeur un ID client d’application Azure Active Directory valide. |
|`KeyStoreSecret` | Quand `KeyStoreAuthentication` = `KeyVaultPassword`, affectez à cette valeur le mot de passe du nom d’utilisateur correspondant. <br>Quand `KeyStoreAuthentication` = `KeyVaultClientSecret`, affectez à cette valeur le secret d’application associé à un ID client d’application Azure Active Directory valide. |


### <a name="connection-attributes"></a>Attributs de connexion

|Nom|Type|Description|  
|----------|-------|----------|  
|`SQL_COPT_SS_COLUMN_ENCRYPTION`|Avant la connexion|`SQL_COLUMN_ENCRYPTION_DISABLE` (0) -- Désactiver Always Encrypted <br>`SQL_COLUMN_ENCRYPTION_ENABLE` (1) -- Activer Always Encrypted<br> le pointeur vers la chaîne du *protocole d’attestation*,*URL d’attestation* : (version 17.4 et ultérieure) s’active avec enclave sécurisée|
|`SQL_COPT_SS_CEKEYSTOREPROVIDER`|Après la connexion|[Set] Tenter de charger CEKeystoreProvider<br>[Get] Retourner un nom CEKeystoreProvider|
|`SQL_COPT_SS_CEKEYSTOREDATA`|Après la connexion|[Set] Écrire les données dans CEKeystoreProvider<br>[Get] Lire les données à partir de CEKeystoreProvider|
|`SQL_COPT_SS_CEKCACHETTL`|Après la connexion|[Set] Définir la durée de vie du cache CEK<br>[Get] Récupérer la durée de vie actuelle du cache CEK|
|`SQL_COPT_SS_TRUSTEDCMKPATHS`|Après la connexion|[Set] Définir le pointeur des chemins de clés CMK approuvés<br>[Get] Obtenir le pointeur actuel des chemins de clés CMK approuvés|

### <a name="statement-attributes"></a>Attributs d'instruction

|Nom|Description|  
|----------|-----------------|  
|`SQL_SOPT_SS_COLUMN_ENCRYPTION`|`SQL_CE_DISABLED` (0) -- Always Encrypted est désactivé pour l’instruction <br>`SQL_CE_RESULTSETONLY` (1) -- Déchiffrement uniquement. Les jeux de résultats et les valeurs de retour sont déchiffrés, et les paramètres ne sont pas chiffrés <br>`SQL_CE_ENABLED` (3) -- Always Encrypted est activé et utilisé pour les paramètres et les résultats|

### <a name="descriptor-fields"></a>Champs de descripteur

|Champ IPD|Taille/Type|Valeur par défaut|Description|
|-|-|-|-|  
|`SQL_CA_SS_FORCE_ENCRYPT` (1236)|WORD (deux octets)|0|Quand ce champ a la valeur 0 (valeur par défaut) : la décision de chiffrer ce paramètre est déterminé par la disponibilité des métadonnées de chiffrement.<br><br>Quand ce champ a une valeur différente de zéro : si les métadonnées de chiffrement sont disponibles pour ce paramètre, il est chiffré. Sinon, la requête échoue avec l’erreur [CE300] [Microsoft][ODBC Driver 17 for SQL Server]Le chiffrement obligatoire a été spécifié pour un paramètre, mais aucune métadonnée de chiffrement n’a été fournie par le serveur.|

### <a name="bcp_control-options"></a>Options de bcp_control

|Nom d’option|Valeur par défaut|Description|
|-|-|-|
|`BCPMODIFYENCRYPTED` (21)|FALSE|Quand la valeur est TRUE, autorise l’insertion de valeurs varbinary(max) dans une colonne chiffrée. Quand la valeur est FALSE, empêche l’insertion, sauf si les métadonnées de chiffrement et le type correct sont fournis.|

## <a name="troubleshooting"></a>Dépannage

Si vous rencontrez des difficultés avec Always Encrypted, commencez par vérifier les points suivants :

- La clé CEK qui chiffre la colonne souhaitée est présente et accessible sur le serveur.

- La clé CMK qui chiffre le clé CEK comporte des métadonnées accessibles sur le serveur et est également accessible à partir du client.

- `ColumnEncryption` est activé dans le nom de source de données, la chaîne de connexion ou l’attribut de connexion et, si vous utilisez l’enclave sécurisée, est au bon format.

Par ailleurs, si vous utilisez l’enclave sécurisée, les défaillances d’attestation permettent d’identifier l’étape du processus d’attestation où la défaillance s’est produite, selon le tableau suivant :

|Étape|Description|
|----|-----------|
|0-99| Réponse d’attestation non valide ou erreur lors de la vérification de la signature. |
|100-199| Erreur lors de la récupération des certificats à partir de l’URL d’attestation. Vérifiez que `<attestation URL>/v2.0/signingCertificates` est valide et accessible. |
|200-299| Format inattendu ou incorrect de l’identité de l’enclave. |
|300-399| Erreur lors de l’établissement d’un canal sécurisé avec l’enclave. |

## <a name="see-also"></a>Voir aussi

- [Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Always Encrypted avec enclaves sécurisées](../../relational-databases/security/encryption/always-encrypted-enclaves.md)