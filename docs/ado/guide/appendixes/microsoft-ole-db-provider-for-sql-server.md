---
description: Vue d’ensemble du fournisseur Microsoft OLE DB pour SQL Server
title: Fournisseur Microsoft OLE DB pour SQL Server | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], OLE DB provider for SQL Server
- OLE DB provider for SQL Server [ADO]
- SQLOLEDB [ADO]
ms.assetid: 99bc40c4-9181-4ca1-a06f-9a1a914a0b7b
author: rothja
ms.author: jroth
ms.openlocfilehash: 1b6a62e8946415cc9c1e869a173de43b98ba14f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029253"
---
# <a name="microsoft-ole-db-provider-for-sql-server-overview"></a>Vue d’ensemble du fournisseur Microsoft OLE DB pour SQL Server
Le fournisseur Microsoft OLE DB pour SQL Server, SQLOLEDB, permet à ADO d’accéder à Microsoft SQL Server.

> [!IMPORTANT]
> Le fournisseur Microsoft OLE DB pour SQL Server (SQLOLEDB) reste déconseillé et il n’est pas recommandé de l’utiliser pour un nouveau travail de développement. Au lieu de cela, utilisez le nouveau [Microsoft OLE DB Driver pour SQL Server](../../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL), qui sera mis à jour avec les fonctionnalités serveur les plus récentes.

## <a name="connection-string-parameters"></a>Paramètres de chaîne de connexion
 Pour vous connecter à ce fournisseur, affectez à l’argument *Provider* la valeur de la propriété [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) :

```vb
SQLOLEDB
```

 Cette valeur peut également être définie ou lue à l’aide de la propriété [Provider](../../reference/ado-api/provider-property-ado.md) .

## <a name="typical-connection-string"></a>Chaîne de connexion classique
 Une chaîne de connexion classique pour ce fournisseur est la suivante :

```vb
"Provider=SQLOLEDB;Data Source=serverName;"
Initial Catalog=databaseName;
User ID=MyUserID;Password=MyPassword;"
```

 La chaîne se compose des mots clés suivants :

|Mot clé|Description|
|-------------|-----------------|
|**Fournisseur**|Spécifie le fournisseur de OLE DB pour SQL Server.|
|**Source de données** ou **serveur**|Spécifie le nom d'un serveur.|
|**Catalogue** ou **base de données** initial|Spécifie le nom d’une base de données sur le serveur.|
|**ID d’utilisateur** ou **UID**|Spécifie le nom d’utilisateur (pour l’authentification SQL Server).|
|**Mot de passe** ou **PWD**|Spécifie le mot de passe de l’utilisateur (pour l’authentification SQL Server).|

> [!NOTE]
>  Si vous vous connectez à un fournisseur de sources de données qui prend en charge l’authentification Windows, vous devez spécifier **Trusted_Connection = Yes** ou **Integrated Security = SSPI** à la place des informations d’ID d’utilisateur et de mot de passe dans la chaîne de connexion.

## <a name="provider-specific-connection-parameters"></a>Provider-Specific les paramètres de connexion
 Le fournisseur prend en charge plusieurs paramètres de connexion spécifiques au fournisseur, en plus de ceux définis par ADO. Comme avec les propriétés de connexion ADO, ces propriétés spécifiques au fournisseur peuvent être définies via la collection [Properties](../../reference/ado-api/properties-collection-ado.md) d’une [connexion](../../reference/ado-api/connection-object-ado.md) ou peuvent être définies dans le cadre de **ConnectionString**.

|Paramètre|Description|
|---------------|-----------------|
|Trusted_Connection|Indique le mode d’authentification de l’utilisateur. Cette valeur peut être définie sur **Oui** ou **non**. La valeur par défaut est **Non**. Si cette propriété est définie sur **Oui**, SQLOLEDB utilise le mode d’authentification Microsoft Windows NT pour autoriser l’accès utilisateur à la base de données SQL Server spécifiée par les valeurs de propriété **emplacement** et [DataSource](../../reference/ado-api/datasource-property-ado.md) . Si cette propriété a la valeur **non**, SQLOLEDB utilise le mode mixte pour autoriser l’accès utilisateur à la base de données SQL Server. La connexion et le mot de passe SQL Server sont spécifiés dans les propriétés **ID d’utilisateur** et **mot de passe** .|
|Current Language|Indique un nom de langage SQL Server. Identifie la langue utilisée pour le choix et la mise en forme des messages système. La langue doit être installée sur le SQL Server. sinon, l’ouverture de la connexion échoue.|
|Adresse réseau|Indique l’adresse réseau du SQL Server spécifié par la propriété **location** .|
|Network Library|Indique le nom de la bibliothèque réseau (DLL) utilisée pour communiquer avec le SQL Server. Le nom ne doit pas inclure le chemin d'accès ni l'extension de nom de fichier .dll. La valeur par défaut est fournie par la configuration du client SQL Server.|
|Utiliser la procédure de préparation|Détermine si SQL Server crée des procédures stockées temporaires lors de la préparation des commandes (par la propriété **Prepared** ).|
|Traduire automatiquement|Indique si les caractères OEM/ANSI sont convertis. Cette propriété peut avoir la valeur **true** ou **false**. La valeur par défaut est **True**. Si cette propriété est définie sur **true**, SQLOLEDB effectue une conversion de caractères OEM/ANSI lorsque les chaînes de caractères multioctets sont récupérées à partir de la SQL Server ou envoyées à celle-ci. Si cette propriété a la valeur **false**, SQLOLEDB n’effectue pas de conversion de caractères OEM/ANSI sur les données de chaînes de caractères multioctets.|
|Packet Size|Indique une taille de paquet réseau en octets. La valeur de la propriété taille du paquet doit être comprise entre 512 et 32767. La taille du paquet réseau SQLOLEDB par défaut est 4096.|
|Nom de l’application|Indique le nom de l’application cliente.|
|ID Station de travail|Chaîne identifiant la station de travail.|

## <a name="command-object-usage"></a>Utilisation de l’objet de commande
 SQLOLEDB accepte un amalgame de syntaxe Transact-SQL spécifique à ODBC, ANSI et SQL Server. Par exemple, l'instruction SQL suivante utilise une séquence d'échappement ODBC SQL pour spécifier la fonction de chaîne LCASE :

```sql
SELECT customerid={fn LCASE(CustomerID)} FROM Customers

```

 LCASE retourne une chaîne de caractères, en convertissant toutes les majuscules en leurs équivalents minuscules. La fonction de chaîne SQL ANSI LOWER effectue la même opération. par conséquent, l’instruction SQL suivante est un équivalent ANSI de l’instruction ODBC présentée précédemment :

```sql
SELECT customerid=LOWER(CustomerID) FROM Customers

```

 SQLOLEDB traite correctement l’une ou l’autre des formes de l’instruction lorsqu’elle est spécifiée sous forme de texte pour une commande.

## <a name="stored-procedures"></a>Procédures stockées
 Lors de l’exécution d’une procédure stockée SQL Server à l’aide d’une commande SQLOLEDB, utilisez la séquence d’échappement d’appel de procédure ODBC dans le texte de la commande. SQLOLEDB utilise ensuite le mécanisme d’appel de procédure distante de SQL Server pour optimiser le traitement des commandes. Par exemple, l’instruction SQL ODBC suivante est le texte de la commande par défaut sur le formulaire Transact-SQL :

## <a name="odbc-sql"></a>ODBC SQL

```vb
{call SalesByCategory('Produce', '1995')}

```

## <a name="transact-sql"></a>Transact-SQL

```sql
EXECUTE SalesByCategory 'Produce', '1995'

```

## <a name="sql-server-features"></a>Fonctionnalités de SQL Server
 Avec SQL Server, ADO peut utiliser XML pour l’entrée de **commande** et récupérer les résultats dans un format de flux XML plutôt que dans des objets **Recordset** . Pour plus d’informations, consultez [utilisation de flux pour l’entrée de commande](../data/command-streams.md) et extraction de [jeux de résultats dans des flux](../data/retrieving-resultsets-into-streams.md).

### <a name="accessing-sql_variant-data-using-mdac-27-mdac-28-or-windows-dac-60"></a>Accès aux données sql_variant à l’aide de MDAC 2,7, MDAC 2,8 ou Windows DAC 6,0
 Microsoft SQL Server a un type de données nommé **sql_variant**. Semblable au **DBTYPE_VARIANT** de OLE DB, le type de données **sql_variant** peut stocker des données de différents types. Toutefois, il existe quelques différences clés entre **DBTYPE_VARIANT** et **sql_variant**. ADO gère également les données stockées sous la forme d’une valeur **sql_variant** différente de celle qui gère les autres types de données. La liste suivante décrit les problèmes à prendre en compte lorsque vous accédez à SQL Server données stockées dans des colonnes de type **sql_variant**.

-   Dans les composants MDAC 2,7, MDAC 2,8 et Windows DAC (Windows Data Access Components) 6,0, le fournisseur OLE DB pour SQL Server prend en charge le type de **sql_variant** . Le fournisseur OLE DB pour ODBC ne le fait pas.

-   Le type de **sql_variant** ne correspond pas exactement au type de données **DBTYPE_VARIANT** .  Le type de **sql_variant** prend en charge quelques nouveaux sous-types non pris en charge par **DBTYPE_VARIANT,** y compris les chaînes **GUID**, **ANSI** (non-Unicode) et **bigint**. L’utilisation de sous-types autres que ceux répertoriés précédemment fonctionnera correctement.

-   Le **sql_variant** sous-type **Numeric** ne correspond pas à la taille de **DBTYPE_DECIMAL** .

-   Plusieurs contraintes de type de données entraînent des types qui ne correspondent pas. Par exemple, la conversion d’un **sql_variant** avec un sous-type de **GUID** en **DBTYPE_VARIANT** entraînera un sous-type de **SAFEARRAY**(octets). Si vous reconvertissez ce type en **sql_variant** , vous obtiendrez un nouveau sous-type de **tableau**(octets).

-   Les champs de **Recordset** qui contiennent des **sql_variant** données peuvent être distants (marshalés) ou persistants uniquement si le **sql_variant** contient des sous-types spécifiques. Toute tentative d’accès à distance ou de persistance des données avec les sous-types non pris en charge suivants provoque une erreur au moment de l’exécution (conversion non prise en charge) à partir du fournisseur de persistance Microsoft (MSPersist) : **VT_VARIANT**, **VT_RECORD**, **VT_ILLEGAL**, **VT_UNKNOWN**, **VT_BSTR** et **VT_DISPATCH.**

-   Le fournisseur OLE DB pour SQL Server dans MDAC 2,7, MDAC 2,8 et Windows DAC 6,0 possède une propriété dynamique nommée **allow Native variant** qui, comme son nom l’indique, permet aux développeurs d’accéder à la **sql_variant** dans sa forme native, par opposition à un **DBTYPE_VARIANT**. Si cette propriété est définie et qu’un **jeu d’enregistrements** est ouvert avec le moteur de curseur client (**adUseClient**), l’appel **Recordset. Open** échoue. Si cette propriété est définie et qu’un **jeu d’enregistrements** est ouvert avec des curseurs de serveur (**adUseServer**), l’appel **Recordset. Open** aboutira, mais l’accès aux colonnes de type **sql_variant** produira une erreur.

-   Dans les applications clientes qui utilisent MDAC 2,5, **sql_variant** données peuvent être utilisées avec des requêtes sur Microsoft SQL Server. Toutefois, les valeurs des données **sql_variant** sont traitées comme des chaînes. Ces applications clientes doivent être mises à niveau vers MDAC 2,7, MDAC 2,8 ou Windows DAC 6,0.

## <a name="recordset-behavior"></a>Comportement du Recordset
 SQLOLEDB ne peut pas utiliser SQL Server curseurs pour prendre en charge le résultat multiple généré par de nombreuses commandes. Si un consommateur demande un jeu d’enregistrements nécessitant une prise en charge de curseur SQL Server, une erreur se produit si le texte de commande utilisé génère plus d’un jeu d’enregistrements unique comme résultat.

 Les jeux d’enregistrements SQLOLEDB à défilement sont pris en charge par les curseurs de SQL Server. SQL Server impose des limitations sur les curseurs sensibles aux modifications apportées par d’autres utilisateurs de la base de données. Plus précisément, les lignes de certains curseurs ne peuvent pas être triées et toute tentative de création d’un jeu d’enregistrements à l’aide d’une commande contenant une clause SQL ORDER BY peut échouer.

## <a name="dynamic-properties"></a>Propriétés dynamiques
 Le fournisseur Microsoft OLE DB pour SQL Server insère plusieurs propriétés dynamiques dans la collection **Properties** des objets [Connection](../../reference/ado-api/connection-object-ado.md), [Recordset](../../reference/ado-api/recordset-object-ado.md)et [Command](../../reference/ado-api/command-object-ado.md) inouverts.

 Les tableaux suivants sont un index croisé des noms ADO et OLE DB pour chaque propriété dynamique. Le Guide de référence du programmeur OLE DB fait référence à un nom de propriété ADO par le terme « Description ». Vous trouverez plus d’informations sur ces propriétés dans le Guide de référence du programmeur OLE DB. Recherchez le nom de la propriété OLE DB dans l’index ou consultez l' [annexe C : propriétés du OLE DB](/previous-versions/windows/desktop/ms723130(v=vs.85)).

## <a name="connection-dynamic-properties"></a>Propriétés dynamiques de la connexion
 Les propriétés suivantes sont ajoutées à la collection **Properties** de l’objet **Connection** .

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
|Taille maximale de l’index|DBPROP_MAXINDEXSIZE|
|Taille de ligne maximale|DBPROP_MAXROWSIZE|
|La taille de ligne maximale comprend l’objet BLOB|DBPROP_MAXROWSIZEINCLUDESBLOB|
|Nombre maximal de tables dans SELECT|DBPROP_MAXTABLESINSELECT|
|Jeux de paramètres multiples|DBPROP_MULTIPLEPARAMSETS|
|Résultats multiples|DBPROP_MULTIPLERESULTS|
|Objets de stockage multiples|DBPROP_MULTIPLESTORAGEOBJECTS|
|Mise à jour de plusieurs tables|DBPROP_MULTITABLEUPDATE|
|Ordre de classement NULL|DBPROP_NULLCOLLATION|
|Comportement de concaténation NULL|DBPROP_CONCATNULLBEHAVIOR|
|Version de OLE DB|DBPROP_PROVIDEROLEDBVER|
|Prise en charge des objets OLE|DBPROP_OLEOBJECTS|
|Prise en charge des ensembles de lignes ouverts|DBPROP_OPENROWSETSUPPORT|
|CLASSER par colonnes dans la liste de sélection|DBPROP_ORDERBYCOLUMNSINSELECT|
|Disponibilité des paramètres de sortie|DBPROP_OUTPUTPARAMETERAVAILABILITY|
|Passer par les accesseurs de référence|DBPROP_BYREFACCESSORS|
|Mot de passe|DBPROP_AUTH_PASSWORD|
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
|Délai d’attente de la commande|DBPROP_COMMANDTIMEOUT|
|Différer la colonne|DBPROP_DEFERRED|
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
|IRowsetLocate|DBPROP_IRowsestLocate|
|IRowsetResynch||
|IRowsetScroll|DBPROP_IRowsetScroll|
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
|Modifications visibles par d’autres utilisateurs|DBPROP_OTHERUPDATEDELETE|
|Autres insertions visibles|DBPROP_OTHERINSERT|
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
|Curseur côté serveur|DBPROP_SERVERCURSOR|
|Ignorer les signets supprimés|DBPROP_BOOKMARKSKIPPED|
|Identité de ligne forte|DBPROP_STRONGITDENTITY|
|Lignes uniques|DBPROP_UNIQUEROWS|
|Mise à jour|DBPROP_UPDATABILITY|
|Utiliser des signets|DBPROP_BOOKMARKS|

## <a name="command-dynamic-properties"></a>Propriétés dynamiques de commande
 Les propriétés suivantes sont ajoutées à la collection **Properties** de l’objet **Command** .

|Nom de la propriété ADO|Nom de la propriété OLE DB|
|-----------------------|--------------------------|
|Ordre d’accès|DBPROP_ACCESSORDER|
|Chemin de base|SSPROP_STREAM_BASEPATH|
|Blocage des objets de stockage|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Type de signet|DBPROP_BOOKMARKTYPE|
|Avec signet|DBPROP_IROWSETLOCATE|
|Modifier les lignes insérées|DBPROP_CHANGEINSERTEDROWS|
|Privilèges de colonne|DBPROP_COLUMNRESTRICT|
|Notification du jeu de colonnes|DBPROP_NOTIFYCOLUMNSET|
|Type de contenu|SSPROP_STREAM_CONTENTTYPE|
|Récupération automatique de curseur|SSPROP_CURSORAUTOFETCH|
|Différer la colonne|DBPROP_DEFERRED|
|Préparation différée|SSPROP_DEFERPREPARE|
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
|IRowsetResynch|DBPROP_IRowsetResynch|
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate.|
|ISequentialStream|DBPROP_ISequentialStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|Signets littéraux|DBPROP_LITERALBOOKMARKS|
|Identité de ligne littérale|DBPROP_LITERALIDENTITY|
|Mode de verrouillage|DBPROP_LOCKMODE|
|Nombre maximal de lignes ouvertes|DBPROP_MAXOPENROWS|
|Nombre maximal de lignes en attente|DBPROP_MAXPENDINGROWS|
|Lignes au maximum|DBPROP_MAXROWS|
|Granularité des notifications|DBPROP_NOTIFICATIONGRANULARITY|
|Phases de notification|DBPROP_NOTIFICATIONPHASES|
|Objets traités|DBPROP_TRANSACTEDOBJECT|
|Modifications visibles par d’autres utilisateurs|DBPROP_OTHERUPDATEDELETE|
|Autres insertions visibles|DBPROP_OTHERINSERT|
|Propriété Encoding de sortie|DBPROP_OUTPUTENCODING|
|Propriété de flux de sortie|DBPROP_OUTPUTSTREAM|
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
|Curseur côté serveur|DBPROP_SERVERCURSOR|
|Données du serveur lors de l’insertion|DBPROP_SERVERDATAONINSERT|
|Ignorer les signets supprimés|DBPROP_BOOKMARKSKIP|
|Identité de ligne forte|DBPROP_STRONGIDENTITY|
|Mise à jour|DBPROP_UPDATABILITY|
|Utiliser des signets|DBPROP_BOOKMARKS|
|Racine XML|SSPROP_STREAM_XMLROOT|
|XSL|SSPROP_STREAM_XSL|

 Pour obtenir des détails d’implémentation spécifiques et des informations fonctionnelles sur le fournisseur de OLE DB Microsoft SQL Server, consultez le [fournisseur SQL Server](/previous-versions/windows/desktop/ms720897(v=vs.85)).

## <a name="see-also"></a>Voir aussi
 [ConnectionString, propriété (ADO)](../../reference/ado-api/connectionstring-property-ado.md) [Provider, propriété](../../reference/ado-api/provider-property-ado.md) (ADO), [objet Recordset (](../../reference/ado-api/recordset-object-ado.md) ADO)