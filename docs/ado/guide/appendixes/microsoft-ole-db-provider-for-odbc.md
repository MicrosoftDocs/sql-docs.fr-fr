---
description: Vue d’ensemble du fournisseur Microsoft OLE DB pour ODBC
title: Fournisseur Microsoft OLE DB pour ODBC | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB provider for ODBC [ADO]
- providers [ADO], OLE DB provider for ODBC
ms.assetid: 2dc0372d-e74d-4d0f-9c8c-04e5a168c148
author: rothja
ms.author: jroth
ms.openlocfilehash: 5a6cfdda6072f8ed51d55569ed68b113a4148642
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029263"
---
# <a name="microsoft-ole-db-provider-for-odbc-overview"></a>Vue d’ensemble du fournisseur Microsoft OLE DB pour ODBC
Pour un programmeur ADO ou RDS, un monde idéal serait celui dans lequel chaque source de données expose une interface OLE DB, afin qu’ADO puisse appeler directement dans la source de données. Bien que de plus en plus de fournisseurs de bases de données implémentent des interfaces OLE DB, certaines sources de données ne sont pas encore exposées de cette façon. Toutefois, la plupart des systèmes SGBD en cours d’utilisation sont accessibles via ODBC.

 Les pilotes ODBC sont disponibles pour tous les principaux SGBD utilisés aujourd’hui, y compris Microsoft SQL Server, Microsoft Access (moteur de base de données Microsoft Jet) et Microsoft FoxPro, en plus des produits de base de données non Microsoft tels qu’Oracle.

 Toutefois, le fournisseur Microsoft ODBC permet à ADO de se connecter à n’importe quelle source de données ODBC. Le fournisseur est libre de thread et Unicode est activé.

 Le fournisseur prend en charge les transactions, bien que différents moteurs SGBD offrent différents types de prise en charge des transactions. Par exemple, Microsoft Access prend en charge les transactions imbriquées jusqu’à cinq niveaux.

 Il s’agit du fournisseur par défaut pour ADO, et toutes les propriétés et méthodes ADO dépendant du fournisseur sont prises en charge.

## <a name="connection-string-parameters"></a>Paramètres de chaîne de connexion
 Pour vous connecter à ce fournisseur, affectez à l’argument **Provider =** de la propriété [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) la valeur :

```
MSDASQL
```

 La lecture de la propriété [Provider](../../reference/ado-api/provider-property-ado.md) retourne également cette chaîne.

## <a name="typical-connection-string"></a>Chaîne de connexion classique
 Une chaîne de connexion classique pour ce fournisseur est la suivante :

```
"Provider=MSDASQL;DSN=dsnName;UID=MyUserID;PWD=MyPassword;"
```

 La chaîne se compose des mots clés suivants :

|Mot clé|Description|
|-------------|-----------------|
|**Fournisseur**|Spécifie le fournisseur de OLE DB pour ODBC.|
|**DSN**|Spécifie le nom de la source de données.|
|**UID**|Spécifie le nom d'utilisateur.|
|**PWD**|Spécifie le mot de passe de l’utilisateur.|
|**URL**|Spécifie l’URL d’un fichier ou d’un répertoire publié dans un dossier Web.|

 Étant donné qu’il s’agit du fournisseur par défaut pour ADO, si vous omettez le paramètre **Provider =** de la chaîne de connexion, ADO essaiera d’établir une connexion à ce fournisseur.

> [!NOTE]
>  Si vous vous connectez à un fournisseur de sources de données qui prend en charge l’authentification Windows, vous devez spécifier **Trusted_Connection = Yes** ou **Integrated Security = SSPI** à la place des informations d’ID d’utilisateur et de mot de passe dans la chaîne de connexion.

 Le fournisseur ne prend pas en charge les paramètres de connexion spécifiques en plus de ceux définis par ADO. Toutefois, le fournisseur passe tous les paramètres de connexion non ADO au gestionnaire de pilotes ODBC.

 Étant donné que vous pouvez omettre le paramètre **Provider** , vous pouvez donc composer une chaîne de connexion ADO identique à une chaîne de connexion ODBC pour la même source de données. Utilisez les mêmes noms de paramètres (**Driver =**, **Database =**, **DSN =**, etc.), les valeurs et la syntaxe que vous le feriez lors de la composition d’une chaîne de connexion ODBC. Vous pouvez vous connecter avec ou sans nom de source de données prédéfini (DSN) ou FileDSN.

## <a name="syntax-with-a-dsn-or-filedsn"></a>Syntaxe avec un DSN ou un FileDSN :

```
"[Provider=MSDASQL;] { DSN=name | FileDSN=filename } ;
[DATABASE=database;] UID=user; PWD=password"
```

## <a name="syntax-without-a-dsn-dsn-less-connection"></a>Syntaxe sans DSN (connexion sans DSN) :

```
"[Provider=MSDASQL;] DRIVER=driver; SERVER=server;
DATABASE=database; UID=MyUserID; PWD=MyPassword"
```

## <a name="remarks"></a>Notes
 Si vous utilisez un **DSN** ou un **FILEDSN**, vous devez le définir via l’administrateur de la source de données ODBC dans le panneau de configuration Windows. Dans Microsoft Windows 2000, l’administrateur ODBC se trouve sous outils d’administration. Dans les versions antérieures de Windows, l’icône administrateur ODBC est nommée **odbc 32 bits** ou simplement **ODBC**.

 Comme alternative à la définition d’un **DSN**, vous pouvez spécifier le pilote ODBC (**Driver =**), par exemple « SQL Server », le nom du serveur (**Server =**); et le nom de la base de données (**Database =**).

 Vous pouvez également spécifier un nom de compte d’utilisateur (**UID =**) et le mot de passe du compte d’utilisateur (**PWD =**) dans les paramètres spécifiques à ODBC ou dans les paramètres d' *utilisateur* et de *mot de passe* définis par ADO standard.

 Bien qu’une définition de **DSN** spécifie déjà une base de données, vous pouvez spécifier *un* paramètre *de base de données* en plus d’un nom de **source** de données pour vous connecter à une autre base de données. Il est judicieux de toujours inclure le paramètre  *de base de données* lorsque vous utilisez un **nom de source** de données. Cela permet de s’assurer que vous vous connectez à la base de données correcte si un autre utilisateur a modifié le paramètre de base de données par défaut depuis la dernière vérification de la définition du **DSN** .

## <a name="provider-specific-connection-properties"></a>Provider-Specific les propriétés de connexion
 Le fournisseur OLE DB pour ODBC ajoute plusieurs propriétés à la collection [Properties](../../reference/ado-api/properties-collection-ado.md) de l’objet **Connection** . Le tableau suivant répertorie ces propriétés avec le nom de la propriété OLE DB correspondante entre parenthèses.

|Nom de la propriété|Description|
|-------------------|-----------------|
|Procédures accessibles (KAGPROP_ACCESSIBLEPROCEDURES)|Indique si l’utilisateur a accès aux procédures stockées.|
|Tables accessibles (KAGPROP_ACCESSIBLETABLES)|Indique si l’utilisateur a l’autorisation d’exécuter des instructions SELECT sur les tables de base de données.|
|Instructions actives (KAGPROP_ACTIVESTATEMENTS)|Indique le nombre de handles qu’un pilote ODBC peut prendre en charge sur une connexion.|
|Nom du pilote (KAGPROP_DRIVERNAME)|Indique le nom de fichier du pilote ODBC.|
|Version ODBC du pilote (KAGPROP_DRIVERODBCVER)|Indique la version d’ODBC que ce pilote prend en charge.|
|Utilisation des fichiers (KAGPROP_FILEUSAGE)|Indique comment le pilote traite un fichier dans une source de données ; sous forme de table ou de catalogue.|
|Like, clause d’échappement (KAGPROP_LIKEESCAPECLAUSE)|Indique si le pilote prend en charge la définition et l’utilisation d’un caractère d’échappement pour le caractère de pourcentage (%) et le caractère de soulignement (_) dans le prédicat LIKE d’une clause WHERE.|
|Nombre maximal de colonnes dans Group by (KAGPROP_MAXCOLUMNSINGROUPBY)|Indique le nombre maximal de colonnes qui peuvent être listées dans la clause GROUP BY d’une instruction SELECT.|
|Nombre maximal de colonnes dans l’index (KAGPROP_MAXCOLUMNSININDEX)|Indique le nombre maximal de colonnes qui peuvent être incluses dans un index.|
|Nombre maximal de colonnes dans l’ordre (KAGPROP_MAXCOLUMNSINORDERBY)|Indique le nombre maximal de colonnes qui peuvent être listées dans la clause ORDER BY d’une instruction SELECT.|
|Nombre maximal de colonnes dans Select (KAGPROP_MAXCOLUMNSINSELECT)|Indique le nombre maximal de colonnes qui peuvent être affichées dans la partie SELECT d’une instruction SELECT.|
|Nombre maximal de colonnes dans la table (KAGPROP_MAXCOLUMNSINTABLE)|Indique le nombre maximal de colonnes autorisées dans une table.|
|Fonctions numériques (KAGPROP_NUMERICFUNCTIONS)|Indique quelles fonctions numériques sont prises en charge par le pilote ODBC. Pour obtenir la liste des noms de fonctions et les valeurs associées utilisées dans ce masque de réutiliser, consultez [annexe E : fonctions scalaires](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md)dans la documentation ODBC.|
|Fonctionnalités de jointure externe (KAGPROP_OJCAPABILITY)|Indique les types de JOINTUREs externes pris en charge par le fournisseur.|
|Jointures externes (KAGPROP_OUTERJOINS)|Indique si le fournisseur prend en charge les JOINTUREs externes.|
|Caractères spéciaux (KAGPROP_SPECIALCHARACTERS)|Indique les caractères qui ont une signification particulière pour le pilote ODBC.|
|Procédures stockées (KAGPROP_PROCEDURES)|Indique si les procédures stockées peuvent être utilisées avec ce pilote ODBC.|
|Fonctions de chaîne (KAGPROP_STRINGFUNCTIONS)|Indique les fonctions de chaîne prises en charge par le pilote ODBC. Pour obtenir la liste des noms de fonctions et les valeurs associées utilisées dans ce masque de réutiliser, consultez [annexe E : fonctions scalaires](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md)dans la documentation ODBC.|
|Fonctions système (KAGPROP_SYSTEMFUNCTIONS)|Indique quelles fonctions système sont prises en charge par le pilote ODBC. Pour obtenir la liste des noms de fonctions et les valeurs associées utilisées dans ce masque de réutiliser, consultez [annexe E : fonctions scalaires](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md)dans la documentation ODBC.|
|Fonctions de date/heure (KAGPROP_TIMEDATEFUNCTIONS)|Indique les fonctions de date et d’heure prises en charge par le pilote ODBC. Pour obtenir la liste des noms de fonctions et les valeurs associées utilisées dans ce masque de réutiliser, consultez [annexe E : fonctions scalaires](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md)dans la documentation ODBC.|
|Prise en charge de la grammaire SQL (KAGPROP_ODBCSQLCONFORMANCE)|Indique la grammaire SQL prise en charge par le pilote ODBC.|

## <a name="provider-specific-recordset-and-command-properties"></a>Propriétés de commande et Recordset Provider-Specific
 Le fournisseur OLE DB pour ODBC ajoute plusieurs propriétés à la collection **Properties** des objets **Recordset** et **Command** . Le tableau suivant répertorie ces propriétés avec le nom de la propriété OLE DB correspondante entre parenthèses.

|Nom de la propriété|Description|
|-------------------|-----------------|
|Mises à jour/insertions basées sur une requête (KAGPROP_QUERYBASEDUPDATES)|Indique si les mises à jour, les suppressions et les insertions peuvent être effectuées à l’aide de requêtes SQL.|
|Type d’accès concurrentiel ODBC (KAGPROP_CONCURRENCY)|Indique la méthode utilisée pour réduire les problèmes potentiels causés par deux utilisateurs tentant d’accéder simultanément aux mêmes données à partir de la source de données.|
|Accessibilité des objets BLOB sur Forward-Only curseur (KAGPROP_BLOBSONFOCURSOR)|Indique si les **champs** BLOB sont accessibles lors de l’utilisation d’un curseur avant uniquement.|
|Inclure SQL_FLOAT, SQL_DOUBLE et SQL_REAL dans les clauses d’emplacement de QBU (KAGPROP_INCLUDENONEXACT)|Indique si SQL_FLOAT, SQL_DOUBLE et SQL_REAL valeurs peuvent être incluses dans une clause WHERE QBU.|
|Position sur la dernière ligne après Insert (KAGPROP_POSITIONONNEWROW)|Indique qu’après l’insertion d’un nouvel enregistrement dans une table, la dernière ligne de la table est la ligne actuelle.|
|IRowsetChangeExtInfo (KAGPROP_IROWSETCHANGEEXTINFO)|Indique si l’interface **IRowsetChange** fournit la prise en charge des informations étendues.|
|Type de curseur ODBC (KAGPROP_CURSOR)|Indique le type de curseur utilisé par le **Recordset**.|
|Générer un ensemble de lignes qui peut être marshalé (KAGPROP_MARSHALLABLE)|Indique que le pilote ODBC génère un jeu d’enregistrements qui peut être marshalé|

## <a name="command-text"></a>Texte de la commande
 La façon dont vous utilisez l’objet [Command](../../reference/ado-api/command-object-ado.md) dépend en grande partie de la source de données et du type d’instruction de requête ou de commande qu’elle accepte.

 ODBC fournit une syntaxe spécifique pour appeler des procédures stockées. Pour la [propriété CommandText](../../reference/ado-api/commandtext-property-ado.md) d’un objet **Command** , l' *argument CommandText* de la méthode **Execute** sur un objet [Connection](../../reference/ado-api/connection-object-ado.md) , ou l’argument *source* de la méthode **Open** sur un objet [Recordset](../../reference/ado-api/recordset-object-ado.md) , transmet une chaîne avec la syntaxe suivante :

```
"{ [ ? = ] call procedure [ ( ? [, ? [ , ... ]] ) ] }"
```

 Chacun **?** fait référence à un objet dans la collection [Parameters](../../reference/ado-api/parameters-collection-ado.md) . La première **?** référence les **paramètres**(0), le suivant **?** référence les **paramètres**(1), et ainsi de suite.

 Les références de paramètres sont facultatives et dépendent de la structure de la procédure stockée. Si vous souhaitez appeler une procédure stockée qui ne définit aucun paramètre, votre chaîne ressemble à ce qui suit :

```
"{ call procedure }"
```

 Si vous avez deux paramètres de requête, votre chaîne ressemble à ce qui suit :

```
"{ call procedure ( ?, ? ) }"
```

 Si la procédure stockée retourne une valeur, la valeur de retour est traitée comme un autre paramètre. Si vous n’avez pas de paramètres de requête, mais que vous avez une valeur de retour, votre chaîne ressemble à ce qui suit :

```
"{ ? = call procedure }"
```

 Enfin, si vous avez une valeur de retour et deux paramètres de requête, votre chaîne ressemble à ce qui suit :

```
"{ ? = call procedure ( ?, ? ) }"
```

## <a name="recordset-behavior"></a>Comportement du Recordset
 Les tableaux suivants répertorient les méthodes et propriétés ADO standard disponibles sur un objet **Recordset** ouvert avec ce fournisseur.

 Pour plus d’informations sur le comportement du **Recordset** pour la configuration de votre fournisseur, exécutez la méthode [supports](../../reference/ado-api/supports-method.md) et Énumérez la collection **Properties** du **Recordset** pour déterminer si des propriétés dynamiques spécifiques au fournisseur sont présentes.

 Disponibilité des propriétés standard du **Recordset** ADO :

|Propriété|ForwardOnly|Dynamique|Keyset|statique|
|--------------|-----------------|-------------|------------|------------|
|[AbsolutePage](../../reference/ado-api/absolutepage-property-ado.md)|non disponible|non disponible|lecture/écriture|lecture/écriture|
|[AbsolutePosition](../../reference/ado-api/absoluteposition-property-ado.md)|non disponible|non disponible|lecture/écriture|lecture/écriture|
|[ActiveConnection](../../reference/ado-api/activeconnection-property-ado.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[Business](../../reference/ado-api/bof-eof-properties-ado.md)|en lecture seule|en lecture seule|en lecture seule|en lecture seule|
|[Signet](../../reference/ado-api/bookmark-property-ado.md)|non disponible|non disponible|lecture/écriture|lecture/écriture|
|[CacheSize](../../reference/ado-api/cachesize-property-ado.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[Ait](../../reference/ado-api/cursorlocation-property-ado.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[CursorType](../../reference/ado-api/cursortype-property-ado.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[EditMode](../../reference/ado-api/editmode-property.md)|en lecture seule|en lecture seule|en lecture seule|en lecture seule|
|[Filter](../../reference/ado-api/filter-property.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[Verrou](../../reference/ado-api/locktype-property-ado.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[MarshalOptions](../../reference/ado-api/marshaloptions-property-ado.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[MaxRecords](../../reference/ado-api/maxrecords-property-ado.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[PageCount](../../reference/ado-api/pagecount-property-ado.md)|lecture/écriture|non disponible|en lecture seule|en lecture seule|
|[PageSize](../../reference/ado-api/pagesize-property-ado.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[RecordCount](../../reference/ado-api/recordcount-property-ado.md)|lecture/écriture|non disponible|en lecture seule|en lecture seule|
|[Source](../../reference/ado-api/source-property-ado-recordset.md)|lecture/écriture|lecture/écriture|lecture/écriture|lecture/écriture|
|[State](../../reference/ado-api/state-property-ado.md)|en lecture seule|en lecture seule|en lecture seule|en lecture seule|
|[État](../../reference/ado-api/status-property-ado-recordset.md)|en lecture seule|en lecture seule|en lecture seule|en lecture seule|

 Les propriétés [AbsolutePosition](../../reference/ado-api/absoluteposition-property-ado.md) et [AbsolutePage](../../reference/ado-api/absolutepage-property-ado.md) sont en écriture seule quand ADO est utilisé avec la version 1,0 du fournisseur Microsoft OLE DB pour ODBC.

 Disponibilité des méthodes de l’ensemble d' **enregistrements** ADO standard :

|Méthode|ForwardOnly|Dynamique|Keyset|statique|
|------------|-----------------|-------------|------------|------------|
|[AddNew](../../reference/ado-api/addnew-method-ado.md)|Oui|Oui|Oui|Oui|
|[Annuler](../../reference/ado-api/cancel-method-ado.md)|Oui|Oui|Oui|Oui|
|[CancelBatch](../../reference/ado-api/cancelbatch-method-ado.md)|Oui|Oui|Oui|Oui|
|[CancelUpdate](../../reference/ado-api/cancelupdate-method-ado.md)|Oui|Oui|Oui|Oui|
|[Répliqué](../../reference/ado-api/clone-method-ado.md)|Non|Non|Oui|Oui|
|[Close](../../reference/ado-api/close-method-ado.md)|Oui|Oui|Oui|Oui|
|[Supprimer](../../reference/ado-api/delete-method-ado-recordset.md)|Oui|Oui|Oui|Oui|
|[GetRows](../../reference/ado-api/getrows-method-ado.md)|Oui|Oui|Oui|Oui|
|[Déplacer](../../reference/ado-api/move-method-ado.md)|Oui|Oui|Oui|Oui|
|[MoveFirst](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|Oui|Oui|Oui|Oui|
|[MoveLast](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|Non|Oui|Oui|Oui|
|[MoveNext](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|Oui|Oui|Oui|Oui|
|[MovePrevious](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|Non|Oui|Oui|Oui|
|[NextRecordset](../../reference/ado-api/nextrecordset-method-ado.md)*|Oui|Oui|Oui|Oui|
|[Ouvrir](../../reference/ado-api/open-method-ado-recordset.md)|Oui|Oui|Oui|Oui|
|[Requery](../../reference/ado-api/requery-method.md)|Oui|Oui|Oui|Oui|
|[Resynchroniser](../../reference/ado-api/resync-method.md)|Non|Non|Oui|Oui|
|[Prise en charge](../../reference/ado-api/supports-method.md)|Oui|Oui|Oui|Oui|
|[Mettre à jour](../../reference/ado-api/update-method.md)|Oui|Oui|Oui|Oui|
|[UpdateBatch](../../reference/ado-api/updatebatch-method.md)|Oui|Oui|Oui|Oui|

 * Non pris en charge pour les bases de données Microsoft Access.

## <a name="dynamic-properties"></a>Propriétés dynamiques
 Le fournisseur Microsoft OLE DB pour ODBC insère plusieurs propriétés dynamiques dans la collection **Properties** des objets [Connection](../../reference/ado-api/connection-object-ado.md), [Recordset](../../reference/ado-api/recordset-object-ado.md)et [Command](../../reference/ado-api/command-object-ado.md) inouverts.

 Les tableaux suivants sont un index croisé des noms ADO et OLE DB pour chaque propriété dynamique. Le Guide de référence du programmeur OLE DB fait référence à un nom de propriété ADO par le terme « Description ». Vous trouverez plus d’informations sur ces propriétés dans le Guide de référence du programmeur OLE DB. Recherchez le nom de la propriété OLE DB dans l’index ou consultez l' [annexe C : propriétés du OLE DB](/previous-versions/windows/desktop/ms723130(v=vs.85)).

## <a name="connection-dynamic-properties"></a>Propriétés dynamiques de la connexion
 Les propriétés suivantes sont ajoutées à la collection de **Propriétés** de l’objet de **connexion** .

|Nom de la propriété ADO|Nom de la propriété OLE DB|
|-----------------------|--------------------------|
|Sessions actives|DBPROP_ACTIVESESSIONS|
|Abandon Asynchable|DBPROP_ASYNCTXNABORT|
|Validation Asynchable|DBPROP_ASYNCTNXCOMMIT|
|Niveaux d’isolement de validation automatique|DBPROP_SESS_AUTOCOMMITISOLEVELS|
|Emplacement du catalogue|DBPROP_CATALOGLOCATION|
|Terme du catalogue|DBPROP_CATALOGTERM|
|Définition de colonne|DBPROP_COLUMNDEFINITION|
|Connect Timeout|DBPROP_INIT_TIMEOUT|
|Catalogue actuel|DBPROP_CURRENTCATALOG|
|source de données|DBPROP_INIT_DATASOURCE|
|Nom de la source de données|DBPROP_DATASOURCENAME|
|Modèle de thread de l’objet de source de données|DBPROP_DSOTHREADMODEL|
|Nom SGBD|DBPROP_DBMSNAME|
|Version de SGBD|DBPROP_DBMSVER|
|Extended Properties|DBPROP_INIT_PROVIDERSTRING|
|Prise en charge de GROUP BY|DBPROP_GROUPBY|
|Prise en charge des tables hétérogènes|DBPROP_HETEROGENEOUSTABLES|
|Respect de la casse des identificateurs|DBPROP_IDENTIFIERCASE|
|Initial Catalog|DBPROP_INIT_CATALOG|
|Niveaux d’isolation|DBPROP_SUPPORTEDTXNISOLEVELS|
|Rétention de l’isolation|DBPROP_SUPPORTEDTXNISORETAIN|
|Identificateur de paramètres régionaux|DBPROP_INIT_LCID|
|Emplacement|DBPROP_INIT_LOCATION|
|Taille maximale de l’index|DBPROP_MAXINDEXSIZE|
|Taille de ligne maximale|DBPROP_MAXROWSIZE|
|La taille de ligne maximale comprend l’objet BLOB|DBPROP_MAXROWSIZEINCLUDESBLOB|
|Nombre maximal de tables dans SELECT|DBPROP_MAXTABLESINSELECT|
|Mode|DBPROP_INIT_MODE|
|Jeux de paramètres multiples|DBPROP_MULTIPLEPARAMSETS|
|Résultats multiples|DBPROP_MULTIPLERESULTS|
|Objets de stockage multiples|DBPROP_MULTIPLESTORAGEOBJECTS|
|Mise à jour de plusieurs tables|DBPROP_MULTITABLEUPDATE|
|Ordre de classement NULL|DBPROP_NULLCOLLATION|
|Comportement de concaténation NULL|DBPROP_CONCATNULLBEHAVIOR|
|Services OLE DB|DBPROP_INIT_OLEDBSERVICES|
|Version de OLE DB|DBPROP_PROVIDEROLEDBVER|
|Prise en charge des objets OLE|DBPROP_OLEOBJECTS|
|Prise en charge des ensembles de lignes ouverts|DBPROP_OPENROWSETSUPPORT|
|CLASSER par colonnes dans la liste de sélection|DBPROP_ORDERBYCOLUMNSINSELECT|
|Disponibilité des paramètres de sortie|DBPROP_OUTPUTPARAMETERAVAILABILITY|
|Mot de passe|DBPROP_AUTH_PASSWORD|
|Passer par les accesseurs de référence|DBPROP_BYREFACCESSORS|
|Persist Security Info|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|
|Type d’ID persistant|DBPROP_PERSISTENTIDTYPE|
|Comportement de préparation d’abandon|DBPROP_PREPAREABORTBEHAVIOR|
|Préparer le comportement de validation|DBPROP_PREPARECOMMITBEHAVIOR|
|Terme de procédure|DBPROP_PROCEDURETERM|
|Prompt|DBPROP_INIT_PROMPT|
|Nom convivial du fournisseur|DBPROP_PROVIDERFRIENDLYNAME|
|Nom du fournisseur|DBPROP_PROVIDERFILENAME|
|Version du fournisseur|DBPROP_PROVIDERVER|
|Read-Only source de données|DBPROP_DATASOURCEREADONLY|
|Conversions d’ensemble de lignes sur la commande|DBPROP_ROWSETCONVERSIONSONCOMMAND|
|Terme de schéma|DBPROP_SCHEMATERM|
|Utilisation du schéma|DBPROP_SCHEMAUSAGE|
|Prise en charge SQL|DBPROP_SQLSUPPORT|
|Stockage structuré|DBPROP_STRUCTUREDSTORAGE|
|Prise en charge des sous-requêtes|DBPROP_SUBQUERIES|
|Terme de table|DBPROP_TABLETERM|
|DDL de la transaction|DBPROP_SUPPORTEDTXNDDL|
|ID d'utilisateur|DBPROP_AUTH_USERID|
|User Name|DBPROP_USERNAME|
|Handle de la fenêtre|DBPROP_INIT_HWND|

## <a name="recordset-dynamic-properties"></a>Propriétés dynamiques du Recordset
 Les propriétés suivantes sont ajoutées à la collection **Properties** de l’objet **Recordset** .

|Nom de la propriété ADO|Nom de la propriété OLE DB|
|-----------------------|--------------------------|
|Ordre d’accès|DBPROP_ACCESSORDER|
|Blocage des objets de stockage|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Type de signet|DBPROP_BOOKMARKTYPE|
|Avec signet|DBPROP_IROWSETLOCATE|
|Modifier les lignes insérées|DBPROP_CHANGEINSERTEDROWS|
|Privilèges de colonne|DBPROP_COLUMNRESTRICT|
|Notification du jeu de colonnes|DBPROP_NOTIFYCOLUMNSET|
|Retarder les mises à jour des objets de stockage|DBPROP_DELAYSTORAGEOBJECTS|
|Extraire vers l’arrière|DBPROP_CANFETCHBACKWARDS|
|Conserver les lignes|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|Interfaces|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|Lignes immobiles|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IRowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IRowsetLocate|DBPROP_IRowsetLocate|
|IRowsetResynch||
|IRowsetUpdate|DBPROP_IRowsetUpdate.|
|ISequentialStream|DBPROP_ISequentialStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|Signets littéraux|DBPROP_LITERALBOOKMARKS|
|Identité de ligne littérale|DBPROP_LITERALIDENTITY|
|Nombre maximal de lignes ouvertes|DBPROP_MAXOPENROWS|
|Nombre maximal de lignes en attente|DBPROP_MAXPENDINGROWS|
|Lignes au maximum|DBPROP_MAXROWS|
|Granularité des notifications|DBPROP_NOTIFICATIONGRANULARITY|
|Phases de notification|DBPROP_NOTIFICATIONPHASES|
|Objets traités|DBPROP_TRANSACTEDOBJECT|
|Propres modifications visibles|DBPROP_OWNUPDATEDELETE|
|Insertions visibles|DBPROP_OWNINSERT|
|Conserver lors de l’abandon|DBPROP_ABORTPRESERVE|
|Conserver lors de la validation|DBPROP_COMMITPRESERVE|
|Redémarrage rapide|DBPROP_QUICKRESTART|
|Événements réentrants|DBPROP_REENTRANTEVENTS|
|Supprimer les lignes supprimées|DBPROP_REMOVEDELETED|
|Signaler plusieurs modifications|DBPROP_REPORTMULTIPLECHANGES|
|Retourner les insertions en attente|DBPROP_RETURNPENDINGINSERTS|
|Notification de suppression de ligne|DBPROP_NOTIFYROWDELETE|
|Notification de modification de la première ligne|DBPROP_NOTIFYROWFIRSTCHANGE|
|Notification d’insertion de ligne|DBPROP_NOTIFYROWINSERT|
|Privilèges de ligne|DBPROP_ROWRESTRICT|
|Notification de resynchronisation de ligne|DBPROP_NOTIFYROWRESYNCH|
|Modèle de thread de ligne|DBPROP_ROWTHREADMODEL|
|Notification de modification de ligne annuler|DBPROP_NOTIFYROWUNDOCHANGE|
|Notification de suppression de ligne annuler|DBPROP_NOTIFYROWUNDODELETE|
|Notification d’insertion de ligne annuler|DBPROP_NOTIFYROWUNDOINSERT|
|Notification de mise à jour de ligne|DBPROP_NOTIFYROWUPDATE|
|Notification de modification de position d’extraction d’ensemble de lignes|DBPROP_NOTIFYROWSETFETCHPOSISIONCHANGE|
|Notification de libération de l’ensemble de lignes|DBPROP_NOTIFYROWSETRELEASE|
|Défilement vers l’arrière|DBPROP_CANSCROLLBACKWARDS|
|Ignorer les signets supprimés|DBPROP_BOOKMARKSKIPPED|
|Identité de ligne forte|DBPROP_STRONGITDENTITY|
|Lignes uniques|DBPROP_UNIQUEROWS|
|Mise à jour|DBPROP_UPDATABILITY|
|Utiliser des signets|DBPROP_BOOKMARKS|

## <a name="command-dynamic-properties"></a>Propriétés dynamiques de commande
 Les propriétés suivantes sont ajoutées à la collection de **Propriétés** de l’objet de **commande** .

|Nom de la propriété ADO|Nom de la propriété OLE DB|
|-----------------------|--------------------------|
|Ordre d’accès|DBPROP_ACCESSORDER|
|Blocage des objets de stockage|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Type de signet|DBPROP_BOOKMARKTYPE|
|Avec signet|DBPROP_IROWSETLOCATE|
|Modifier les lignes insérées|DBPROP_CHANGEINSERTEDROWS|
|Privilèges de colonne|DBPROP_COLUMNRESTRICT|
|Notification du jeu de colonnes|DBPROP_NOTIFYCOLUMNSET|
|Retarder les mises à jour des objets de stockage|DBPROP_DELAYSTORAGEOBJECTS|
|Extraire vers l’arrière|DBPROP_CANFETCHBACKWARDS|
|Conserver les lignes|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|Interfaces|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|Lignes immobiles|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IRowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IRowsetLocate|DBPROP_IRowsetLocate|
|IRowsetResynch||
|IRowsetUpdate|DBPROP_IRowsetUpdate.|
|ISequentialStream|DBPROP_ISequentialStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|Signets littéraux|DBPROP_LITERALBOOKMARKS|
|Identité de ligne littérale|DBPROP_LITERALIDENTITY|
|Nombre maximal de lignes ouvertes|DBPROP_MAXOPENROWS|
|Nombre maximal de lignes en attente|DBPROP_MAXPENDINGROWS|
|Lignes au maximum|DBPROP_MAXROWS|
|Granularité des notifications|DBPROP_NOTIFICATIONGRANULARITY|
|Phases de notification|DBPROP_NOTIFICATIONPHASES|
|Objets traités|DBPROP_TRANSACTEDOBJECT|
|Propres modifications visibles|DBPROP_OWNUPDATEDELETE|
|Insertions visibles|DBPROP_OWNINSERT|
|Conserver lors de l’abandon|DBPROP_ABORTPRESERVE|
|Conserver lors de la validation|DBPROP_COMMITPRESERVE|
|Redémarrage rapide|DBPROP_QUICKRESTART|
|Événements réentrants|DBPROP_REENTRANTEVENTS|
|Supprimer les lignes supprimées|DBPROP_REMOVEDELETED|
|Signaler plusieurs modifications|DBPROP_REPORTMULTIPLECHANGES|
|Retourner les insertions en attente|DBPROP_RETURNPENDINGINSERTS|
|Notification de suppression de ligne|DBPROP_NOTIFYROWDELETE|
|Notification de modification de la première ligne|DBPROP_NOTIFYROWFIRSTCHANGE|
|Notification d’insertion de ligne|DBPROP_NOTIFYROWINSERT|
|Privilèges de ligne|DBPROP_ROWRESTRICT|
|Notification de resynchronisation de ligne|DBPROP_NOTIFYROWRESYNCH|
|Modèle de thread de ligne|DBPROP_ROWTHREADMODEL|
|Notification de modification de ligne annuler|DBPROP_NOTIFYROWUNDOCHANGE|
|Notification de suppression de ligne annuler|DBPROP_NOTIFYROWUNDODELETE|
|Notification d’insertion de ligne annuler|DBPROP_NOTIFYROWUNDOINSERT|
|Notification de mise à jour de ligne|DBPROP_NOTIFYROWUPDATE|
|Notification de modification de position d’extraction d’ensemble de lignes|DBPROP_NOTIFYROWSETFETCHPOSITIONCHANGE|
|Notification de libération de l’ensemble de lignes|DBPROP_NOTIFYROWSETRELEASE|
|Défilement vers l’arrière|DBPROP_CANSCROLLBACKWARDS|
|Ignorer les signets supprimés|DBPROP_BOOKMARKSKIP|
|Identité de ligne forte|DBPROP_STRONGIDENTITY|
|Mise à jour|DBPROP_UPDATABILITY|
|Utiliser des signets|DBPROP_BOOKMARKS|

 Pour plus d’informations sur l’implémentation spécifique et les informations fonctionnelles sur le fournisseur Microsoft OLE DB pour ODBC, consultez le [Guide de référence du programmeur OLE DB](/previous-versions/windows/desktop/ms713643(v=vs.85)) ou visitez le site Web Centre de développement d’accès aux données et de stockage sur MSDN.

## <a name="see-also"></a>Voir aussi
 [Command, objet (ADO)](../../reference/ado-api/command-object-ado.md) [CommandText, propriété (](../../reference/ado-api/commandtext-property-ado.md) ADO) [Connection Object](../../reference/ado-api/connection-object-ado.md) (ADO) [ConnectionString, propriété (ADO)](../../reference/ado-api/connectionstring-property-ado.md) [Execute, méthode (ADO Command)](../../reference/ado-api/execute-method-ado-command.md) [Open, méthode (ADO Recordset)](../../reference/ado-api/open-method-ado-recordset.md) [Collections Parameters](../../reference/ado-api/parameters-collection-ado.md) (ADO) collection [Properties](../../reference/ado-api/properties-collection-ado.md) (ADO) [Provider, propriété (](../../reference/ado-api/provider-property-ado.md) ADO) [Recordset, objet Recordset (](../../reference/ado-api/recordset-object-ado.md) ADO) [prend en charge la méthode](../../reference/ado-api/supports-method.md)