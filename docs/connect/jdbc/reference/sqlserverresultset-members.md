---
description: Membres de SQLServerResultSet
title: Membres de SQLServerResultSet | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 2a438d5d-2d6a-46a0-a2ae-f35fbae4a472
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bf8e9b71b638a3836ebefb71e3d574183372267e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180102"
---
# <a name="sqlserverresultset-members"></a>Membres de SQLServerResultSet
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Les tableaux suivants listent les membres qui sont exposés par la classe [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="constructors"></a>Constructeurs  
 Aucun.  
  
## <a name="fields"></a>Champs  
  
|Nom|Description|  
|----------|-----------------|  
|[CONCUR_SS_OPTIMISTIC_CC](../../../connect/jdbc/reference/concur-ss-optimistic-cc-field-sqlserverresultset.md)|Utilisé pour spécifier un type [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de concurrence optimiste en lecture/écriture, sans verrou de ligne.|  
|[CONCUR_SS_OPTIMISTIC_CCVAL](../../../connect/jdbc/reference/concur-ss-optimistic-ccval-field-sqlserverresultset.md)|Utilisé pour spécifier un type [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de concurrence optimiste en lecture/écriture, sans verrou de ligne.|  
|[CONCUR_SS_SCROLL_LOCKS](../../../connect/jdbc/reference/concur-ss-scroll-locks-field-sqlserverresultset.md)|Utilisé pour spécifier un type [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] concurrentiel optimiste en lecture/écriture, avec des verrous de ligne.|  
|[TYPE_SS_DIRECT_FORWARD_ONLY](../../../connect/jdbc/reference/type-ss-direct-forward-only-field-sqlserverresultset.md)|Sert à spécifier un type de curseur avance rapide uniquement, en lecture seule [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[TYPE_SS_SCROLL_DYNAMIC](../../../connect/jdbc/reference/type-ss-scroll-dynamic-field-sqlserverresultset.md)|Utilisé pour spécifier un type de curseur dynamique [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[TYPE_SS_SCROLL_KEYSET](../../../connect/jdbc/reference/type-ss-scroll-keyset-field-sqlserverresultset.md)|Sert à spécifier un type de curseur de jeu de clés [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[TYPE_SS_SCROLL_STATIC](../../../connect/jdbc/reference/type-ss-scroll-static-field-sqlserverresultset.md)|Sert à spécifier un type de curseur statique [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[TYPE_SS_SERVER_CURSOR_FORWARD_ONLY](../../../connect/jdbc/reference/type-ss-server-cursor-forward-only-field-sqlserverresultset.md)|Sert à spécifier un type de curseur avance rapide uniquement, en lecture seule [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
  
## <a name="inherited-fields"></a>Champs hérités  
  
|Classe héritée de :|Description|  
|---------------------------|-----------------|  
|java.sql.ResultSet|CLOSE_CURSORS_AT_COMMIT, CONCUR_READ_ONLY, CONCUR_UPDATABLE, FETCH_FORWARD, FETCH_REVERSE, FETCH_UNKNOWN, HOLD_CURSORS_OVER_COMMIT, TYPE_FORWARD_ONLY, TYPE_SCROLL_INSENSITIVE, TYPE_SCROLL_SENSITIVE|  
  
## <a name="methods"></a>Méthodes  
  
|Nom|Description|  
|----------|-----------------|  
|[absolute](../../../connect/jdbc/reference/absolute-method-sqlserverresultset.md)|Déplace le curseur à la ligne spécifiée dans cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[afterLast](../../../connect/jdbc/reference/afterlast-method-sqlserverresultset.md)|Déplace le curseur après la dernière ligne de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[beforeFirst](../../../connect/jdbc/reference/beforefirst-method-sqlserverresultset.md)|Déplace le curseur avant la première ligne de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[cancelRowUpdates](../../../connect/jdbc/reference/cancelrowupdates-method-sqlserverresultset.md)|Annule les mises à jour apportées à la ligne actuelle dans cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverresultset.md)|Efface tous les avertissements signalés sur cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverresultset.md)|Libère immédiatement les ressources de base de données et JDBC de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md), au lieu d’attendre que cette opération se produise lors de sa fermeture automatique.|  
|[deleteRow](../../../connect/jdbc/reference/deleterow-method-sqlserverresultset.md)|Supprime la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) et de la base de données sous-jacente.|  
|[finalize](../../../connect/jdbc/reference/finalize-method-sqlserverresultset.md)|Ferme explicitement cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[findColumn](../../../connect/jdbc/reference/findcolumn-method-sqlserverresultset.md)|Récupère l’index de la première colonne correspondante pour le nom de colonne spécifié dans cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[first](../../../connect/jdbc/reference/first-method-sqlserverresultset.md)|Déplace le curseur à la première ligne de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getArray](../../../connect/jdbc/reference/getarray-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant qu’objet Array.|  
|[getAsciiStream](../../../connect/jdbc/reference/getasciistream-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que flux de caractères ASCII.|  
|[getBigDecimal](../../../connect/jdbc/reference/getbigdecimal-method-sqlserverresultset.md)|Récupère la valeur de l’index de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que java.math.BigDecimal.|  
|[getBinaryStream](../../../connect/jdbc/reference/getbinarystream-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que flux binaire d’octets non interprétés.|  
|[getBlob](../../../connect/jdbc/reference/getblob-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant qu’objet Blob dans le langage de programmation Java.|  
|[getBoolean](../../../connect/jdbc/reference/getboolean-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que **booléen** dans le langage de programmation Java.|  
|[getByte](../../../connect/jdbc/reference/getbyte-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que **octet** dans le langage de programmation Java.|  
|[getBytes](../../../connect/jdbc/reference/getbytes-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que tableau **d’octets** dans le langage de programmation Java.|  
|[getCharacterStream](../../../connect/jdbc/reference/getcharacterstream-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant qu’objet java.io.Reader.|  
|[getClob](../../../connect/jdbc/reference/getclob-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant qu’objet Clob dans le langage de programmation Java.|  
|[getConcurrency](../../../connect/jdbc/reference/getconcurrency-method-sqlserverresultset.md)|Récupère le mode de concurrence de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getCursorName](../../../connect/jdbc/reference/getcursorname-method-sqlserverresultset.md)|Récupère le nom du curseur SQL utilisé par cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getDate](../../../connect/jdbc/reference/getdate-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant qu’objet java.sql.Date dans le langage de programmation Java.|  
|[getDateTimeOffset](../../../connect/jdbc/reference/getdatetimeoffset-sqlserverresultset.md)|Récupère la valeur de la colonne spécifiée sous la forme d’un objet [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[getDouble](../../../connect/jdbc/reference/getdouble-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que **double** dans le langage de programmation Java.|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverresultset.md)|Récupère la direction d’extraction pour cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverresultset.md)|Récupère la taille d’extraction pour cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getFloat](../../../connect/jdbc/reference/getfloat-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que **float** dans le langage de programmation Java.|  
|[getHoldability](../../../connect/jdbc/reference/getholdability-method-sqlserverresultset.md)|Récupère la fonctionnalité de mise en attente de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getInt](../../../connect/jdbc/reference/getint-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que **int** dans le langage de programmation Java.|  
|[getLong](../../../connect/jdbc/reference/getlong-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que **long** dans le langage de programmation Java.|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverresultset.md)|Récupère le nombre, les types et les propriétés des colonnes de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getNCharacterStream](../../../connect/jdbc/reference/getncharacterstream-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de l’objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant qu’objet Reader.|  
|[getNClob](../../../connect/jdbc/reference/getnclob-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de l’objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant qu’objet **NClob** dans le langage de programmation Java.|  
|[getNString](../../../connect/jdbc/reference/getnstring-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de l’objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que String dans le langage de programmation Java.|  
|[getObject](../../../connect/jdbc/reference/getobject-method-sqlserverresultset.md)|Obtient la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant qu’objet dans le langage de programmation Java.|  
|[getRef](../../../connect/jdbc/reference/getref-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant qu’objet Ref dans le langage de programmation Java.|  
|[getRow](../../../connect/jdbc/reference/getrow-method-sqlserverresultset.md)|Récupère le nombre de lignes actuel|  
|[getShort](../../../connect/jdbc/reference/getshort-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que **short** dans le langage de programmation Java.|  
|[getStatement](../../../connect/jdbc/reference/getstatement-method-sqlserverresultset.md)|Récupère l’objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverstatement-class.md) qui a produit cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getString](../../../connect/jdbc/reference/getstring-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de l’objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que **String** dans le langage de programmation Java.|  
|[getSQLXML](../../../connect/jdbc/reference/getsqlxml-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de l’objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant qu’objet **SQLXML**.|  
|[getTime](../../../connect/jdbc/reference/gettime-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant qu’objet java.sql.Time dans le langage de programmation Java.|  
|[getTimestamp](../../../connect/jdbc/reference/gettimestamp-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant qu’objet java.sql.Timestamp dans le langage de programmation Java.|  
|[getType](../../../connect/jdbc/reference/gettype-method-sqlserverresultset.md)|Récupère le type de curseur de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getUnicodeStream](../../../connect/jdbc/reference/getunicodestream-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que flux de caractères Unicode.|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverresultset.md)|Récupère la valeur de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant qu’objet URL.|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverresultset.md)|Récupère le premier avertissement signalé par des appels sur cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[insertRow](../../../connect/jdbc/reference/insertrow-method-sqlserverresultset.md)|Insère le contenu de la ligne d’insertion dans cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) et dans la base de données.|  
|[isAfterLast](../../../connect/jdbc/reference/isafterlast-method-sqlserverresultset.md)|Récupère l’information indiquant si le curseur se trouve après la dernière ligne dans cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[isBeforeFirst](../../../connect/jdbc/reference/isbeforefirst-method-sqlserverresultset.md)|Récupère l’information indiquant si le curseur se trouve avant la première ligne dans cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverresultset.md)|Indique si cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) a été fermé.|  
|[isFirst](../../../connect/jdbc/reference/isfirst-method-sqlserverresultset.md)|Récupère l’information indiquant si le curseur se trouve sur la première ligne de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[isLast](../../../connect/jdbc/reference/islast-method-sqlserverresultset.md)|Récupère l’information indiquant si le curseur se trouve sur la dernière ligne de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[last](../../../connect/jdbc/reference/last-method-sqlserverresultset.md)|Déplace le curseur à la dernière ligne dans cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[moveToCurrentRow](../../../connect/jdbc/reference/movetocurrentrow-method-sqlserverresultset.md)|Déplace le curseur à la position de curseur mémorisée, généralement la ligne actuelle.|  
|[moveToInsertRow](../../../connect/jdbc/reference/movetoinsertrow-method-sqlserverresultset.md)|Déplace le curseur vers la ligne d'insertion.|  
|[suivant](../../../connect/jdbc/reference/next-method-sqlserverresultset.md)|Déplace le curseur d'une ligne vers le bas depuis sa position actuelle.|  
|[previous](../../../connect/jdbc/reference/previous-method-sqlserverresultset.md)|Déplace le curseur à la ligne précédente dans cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[refreshRow](../../../connect/jdbc/reference/refreshrow-method-sqlserverresultset.md)|Actualise la ligne actuelle avec sa valeur la plus récente dans la base de données.|  
|[relative](../../../connect/jdbc/reference/relative-method-sqlserverresultset.md)|Déplace le curseur en fonction du nombre de lignes spécifié, par rapport à la ligne actuelle, dans une direction positive ou négative.|  
|[rowDeleted](../../../connect/jdbc/reference/rowdeleted-method-sqlserverresultset.md)|Récupère les informations déterminant si une ligne a été supprimée.|  
|[rowInserted](../../../connect/jdbc/reference/rowinserted-method-sqlserverresultset.md)|Récupère les informations déterminant si la ligne actuelle a eu une insertion.|  
|[rowUpdated](../../../connect/jdbc/reference/rowupdated-method-sqlserverresultset.md)|Récupère les informations déterminant si la ligne actuelle a été mise à jour.|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverresultset.md)|Fournit un conseil concernant la direction de traitement des lignes de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverresultset.md)|Fournit un conseil au pilote JDBC concernant le nombre de lignes qui doivent être extraites de la base de données, quand davantage de lignes sont nécessaires pour cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[updateArray](../../../connect/jdbc/reference/updatearray-method-sqlserverresultset.md)|Met à jour la colonne désignée avec un objet Array.|  
|[updateAsciiStream](../../../connect/jdbc/reference/updateasciistream-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur de flux ASCII.|  
|[updateBigDecimal](../../../connect/jdbc/reference/updatebigdecimal-method-sqlserverresultset.md)|Met à jour la colonne désignée avec un objet BigDecimal.|  
|[updateBinaryStream](../../../connect/jdbc/reference/updatebinarystream-method-sqlserverresultset.md)|Met à jour la colonne désignée avec la valeur de flux binaire.|  
|[updateBlob](../../../connect/jdbc/reference/updateblob-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur java.sql.Blob.|  
|[updateBoolean](../../../connect/jdbc/reference/updateboolean-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur **booléenne**.|  
|[updateByte](../../../connect/jdbc/reference/updatebyte-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur **byte**.|  
|[updateBytes](../../../connect/jdbc/reference/updatebytes-method-sqlserverresultset.md)|Met à jour la colonne désignée avec un tableau de valeurs **byte**.|  
|[updateCharacterStream](../../../connect/jdbc/reference/updatecharacterstream-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur de flux de caractères.|  
|[updateClob](../../../connect/jdbc/reference/updateclob-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur java.sql.Clob.|  
|[updateDate](../../../connect/jdbc/reference/updatedate-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur de date.|  
|[updateDateTimeOffset](../../../connect/jdbc/reference/updatedatetimeoffset-sqlserverresultset.md)|Met à jour une colonne [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[updateDouble](../../../connect/jdbc/reference/updatedouble-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur **double**.|  
|[updateFloat](../../../connect/jdbc/reference/updatefloat-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur **float**.|  
|[updateInt](../../../connect/jdbc/reference/updateint-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur **int**.|  
|[updateLong](../../../connect/jdbc/reference/updatelong-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur **long**.|  
|[updateNCharacterStream](../../../connect/jdbc/reference/updatencharacterstream-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur de flux de caractères.|  
|[updateNClob](../../../connect/jdbc/reference/updatenclob-method-sqlserverresultset.md)|Met à jour la colonne désignée avec la valeur d'objet spécifiée.|  
|[updateNString](../../../connect/jdbc/reference/updatenstring-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur **String**.|  
|[updateNull](../../../connect/jdbc/reference/updatenull-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur Null.|  
|[updateObject](../../../connect/jdbc/reference/updateobject-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur **Object**.|  
|[updateRef](../../../connect/jdbc/reference/updateref-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur java.sql.Ref.|  
|[updateRow](../../../connect/jdbc/reference/updaterow-method-sqlserverresultset.md)|Met à jour la base de données sous-jacente avec le nouveau contenu de la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[updateShort](../../../connect/jdbc/reference/updateshort-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur **short**.|  
|[updateString](../../../connect/jdbc/reference/updatestring-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur **String**.|  
|[updateSQLXML](../../../connect/jdbc/reference/updatesqlxml-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur **SQLXML**.|  
|[updateTime](../../../connect/jdbc/reference/updatetime-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur d'heure.|  
|[updateTimestamp](../../../connect/jdbc/reference/updatetimestamp-method-sqlserverresultset.md)|Met à jour la colonne désignée avec une valeur d'horodateur.|  
|[wasNull](../../../connect/jdbc/reference/wasnull-method-sqlserverresultset.md)|Vérifie si la dernière valeur lue était une valeur NULL.|  
  
## <a name="inherited-methods"></a>Méthodes héritées  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
