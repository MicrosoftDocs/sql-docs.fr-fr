---
description: Membres de SQLServerCallableStatement
title: Membres de SQLServerCallableStatement | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apitype: Assembly
ms.assetid: 5ebdc186-e50f-4d14-bbf4-95af5051e4a4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f134ee20c60dc45b216122f76482f6a331fa9a35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172872"
---
# <a name="sqlservercallablestatement-members"></a>Membres de SQLServerCallableStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Les tableaux suivants présentent les membres exposés par la classe [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md).  
  
## <a name="constructors"></a>Constructeurs  
 Aucun.  
  
## <a name="fields"></a>Champs  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|java.sql.Statement|CLOSE_ALL_RESULTS, CLOSE_CURRENT_RESULT, EXECUTE_FAILED, KEEP_CURRENT_RESULT, NO_GENERATED_KEYS, RETURN_GENERATED_KEYS, SUCCESS_NO_INFO|  
  
## <a name="inherited-fields"></a>Champs hérités  
 Aucun.  
  
## <a name="methods"></a>Méthodes  
  
|Nom|Description|  
|----------|-----------------|  
|[addBatch](../../../connect/jdbc/reference/addbatch-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Ajoute un ensemble de paramètres au lot de commandes de cet objet CallableStatement.|  
|[cancel](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Annule l’instruction SQL actuellement exécutée par cet objet CallableStatement.|  
|[clearBatch](../../../connect/jdbc/reference/clearbatch-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Vide la liste actuelle de commandes SQL de cet objet CallableStatement.|  
|[clearParameters](../../../connect/jdbc/reference/clearparameters-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Efface immédiatement les valeurs des paramètres actuels.|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Efface tous les avertissements signalés sur cet objet CallableStatement.|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Libère immédiatement les ressources JDBC et la base de données de cet objet CallableStatement, au lieu de patienter jusqu’à leur libération automatique.|  
|[execute](../../../connect/jdbc/reference/execute-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Exécute l’instruction SQL dans cet objet CallableStatement, qui peut être n’importe quel type d’instruction SQL.|  
|[executeBatch](../../../connect/jdbc/reference/executebatch-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Envoie un lot de commandes à exécuter à la base de données. En cas d'exécution correcte de toutes les commandes, un tableau des nombres de mises à jour est retourné.|  
|[executeQuery](../../../connect/jdbc/reference/executequery-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Exécute la requête SQL dans cet objet CallableStatement, puis retourne l’objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) généré par la requête.|  
|[executeUpdate](../../../connect/jdbc/reference/executeupdate-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Exécute l’instruction SQL dans cet objet CallableStatement, qui doit être une instruction SQL INSERT, UPDATE, MERGE ou DELETE, ou une instruction SQL qui ne retourne rien, comme une instruction DDL.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère l’objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) qui a produit cet objet CallableStatement.|  
|[getDateTimeOffset](../../../connect/jdbc/reference/getdatetimeoffset-method-sqlservercallablestatement.md)|Récupère la valeur de la colonne spécifiée sous la forme d’un objet [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère la direction d’extraction des lignes dans des tables de base de données. C’est la méthode par défaut pour les jeux de résultats générés à partir de cet objet CallableStatement.|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le nombre de lignes du jeu de résultats. C’est la taille d’extraction par défaut pour les objets du jeu de résultats générés à partir de cet objet CallableStatement.|  
|[getGeneratedKeys](../../../connect/jdbc/reference/getgeneratedkeys-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère toutes les clés générées automatiquement du fait de l’exécution de cet objet CallableStatement.|  
|[getMaxFieldSize](../../../connect/jdbc/reference/getmaxfieldsize-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le nombre maximal d’octets pouvant être retournés pour des valeurs de colonne de caractères ou binaire dans un objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) produit par cet objet CallableStatement.|  
|[getMaxRows](../../../connect/jdbc/reference/getmaxrows-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le nombre maximal de lignes que peut contenir un objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) produit par cet objet CallableStatement.|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Récupère un objet [SQLServerResultSetMetaData Class](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md) contenant des informations sur les colonnes de l’objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) retourné à l’exécution de cet objet CallableStatement.|  
|[getMoreResults](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Passe au résultat suivant de cet objet CallableStatement.|  
|[getParameterMetaData](../../../connect/jdbc/reference/getparametermetadata-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Récupère le nombre, les types et les propriétés des paramètres de cet objet CallableStatement.|  
|[getArray](../../../connect/jdbc/reference/getarray-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné sous forme d’objet Array.|  
|[getAsciiStream](../../../connect/jdbc/reference/getasciistream-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné sous forme de flux de caractères **ASCII**.|  
|[getBigDecimal](../../../connect/jdbc/reference/getbigdecimal-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant que java.math.BigDecimal.|  
|[getBinaryStream](../../../connect/jdbc/reference/getbinarystream-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant que flux binaire d'octets ininterrompus.|  
|[getBlob](../../../connect/jdbc/reference/getblob-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre Blob JDBC désigné sous forme d’objet Blob dans le langage de programmation Java.|  
|[getboolean](../../../connect/jdbc/reference/getboolean-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné sous forme de valeur **booléenne**.|  
|[getByte](../../../connect/jdbc/reference/getbyte-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné sous forme de valeur **d’octet**.|  
|[getBytes](../../../connect/jdbc/reference/getbytes-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant que tableau d'octets.|  
|[getCharacterStream](../../../connect/jdbc/reference/getcharacterstream-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu'objet java.io.Reader.|  
|[getClob](../../../connect/jdbc/reference/getclob-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre Blob JDBC désigné sous forme d’objet Clob dans le langage de programmation Java.|  
|[getDate](../../../connect/jdbc/reference/getdate-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu'objet java.sql.Date dans le langage de programmation Java.|  
|[getDateTimeOffset](../../../connect/jdbc/reference/getdatetimeoffset-method-sqlservercallablestatement.md)|Récupère la valeur de la colonne spécifiée sous la forme d’un objet [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[getDouble](../../../connect/jdbc/reference/getdouble-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné sous forme d’objet **double** dans le langage de programmation Java.|  
|[getFloat](../../../connect/jdbc/reference/getfloat-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné sous forme d’objet **float** dans le langage de programmation Java.|  
|[getInt](../../../connect/jdbc/reference/getint-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné sous forme **d’int** dans le langage de programmation Java.|  
|[getLong](../../../connect/jdbc/reference/getlong-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné sous forme d’objet **long** dans le langage de programmation Java.|  
|[getNCharacterStream](../../../connect/jdbc/reference/getncharacterstream-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné sous forme d’objet Reader.|  
|[getNClob](../../../connect/jdbc/reference/getnclob-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre **NCLOB** JDBC désigné sous forme d’objet **NClob** dans le langage de programmation Java.|  
|[getNString](../../../connect/jdbc/reference/getnstring-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre **NCHAR**, **NVARCHAR** ou **LONGNVARCHAR** désigné sous forme de chaîne dans le langage de programmation Java.|  
|[getObject](../../../connect/jdbc/reference/getobject-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu'objet dans le langage de programmation Java.|  
|[getQueryTimeout](../../../connect/jdbc/reference/getquerytimeout-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le nombre de secondes pendant lesquelles [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] attend que cet objet CallableStatement s’exécute.|  
|[getRef](../../../connect/jdbc/reference/getref-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné sous forme d’objet Ref dans le langage de programmation Java.|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le mode de mise en mémoire tampon des réponses de cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getResultSet](../../../connect/jdbc/reference/getresultset-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le résultat actuel sous forme d’objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getResultSetConcurrency](../../../connect/jdbc/reference/getresultsetconcurrency-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère la concurrence du jeu de résultats pour les objets [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) générés par cet objet CallableStatement.|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère la capacité de mise en attente du jeu de résultats pour les objets [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) générés par cet objet CallableStatement.|  
|[getResultSetType](../../../connect/jdbc/reference/getresultsettype-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le type du jeu de résultats pour les objets [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) générés par cet objet CallableStatement.|  
|[getShort](../../../connect/jdbc/reference/getshort-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné sous forme d’objet **short** dans le langage de programmation Java.|  
|[getString](../../../connect/jdbc/reference/getstring-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné sous forme de **String** dans le langage de programmation Java.|  
|[getSQLXML](../../../connect/jdbc/reference/getsqlxml-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu'objet java.sql.SQLXML.|  
|[getTime](../../../connect/jdbc/reference/gettime-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu'objet java.sql.Time dans le langage de programmation Java.|  
|[getTimestamp](../../../connect/jdbc/reference/gettimestamp-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné en tant qu'objet java.sql.Timestamp dans le langage de programmation Java.|  
|[getUpdateCount](../../../connect/jdbc/reference/getupdatecount-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le résultat actuel en tant que nombre de mises à jour.|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlservercallablestatement.md)|Récupère la valeur du paramètre désigné sous forme d’objet URL dans le langage de programmation Java.|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le premier avertissement signalé par des appels sur cet objet CallableStatement.|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Indique si cet objet Statement a été fermé.|  
|[isPoolable](../../../connect/jdbc/reference/ispoolable-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Retourne une valeur qui indique s'il est possible d'ajouter une instruction au regroupement d'instructions fourni par l'utilisateur.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlservercallablestatement.md)|Indique si cet objet d'instruction est un wrapper pour l'interface spécifiée.|  
|[registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md)|Inscrit le paramètre OUT.|  
|[setArray](../../../connect/jdbc/reference/setarray-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Définit le numéro de paramètre désigné selon l’objet Array donné.|  
|[setAsciiStream](../../../connect/jdbc/reference/setasciistream-sqlservercallablestatement.md)|Définit le nombre de paramètres désignés selon le flux d'entrée donné.|  
|[setBigDecimal](../../../connect/jdbc/reference/setbigdecimal-method-sqlservercallablestatement.md)|Définit le numéro de paramètre désigné selon l’objet BigDecimal donné.|  
|[setBinaryStream](../../../connect/jdbc/reference/setbinarystream-sqlservercallablestatement.md)|Définit le paramètre désigné selon le flux d'entrée spécifié.|  
|[setBlob](../../../connect/jdbc/reference/setblob-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Définit le paramètre désigné selon l’objet Blob donné.|  
|[setboolean](../../../connect/jdbc/reference/setboolean-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la valeur **booléenne** donnée.|  
|[setByte](../../../connect/jdbc/reference/setbyte-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la valeur **d’octet** donnée.|  
|[setBytes](../../../connect/jdbc/reference/setbytes-method-sqlservercallablestatement.md)|Définit le paramètre désigné sur le tableau de valeurs **d’octet**.|  
|[setCharacterStream](../../../connect/jdbc/reference/setcharacterstream-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon l’objet Reader donné.|  
|[setClob](../../../connect/jdbc/reference/setclob-method-sqlservercallablestatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Définit le paramètre désigné selon l'objet spécifié.|  
|[setCursorName](../../../connect/jdbc/reference/setcursorname-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit le nom de curseur SQL selon la chaîne donnée, qui est exécutée par les méthodes d'exécution suivantes.|  
|[setDate](../../../connect/jdbc/reference/setdate-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la valeur de date donnée.|  
|[setDateTimeOffset](../../../connect/jdbc/reference/setdatetimeoffset-method-sqlservercallablestatement.md)|Définit la valeur de la colonne spécifiée sur la valeur [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[setDouble](../../../connect/jdbc/reference/setdouble-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la valeur **double** donnée.|  
|[setEscapeProcessing](../../../connect/jdbc/reference/setescapeprocessing-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit le mode de traitement d'échappement.|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Fournit un conseil au pilote JDBC concernant la direction de traitement des lignes du jeu de résultats.|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Fournit un conseil au pilote JDBC concernant le nombre de lignes qui doivent être extraites depuis la base de données, lorsque davantage de lignes sont nécessaires.|  
|[setFloat](../../../connect/jdbc/reference/setfloat-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la valeur **float** spécifiée.|  
|[setInt](../../../connect/jdbc/reference/setint-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la valeur **int** spécifiée.|  
|[setLong](../../../connect/jdbc/reference/setlong-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la valeur **long** spécifiée.|  
|[setMaxFieldSize](../../../connect/jdbc/reference/setmaxfieldsize-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit le nombre maximal d’octets dans une colonne [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) qui stocke des valeurs de caractères ou binaires selon le nombre d’octets spécifié.|  
|[setMaxRows](../../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit le nombre maximal de lignes que peuvent contenir les objets [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) selon le nombre spécifié.|  
|[setNCharacterStream](../../../connect/jdbc/reference/setncharacterstream-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon l’objet Reader spécifié.|  
|[setNClob](../../../connect/jdbc/reference/setnclob-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon l'objet spécifié.|  
|[setNString](../../../connect/jdbc/reference/setnstring-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon l’objet String spécifié.|  
|[setNull](../../../connect/jdbc/reference/setnull-method-sqlservercallablestatement.md)|Définit le paramètre désigné avec une valeur Null, en fonction du type de paramètre à définir.|  
|[setObject](../../../connect/jdbc/reference/setobject-method-sqlservercallablestatement.md)|Définit la valeur du paramètre désigné à l’aide de l’objet spécifique.|  
|[setPoolable](../../../connect/jdbc/reference/setpoolable-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Demande qu'une instruction soit regroupée ou non. Par défaut, un objet SQLServerCallableStatement est regroupable lors de sa création.|  
|[setQueryTimeout](../../../connect/jdbc/reference/setquerytimeout-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit le nombre de secondes pendant lesquelles le pilote attend qu’un objet CallableStatement s’exécute selon le nombre de secondes spécifié.|  
|[setRef](../../../connect/jdbc/reference/setref-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Définit le paramètre désigné selon l’objet Ref spécifié.|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit le mode de mise en mémoire tampon des réponses de cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) selon **String full** ou **adaptive**, sans respect de la casse.|  
|[setShort](../../../connect/jdbc/reference/setshort-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la valeur **short** spécifiée.|  
|[setString](../../../connect/jdbc/reference/setstring-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la valeur **String** Java spécifiée.|  
|[setSQLXML](../../../connect/jdbc/reference/setsqlxml-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon l’objet **SQLXML** spécifié.|  
|[setTime](../../../connect/jdbc/reference/settime-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la valeur d'heure spécifiée.|  
|[setTimestamp](../../../connect/jdbc/reference/settimestamp-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la valeur d'horodatage spécifiée.|  
|[setUnicodeStream](../../../connect/jdbc/reference/setunicodestream-method-sqlserverpreparedstatement.md)|(Héritée de [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).) Définit le numéro de paramètre désigné selon le flux d'entrée donné, qui disposera du nombre spécifique d'octets.|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlservercallablestatement.md)|Définit le paramètre désigné selon la valeur de l'URL spécifiée.|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlservercallablestatement.md)|Retourne un objet qui implémente l’interface spécifiée, afin d’autoriser l’accès aux méthodes propres au [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].|  
|[wasNull](../../../connect/jdbc/reference/wasnull-method-sqlservercallablestatement.md)|Récupère les informations déterminant si le dernier paramètre OUT lu possédait la valeur SQL NULL.|  
  
## <a name="inherited-methods"></a>Méthodes héritées  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerPreparedStatement|addBatch, clearBatch, clearParameters, close, execute, executeBatch, executeQuery, executeUpdate, getMetaData, getParameterMetaData, setArray, setAsciiStream, setBigDecimal, setBinaryStream, setBlob, setboolean, setByte, setBytes, setCharacterStream, setClob, setDate, setDouble, setFloat, setInt, setLong, setNull, setObject, setRef, setShort, setString, setTime, setTimestamp, setUnicodeStream, setURL|  
|com.microsoft.sqlserver.jdbc.SQLServerStatement|cancel, clearWarnings, execute, executeUpdate, getConnection, getFetchDirection, getFetchSize, getGeneratedKeys, getMaxFieldSize, getMaxRows, getMoreResults, getQueryTimeout, getResultSet, getResultSetConcurrency, getResultSetHoldability, getResultSetType, getUpdateCount, getWarnings, isPoolable, setCursorName, setEscapeProcessing, setFetchDirection, setFetchSize, setMaxFieldSize, setMaxRows, setPoolable, setQueryTimeout|  
|class java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait,|  
|java.sql.PreparedStatement|addBatch, clearParameters, execute, executeQuery, executeUpdate, getMetaData, getParameterMetaData, getSQLXML, setArray, setAsciiStream, setBigDecimal, setBinaryStream, setBlob, setboolean, setByte, setBytes, setCharacterStream, setClob, setDate, setDate, setDouble, setFloat, setInt, setLong, setNull, setObject, setRef, setShort, setString, setSQLXML, setTime, setTimestamp, setUnicodeStream, setURL|  
|java.sql.Statement|addBatch, cancel, clearBatch, clearWarnings, close, execute, executeBatch, executeQuery, executeUpdate, getConnection, getFetchDirection, getFetchSize, getGeneratedKeys, getMaxFieldSize, getMaxRows, getMoreResults, getQueryTimeout, getResultSet, getResultSetConcurrency, getResultSetHoldability, getResultSetType, getUpdateCount, getWarnings, setCursorName, setEscapeProcessing, setFetchDirection, setFetchSize, setMaxFieldSize, setMaxRows, setQueryTimeout|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Voir aussi  
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
