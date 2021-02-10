---
description: Vue d’ensemble du fournisseur Microsoft OLE DB pour Microsoft Jet
title: Fournisseur Microsoft OLE DB pour Microsoft Jet | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Jet provider for OLE DB [ADO]
- providers [ADO], OLE DB provider for Microsoft Jet
- OLE DB provider for Microsoft Jet [ADO]
ms.assetid: fd956da1-5203-40af-aa7e-fc13a6c6581f
author: rothja
ms.author: jroth
ms.openlocfilehash: 87d9e159a9a97a0a929bec72d03118a927aad56e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029223"
---
# <a name="microsoft-ole-db-provider-for-microsoft-jet-overview"></a>Vue d’ensemble du fournisseur Microsoft OLE DB pour Microsoft Jet
Le fournisseur OLE DB pour Microsoft Jet permet à ADO d’accéder aux bases de données Microsoft Jet.

## <a name="connection-string-parameters"></a>Paramètres de chaîne de connexion
 Pour vous connecter à ce fournisseur, définissez l’argument *Provider* de la propriété [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) sur la propriété suivante :

```vb
Microsoft.Jet.OLEDB.4.0
```

 La lecture de la propriété [Provider](../../reference/ado-api/provider-property-ado.md) renverra également cette chaîne.

## <a name="typical-connection-string"></a>Chaîne de connexion classique
 Une chaîne de connexion classique pour ce fournisseur est la suivante :

```vb
"Provider=Microsoft.Jet.OLEDB.4.0;Data Source=databaseName;User ID=MyUserID;Password=MyPassword;"
```

 La chaîne se compose des mots clés suivants :

|Mot clé|Description|
|-------------|-----------------|
|**Fournisseur**|Spécifie le fournisseur de OLE DB pour Microsoft Jet.|
|**Source de données**|Spécifie le chemin d’accès et le nom de fichier de la base de données (par exemple, `c:\Northwind.mdb` ).|
|**ID d'utilisateur**|Spécifie le nom d'utilisateur. Si ce mot clé n’est pas spécifié, la chaîne, " `admin` ", est utilisée par défaut.|
|**Mot de passe**|Spécifie le mot de passe de l’utilisateur. Si ce mot clé n’est pas spécifié, la chaîne vide ("") est utilisée par défaut.|

> [!NOTE]
>  Si vous vous connectez à un fournisseur de sources de données qui prend en charge l’authentification Windows, vous devez spécifier **Trusted_Connection = Yes** ou **Integrated Security = SSPI** à la place des informations d’ID d’utilisateur et de mot de passe dans la chaîne de connexion.

## <a name="provider-specific-connection-parameters"></a>Provider-Specific les paramètres de connexion
 Le fournisseur de OLE DB pour Microsoft Jet prend en charge plusieurs propriétés dynamiques spécifiques au fournisseur, en plus de celles définies par ADO. Comme avec tous les autres paramètres de **connexion** , ils peuvent être définis à l’aide de la collection **Properties** de l’objet **Connection** ou dans le cadre de la chaîne de connexion.

 Le tableau suivant répertorie ces propriétés, ainsi que le nom de la propriété OLE DB correspondante entre parenthèses.

|Paramètre|Description|
|---------------|-----------------|
|Jet OLEDB : quantité d’espace récupéré compacte (DBPROP_JETOLEDB_COMPACTFREESPACESIZE)|Indique une estimation de la quantité d’espace, en octets, qui peut être récupérée en compactant la base de données. Cette valeur est valide uniquement après l’établissement d’une connexion de base de données.|
|Jet OLEDB : contrôle de connexion (DBPROP_JETOLEDB_CONNECTIONCONTROL)|Indique si les utilisateurs peuvent se connecter à la base de données.|
|Jet OLEDB : créer une base de données système (DBPROP_JETOLEDB_CREATESYSTEMDATABASE)|Indique si une base de données système doit être créée lors de la création d’une nouvelle source de données.|
|Jet OLEDB : mode de verrouillage de base de données (DBPROP_JETOLEDB_DATABASELOCKMODE)|Indique le mode de verrouillage pour cette base de données. Le premier utilisateur qui ouvre la base de données détermine le mode utilisé pendant que la base de données est ouverte.|
|Jet OLEDB : mot de passe de base de données (DBPROP_JETOLEDB_DATABASEPASSWORD)|Indique le mot de passe de la base de données.|
|Jet OLEDB : ne pas copier les paramètres régionaux sur le compact (DBPROP_JETOLEDB_COMPACT_DONTCOPYLOCALE)|Indique si Jet doit copier les informations de paramètres régionaux lors du compactage d’une base de données.|
|Jet OLEDB : chiffrement de base de données (DBPROP_JETOLEDB_ENCRYPTDATABASE)|Indique si une base de données compactée doit être chiffrée. Si cette propriété n’est pas définie, la base de données compactée est chiffrée si la base de données d’origine a également été chiffrée.|
|Jet OLEDB : type de moteur (DBPROP_JETOLEDB_ENGINE)|Indique le moteur de stockage utilisé pour accéder au magasin de données actuel.|
|Jet OLEDB : délai asynchrone exclusif (DBPROP_JETOLEDB_EXCLUSIVEASYNCDELAY)|Indique la durée maximale, en millisecondes, pendant laquelle jet peut retarder les écritures asynchrones sur le disque lorsque la base de données est ouverte en mode exclusif.<br /><br /> Cette propriété est ignorée sauf si **Jet OLEDB : Flush transaction Timeout** a la valeur 0.|
|Jet OLEDB : vider le délai d’expiration de la transaction (DBPROP_JETOLEDB_FLUSHTRANSACTIONTIMEOUT)|Indique le délai d’attente avant l’écriture des données stockées dans un cache pour l’écriture asynchrone sur le disque. Ce paramètre remplace les valeurs pour **Jet OLEDB : Shared Async Delay** et **Jet OLEDB : exclusive Async Delay**.|
|Jet OLEDB : transactions globales en bloc (DBPROP_JETOLEDB_GLOBALBULKNOTRANSACTIONS)|Indique si les transactions SQL en bloc sont traitées.|
|Jet OLEDB : opérations globales partielles en bloc (DBPROP_JETOLEDB_GLOBALBULKPARTIAL)|Indique le mot de passe utilisé pour ouvrir la base de données.|
|Jet OLEDB : validation de validation implicite (DBPROP_JETOLEDB_IMPLICITCOMMITSYNC)|Indique si les modifications apportées aux transactions implicites internes sont écrites en mode synchrone ou asynchrone.|
|Jet OLEDB : délai de verrouillage (DBPROP_JETOLEDB_LOCKDELAY)|Indique le nombre de millisecondes à attendre avant d’essayer d’acquérir un verrou après l’échec d’une tentative précédente.|
|Jet OLEDB : nouvelle tentative de verrouillage (DBPROP_JETOLEDB_LOCKRETRY)|Indique le nombre de fois qu’une tentative d’accès à une page verrouillée est répétée.|
|Jet OLEDB : taille maximale de la mémoire tampon (DBPROP_JETOLEDB_MAXBUFFERSIZE)|Indique la quantité de mémoire maximale, en kilo-octets, que Jet peut utiliser avant de commencer à vider les modifications sur le disque.|
|Jet OLEDB : nombre maximal de verrous par fichier (DBPROP_JETOLEDB_MAXLOCKSPERFILE)|Indique le nombre maximal de verrous que Jet peut placer sur une base de données. La valeur par défaut est 9500.|
|Jet OLEDB : nouveau mot de passe de base de données (DBPROP_JETOLEDB_NEWDATABASEPASSWORD)|Indique le nouveau mot de passe à définir pour cette base de données. L’ancien mot de passe est stocké dans **Jet OLEDB : Database Password**.|
|Jet OLEDB : délai d’expiration de la commande ODBC (DBPROP_JETOLEDB_ODBCCOMMANDTIMEOUT)|Indique le nombre de millisecondes avant l’expiration d’une requête ODBC distante à partir de jet.|
|Jet OLEDB : verrous de page vers verrou de table (DBPROP_JETOLEDB_PAGELOCKSTOTABLELOCK)|Indique combien de pages doivent être verrouillées dans une transaction avant que Jet tente de promouvoir le verrou sur un verrou de table. Si cette valeur est égale à 0, le verrou n’est jamais promu.|
|Jet OLEDB : page Timeout (DBPROP_JETOLEDB_PAGETIMEOUT)|Indique le nombre de millisecondes pendant lesquelles jet attendra avant de vérifier si son cache est obsolète par le fichier de base de données.|
|Jet OLEDB : recycler les pages de Long-Valued (DBPROP_JETOLEDB_RECYCLELONGVALUEPAGES)|Indique si Jet doit tenter de récupérer des pages BLOB de façon agressive lorsqu’elles sont libérées.|
|Jet OLEDB : chemin d’accès au registre (DBPROP_JETOLEDB_REGPATH)|Indique la clé de Registre Windows qui contient des valeurs pour le moteur de base de données Jet.|
|Jet OLEDB : Reset ISAM stats (DBPROP_JETOLEDB_RESETISAMSTATS)|Indique si le DBSCHEMA_JETOLEDB_ISAMSTATS du **Recordset** de schéma doit réinitialiser ses compteurs de performance après avoir retourné des informations de performances.|
|Jet OLEDB : délai asynchrone partagé (DBPROP_JETOLEDB_SHAREDASYNCDELAY)|Indique la durée maximale, en millisecondes, que Jet peut retarder les écritures asynchrones sur le disque lorsque la base de données est ouverte en mode multi-utilisateur.|
|Jet OLEDB : base de données système (DBPROP_JETOLEDB_SYSDBPATH)|Indique le chemin d’accès et le nom de fichier du fichier d’informations de groupe de travail (base de données système).|
|Jet OLEDB : mode de validation de transaction (DBPROP_JETOLEDB_TXNCOMMITMODE)|Indique si Jet écrit des données sur le disque de façon synchrone ou asynchrone lorsqu’une transaction est validée.|
|Jet OLEDB : synchronisation de validation de l’utilisateur (DBPROP_JETOLEDB_USERCOMMITSYNC)|Indique si les modifications apportées aux transactions sont écrites en mode synchrone ou asynchrone.|

## <a name="provider-specific-recordset-and-command-properties"></a>Propriétés de commande et Recordset Provider-Specific
 Le fournisseur Jet prend également en charge plusieurs propriétés de **commande** et **Recordset** spécifiques au fournisseur. Ces propriétés sont accessibles et définies via la collection **Properties** de l’objet **Recordset** ou **Command** . Le tableau répertorie le nom de la propriété ADO et le nom de la propriété OLE DB correspondante entre parenthèses.

|Nom de la propriété|Description|
|-------------------|-----------------|
|Jet OLEDB : transactions en bloc (DBPROP_JETOLEDB_BULKNOTRANSACTIONS)|Indique si les opérations SQL en bloc sont traitées. Les opérations en bloc volumineuses peuvent échouer lors de la transaction, en raison de retards de ressources.|
|Jet OLEDB : activer les curseurs FAT (DBPROP_JETOLEDB_ENABLEFATCURSOR)|Indique si Jet doit mettre en cache plusieurs lignes lors du remplissage d’un jeu d’enregistrements pour les sources de ligne distantes.|
|Jet OLEDB : taille du cache de curseurs FAT (DBPROP_JETOLEDB_FATCURSORMAXROWS)|Indique le nombre de lignes à mettre en cache lors de l’utilisation de la mise en cache des lignes du magasin de données distant. Cette valeur est ignorée, sauf si **Jet OLEDB : activer les curseurs Fat** a la valeur true.|
|Jet OLEDB : incohérent (DBPROP_JETOLEDB_INCONSISTENT)|Indique si les résultats de la requête autorisent des mises à jour incohérentes.|
|Jet OLEDB : granularité de verrouillage (DBPROP_JETOLEDB_LOCKGRANULARITY)|Indique si une table est ouverte à l’aide du verrouillage au niveau des lignes.|
|Jet OLEDB : ODBC Pass-Through, instruction (DBPROP_JETOLEDB_ODBCPASSTHROUGH)|Indique que Jet doit transmettre le texte SQL dans un objet de **commande** au back end non modifié.|
|Jet OLEDB : opérations en bloc partielles (DBPROP_JETOLEDB_BULKPARTIAL)|Indique le comportement de jet lorsque les opérations DML SQL échouent.|
|Jet OLEDB : Bulk-Op de requête directe (DBPROP_JETOLEDB_PASSTHROUGHBULKOP)|Indique si les requêtes qui ne retournent pas de **Recordset** sont transmises sans modification à la source de données.|
|Jet OLEDB : chaîne de connexion de la requête directe (DBPROP_JETOLEDB_ODBCPASSTHROUGHCONNECTSTRING)|Indique la chaîne de connexion Jet utilisée pour se connecter à un magasin de données distant. Cette valeur est ignorée, sauf si l' **instruction Jet OLEDB : ODBC Pass-Through** a la valeur true.|
|Jet OLEDB : requête stockée (DBPROP_JETOLEDB_STOREDQUERY)|Indique si le texte de la commande doit être interprété comme une requête stockée au lieu d’une commande SQL.|
|Jet OLEDB : valider les règles sur le jeu (DBPROP_JETOLEDB_VALIDATEONSET)|Indique si les règles de validation jet sont évaluées lorsque les données de la colonne sont définies ou lorsque les modifications sont validées dans la base de données.|

 Par défaut, le fournisseur de OLE DB pour Microsoft Jet ouvre les bases de données Microsoft Jet en mode lecture/écriture. Pour ouvrir une base de données en mode lecture seule, définissez la propriété [mode](../../reference/ado-api/mode-property-ado.md) de l’objet de **connexion** ADO sur **adModeRead**.

## <a name="command-object-usage"></a>Utilisation de l’objet de commande
 Le texte de la commande dans l’objet [Command](../../reference/ado-api/command-object-ado.md) utilise le dialecte SQL Microsoft Jet. Vous pouvez spécifier des requêtes qui retournent des lignes, des requêtes d’action et des noms de tables dans le texte de la commande. Toutefois, les procédures stockées ne sont pas prises en charge et ne doivent pas être spécifiées.

## <a name="recordset-behavior"></a>Comportement du Recordset
 Le moteur de base de données Microsoft Jet ne prend pas en charge les curseurs dynamiques. Par conséquent, le fournisseur de OLE DB pour Microsoft Jet ne prend pas en charge le type de curseur **adLockDynamic** . Lorsqu’un curseur dynamique est demandé, le fournisseur retourne un curseur de jeu de clés et réinitialise la propriété [CursorType](../../reference/ado-api/cursortype-property-ado.md) pour indiquer le type de [jeu d’enregistrements](../../reference/ado-api/recordset-object-ado.md) retourné. En outre, si un **jeu d’enregistrements** pouvant être mis à jour est demandé (**LockType** est **adLockOptimistic**, **adLockBatchOptimistic** ou **adLockPessimistic**), le fournisseur retourne également un curseur de jeu de clés et réinitialise la propriété **CursorType** .

## <a name="dynamic-properties"></a>Propriétés dynamiques
 Le fournisseur de OLE DB pour Microsoft Jet insère plusieurs propriétés dynamiques dans la collection **Properties** des objets [Connection](../../reference/ado-api/connection-object-ado.md), [Recordset](../../reference/ado-api/recordset-object-ado.md)et [Command](../../reference/ado-api/command-object-ado.md) inouverts.

 Les tableaux suivants sont un index croisé des noms ADO et OLE DB pour chaque propriété dynamique. Le Guide de référence du programmeur OLE DB fait référence à un nom de propriété ADO par le terme « Description ». Vous trouverez plus d’informations sur ces propriétés dans le Guide de référence du programmeur OLE DB.

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
|Catalogue actuel|DBPROP_CURRENTCATALOG|
|source de données|DBPROP_INIT_DATASOURCE|
|Nom de la source de données|DBPROP_DATASOURCENAME|
|Modèle de thread de l’objet de source de données|DBPROP_DSOTHREADMODEL|
|Nom SGBD|DBPROP_DBMSNAME|
|Version de SGBD|DBPROP_DBMSVER|
|Prise en charge de GROUP BY|DBPROP_GROUPBY|
|Prise en charge des tables hétérogènes|DBPROP_HETEROGENEOUSTABLES|
|Respect de la casse des identificateurs|DBPROP_IDENTIFIERCASE|
|Niveaux d’isolation|DBPROP_SUPPORTEDTXNISOLEVELS|
|Rétention de l’isolation|DBPROP_SUPPORTEDTXNISORETAIN|
|Identificateur de paramètres régionaux|DBPROP_INIT_LCID|
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
|Version de OLE DB|DBPROP_PROVIDEROLEDBVER|
|Prise en charge des objets OLE|DBPROP_OLEOBJECTS|
|Prise en charge des ensembles de lignes ouverts|DBPROP_OPENROWSETSUPPORT|
|CLASSER par colonnes dans la liste de sélection|DBPROP_ORDERBYCOLUMNSINSELECT|
|Disponibilité des paramètres de sortie|DBPROP_OUTPUTPARAMETERAVAILABILITY|
|Passer par les accesseurs de référence|DBPROP_BYREFACCESSORS|
|Mot de passe|DBPROP_AUTH_PASSWORD|
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
|Ensemble de lignes Append-Only|DBPROP_APPENDONLY|
|Blocage des objets de stockage|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Type de signet|DBPROP_BOOKMARKTYPE|
|Avec signet|DBPROP_IROWSETLOCATE|
|Signets triés|DBPROP_ORDEREDBOOKMARKS|
|Mettre en cache les colonnes différées|DBPROP_CACHEDEFERRED|
|Modifier les lignes insérées|DBPROP_CHANGEINSERTEDROWS|
|Privilèges de colonne|DBPROP_COLUMNRESTRICT|
|Notification du jeu de colonnes|DBPROP_NOTIFYCOLUMNSET|
|Colonne accessible en écriture|DBPROP_MAYWRITECOLUMN|
|Différer la colonne|DBPROP_DEFERRED|
|Retarder les mises à jour des objets de stockage|DBPROP_DELAYSTORAGEOBJECTS|
|Extraire vers l’arrière|DBPROP_CANFETCHBACKWARDS|
|Conserver les lignes|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|Interfaces|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|ILockBytes|DBPROP_ILockBytes|
|Lignes immobiles|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IRowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetIndex|DBPROP_IRowsetIndex|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IRowsetLocate|DBPROP_IRowsestLocate|
|IRowsetResynch||
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate.|
|ISequentialStream|DBPROP_ISequentialStream|
|IStorage|DBPROP_IStorage|
|IStream|DBPROP_IStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|Signets littéraux|DBPROP_LITERALBOOKMARKS|
|Identité de ligne littérale|DBPROP_LITERALIDENTITY|
|Nombre maximal de lignes ouvertes|DBPROP_MAXOPENROWS|
|Nombre maximal de lignes en attente|DBPROP_MAXPENDINGROWS|
|Lignes au maximum|DBPROP_MAXROWS|
|Utilisation de la mémoire|DBPROP_MEMORYUSAGE|
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
|Ignorer les signets supprimés|DBPROP_BOOKMARKSKIPPED|
|Identité de ligne forte|DBPROP_STRONGITDENTITY|
|Mise à jour|DBPROP_UPDATABILITY|
|Utiliser des signets|DBPROP_BOOKMARKS|

## <a name="command-dynamic-properties"></a>Propriétés dynamiques de commande
 Les propriétés suivantes sont ajoutées à la collection **Properties** de l’objet **Command** .

|Nom de la propriété ADO|Nom de la propriété OLE DB|
|-----------------------|--------------------------|
|Ordre d’accès|DBPROP_ACCESSORDER|
|Ensemble de lignes Append-Only|DBPROP_APPENDONLY|
|Blocage des objets de stockage|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Type de signet|DBPROP_BOOKMARKTYPE|
|Avec signet|DBPROP_IROWSETLOCATE|
|Modifier les lignes insérées|DBPROP_CHANGEINSERTEDROWS|
|Privilèges de colonne|DBPROP_COLUMNRESTRICT|
|Notification du jeu de colonnes|DBPROP_NOTIFYCOLUMNSET|
|Différer la colonne|DBPROP_DEFERRED|
|Retarder les mises à jour des objets de stockage|DBPROP_DELAYSTORAGEOBJECTS|
|Extraire vers l’arrière|DBPROP_CANFETCHBACKWARDS|
|Conserver les lignes|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|Interfaces|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|ILockBytes|DBPROP_ILockBytes|
|Lignes immobiles|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IRowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetIndex|DBPROP_IRowsetIndex|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IRowsetLocate|DBPROP_IRowsetLocate|
|IRowsetResynch||
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate.|
|ISequentialStream|DBPROP_ISequentialStream|
|IStorage|DBPROP_IStorage|
|IStream|DBPROP_IStream|
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
|Données du serveur lors de l’insertion|DBPROP_SERVERDATAONINSERT|
|Ignorer les signets supprimés|DBPROP_BOOKMARKSKIP|
|Identité de ligne forte|DBPROP_STRONGIDENTITY|
|Mise à jour|DBPROP_UPDATABILITY|
|Utiliser des signets|DBPROP_BOOKMARKS|

 Pour obtenir des détails d’implémentation spécifiques et des informations fonctionnelles sur le fournisseur de OLE DB pour Microsoft Jet, consultez [fournisseur Jet](/previous-versions/windows/desktop/ms722791(v=vs.85)) dans la documentation OLE DB.
