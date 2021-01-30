---
description: Index des propriétés dynamiques ADO
title: Index de propriétés dynamiques ADO | Microsoft Docs
ms.prod: sql
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- dynamic properties [ADO], index
ms.assetid: 80d389dd-46ef-459f-b0d4-6f712fc4f32d
author: rothja
ms.author: jroth
ms.openlocfilehash: 4d3d765cbeba49a5c34bbc5529d833cefc5bb1ab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167906"
---
# <a name="ado-dynamic-property-index"></a>Index des propriétés dynamiques ADO
Les fournisseurs de données, les fournisseurs de services et les composants de service peuvent ajouter des propriétés dynamiques aux collections de **Propriétés** de la [connexion](./connection-object-ado.md) et des objets [Recordset](./recordset-object-ado.md) inouverts. Un fournisseur donné peut également insérer des propriétés supplémentaires quand ces objets sont ouverts. Certaines de ces propriétés sont répertoriées dans la section [propriétés dynamiques ADO](./ado-dynamic-properties.md) . D’autres sont répertoriés sous les fournisseurs spécifiques dans la section [annexe A : fournisseurs](../../guide/appendixes/appendix-a-providers.md) .  
  
 Les tableaux suivants sont des index croisés des noms ADO et OLE DB pour chaque propriété dynamique du fournisseur OLE DB standard. Vos fournisseurs peuvent ajouter d’autres propriétés que celles répertoriées ici. Pour obtenir des informations spécifiques sur les propriétés dynamiques spécifiques au fournisseur, consultez la documentation de votre fournisseur.  
  
 Le Guide de référence du programmeur OLE DB fait référence à un nom de propriété ADO par le terme « Description ». Pour plus d’informations sur ces propriétés standard, recherchez ou parcourez l’index dans la [documentation OLE DB](/previous-versions/windows/desktop/ms722784(v=vs.85))pour la propriété OLE DB par son nom.  
  
## <a name="connection-dynamic-properties"></a>Propriétés dynamiques de la connexion  
  
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
|Passer par les accesseurs de référence|DBPROP_BYREFACCESSORS|  
|Mot de passe|DBPROP_AUTH_PASSWORD|  
|Persist Security Info|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|  
|Type d’ID persistant|DBPROP_PERSISTENTIDTYPE|  
|Comportement de préparation d’abandon|DBPROP_PREPAREABORTBEHAVIOR|  
|Préparer le comportement de validation|DBPROP_PREPARECOMMITBEHAVIOR|  
|Terme de procédure|DBPROP_PROCEDURETERM|  
|Invite|DBPROP_INIT_PROMPT|  
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
 Notez que les **propriétés dynamiques** de l’objet **Recordset** sont hors de portée (deviennent indisponibles) lorsque le **Recordset** est fermé.  
  
|Nom de la propriété ADO|Nom de la propriété OLE DB|  
|-----------------------|--------------------------|  
|IAccessor|DBPROP_IACCESSOR|  
|IChapteredRowset||  
|IColumnsInfo|DBPROP_ICOLUMNSINFO|  
|IColumnsRowset|DBPROP_ICOLUMNSROWSET|  
|Interfaces|DBPROP_ICONNECTIONPOINTCONTAINER|  
|IConvertType||  
|ILockBytes|DBPROP_ILOCKBYTES|  
|IRowset|DBPROP_IROWSET|  
|IDBAsynchStatus|DBPROP_IDBASYNCHSTATUS|  
|IParentRowset||  
|IRowsetChange|DBPROP_IROWSETCHANGE|  
|IRowsetExactScroll||  
|IRowsetFind|DBPROP_IROWSETFIND|  
|IRowsetIdentity|DBPROP_IROWSETIDENTITY|  
|IRowsetInfo|DBPROP_IROWSETINFO|  
|IRowsetLocate|DBPROP_IROWSETLOCATE|  
|IRowsetRefresh|DBPROP_IROWSETREFRESH|  
|IRowsetResynch||  
|IRowsetScroll|DBPROP_IROWSETSCROLL|  
|IRowsetUpdate|DBPROP_IROWSETUPDATE|  
|IRowsetView|DBPROP_IROWSETVIEW|  
|IRowsetIndex|DBPROP_IROWSETINDEX|  
|ISequentialStream|DBPROP_ISEQUENTIALSTREAM|  
|IStorage|DBPROP_ISTORAGE|  
|IStream|DBPROP_ISTREAM|  
|ISupportErrorInfo|DBPROP_ISUPPORTERRORINFO|  
|Ordre d’accès|DBPROP_ACCESSORDER|  
|Ensemble de lignes Append-Only|DBPROP_APPENDONLY|  
|Traitement de l’ensemble de lignes asynchrone|DBPROP_ROWSET_ASYNCH|  
|Recalcul automatique|DBPROP_ADC_AUTORECALC|  
|Taille de l’extraction en arrière-plan|DBPROP_ASYNCHFETCHSIZE|  
|Priorité de thread en arrière-plan|DBPROP_ASYNCHTHREADPRIORITY|  
|Taille du lot|DBPROP_ADC_BATCHSIZE|  
|Blocage des objets de stockage|DBPROP_BLOCKINGSTORAGEOBJECTS|  
|Type de signet|DBPROP_BOOKMARKTYPE|  
|Avec signet|DBPROP_IROWSETLOCATE|  
|Signets triés|DBPROP_ORDEREDBOOKMARKS|  
|Mettre en cache les lignes enfants|DBPROP_ADC_CACHECHILDROWS|  
|Mettre en cache les colonnes différées|DBPROP_CACHEDEFERRED|  
|Modifier les lignes insérées|DBPROP_CHANGEINSERTEDROWS|  
|Privilèges de colonne|DBPROP_COLUMNRESTRICT|  
|Notification du jeu de colonnes|DBPROP_NOTIFYCOLUMNSET|  
|Colonne accessible en écriture|DBPROP_MAYWRITECOLUMN|  
|Délai d’attente de la commande|DBPROP_COMMANDTIMEOUT|  
|Version du moteur de curseur|DBPROP_ADC_CEVER|  
|Différer la colonne|DBPROP_DEFERRED|  
|Retarder les mises à jour des objets de stockage|DBPROP_DELAYSTORAGEOBJECTS|  
|Extraire vers l’arrière|DBPROP_CANFETCHBACKWARDS|  
|Opérations de filtre|DBPROP_FILTERCOMPAREOPS|  
|Opérations de recherche|DBPROP_FINDCOMPAREOPS|  
|Colonnes masquées (nombre)|DBPROP_HIDDENCOLUMNS|  
|Conserver les lignes|DBPROP_CANHOLDROWS|  
|Lignes immobiles|DBPROP_IMMOBILEROWS|  
|Taille initiale de l’extraction|DBPROP_ASYNCHPREFETCHSIZE|  
|Signets littéraux|DBPROP_LITERALBOOKMARKS|  
|Identité de ligne littérale|DBPROP_LITERALIDENTITY|  
|Conserver l’état de modification|DBPROP_ADC_MAINTAINCHANGESTATUS|  
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
|Private1||  
|Redémarrage rapide|DBPROP_QUICKRESTART|  
|Événements réentrants|DBPROP_REENTRANTEVENTS|  
|Supprimer les lignes supprimées|DBPROP_REMOVEDELETED|  
|Signaler plusieurs modifications|DBPROP_REPORTMULTIPLECHANGES|  
|Reformer le nom|DBPROP_ADC_RESHAPENAME|  
|Commande Resync|DBPROP_ADC_CUSTOMRESYNCH|  
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
|Ignorer les signets supprimés|DBPROP_BOOKMARKSKIPPED|  
|Identité de ligne forte|DBPROP_STRONGIDENTITY|  
|Catalogue unique|DBPROP_ADC_UNIQUECATALOG|  
|Lignes uniques|DBPROP_UNIQUEROWS|  
|Schéma unique|DBPROP_ADC_UNIQUESCHEMA|  
|table unique|DBPROP_ADC_UNIQUETABLE|  
|Mise à jour|DBPROP_UPDATABILITY|  
|Mettre à jour les critères|DBPROP_ADC_UPDATECRITERIA|  
|Mettre à jour la resynchronisation|DBPROP_ADC_UPDATERESYNC|  
|Utiliser des signets|DBPROP_BOOKMARKS|