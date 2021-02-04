---
description: Membres de SQLServerPreparedStatement
title: Membres de SQLServerPreparedStatement | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 2363902f-d4c6-4cd4-a5fc-86079eb9e418
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 74ae525bdd1c05eb01f9bce98cba9ef987087a30
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167996"
---
# <a name="sqlserverpreparedstatement-members"></a>Membres de SQLServerPreparedStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Les tableaux suivants présentent les membres exposés par la classe [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).  
  
## <a name="constructors"></a>Constructeurs  
 Aucun.  
  
## <a name="fields"></a>Champs  
 Aucun.  
  
## <a name="inherited-fields"></a>Champs hérités  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|java.sql.Statement|CLOSE_ALL_RESULTS, CLOSE_CURRENT_RESULT, EXECUTE_FAILED, KEEP_CURRENT_RESULT, NO_GENERATED_KEYS, RETURN_GENERATED_KEYS, SUCCESS_NO_INFO|  
  
## <a name="methods"></a>Méthodes  
  
|Nom|Description|  
|----------|-----------------|  
|[addBatch](../../../connect/jdbc/reference/addbatch-method-sqlserverpreparedstatement.md)|Ajoute un ensemble de paramètres au lot de commandes de cet objet Statement.|  
|[cancel](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Annule l’instruction SQL actuellement exécutée par cet objet Statement.|  
|[clearBatch](../../../connect/jdbc/reference/clearbatch-method-sqlserverpreparedstatement.md)|Vide la liste actuelle de commandes SQL de cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[clearParameters](../../../connect/jdbc/reference/clearparameters-method-sqlserverpreparedstatement.md)|Efface immédiatement les valeurs des paramètres actuels.|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Efface tous les avertissements signalés sur cet objet Statement.|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverpreparedstatement.md)|Libère immédiatement les ressources JDBC et la base de données de cet objet Statement, au lieu de patienter jusqu’à leur libération automatique.|  
|[execute](../../../connect/jdbc/reference/execute-method-sqlserverpreparedstatement.md)|Exécute l’instruction SQL dans cet objet Statement, qui peut être n’importe quel type d’instruction SQL.|  
|[executeBatch](../../../connect/jdbc/reference/executebatch-method-sqlserverpreparedstatement.md)|Envoie un lot de commandes à exécuter à la base de données. En cas d'exécution correcte de toutes les commandes, un tableau des nombres de mises à jour est retourné.|  
|[executeQuery](../../../connect/jdbc/reference/executequery-method-sqlserverpreparedstatement.md)|Exécute la requête SQL dans cet objet Statement, puis retourne l’objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) généré par la requête.|  
|[executeUpdate](../../../connect/jdbc/reference/executeupdate-method-sqlserverpreparedstatement.md)|Exécute l’instruction SQL dans cet objet Statement, qui doit être une instruction SQL INSERT, UPDATE, MERGE ou DELETE, ou une instruction SQL qui ne retourne rien, comme une instruction DDL.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère l’objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) qui a produit cet objet Statement.|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère la direction d’extraction des lignes dans des tables de base de données. C’est la méthode par défaut pour les jeux de résultats générés à partir de cet objet Statement.|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le nombre de lignes du jeu de résultats. C’est la taille d’extraction par défaut pour les objets du jeu de résultats générés à partir de cet objet Statement.|  
|[getGeneratedKeys](../../../connect/jdbc/reference/getgeneratedkeys-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère toutes les clés générées automatiquement du fait de l’exécution de cet objet Statement.|  
|[getMaxFieldSize](../../../connect/jdbc/reference/getmaxfieldsize-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le nombre maximal d’octets pouvant être retournés pour des valeurs de colonne de caractères ou binaire dans un objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) produit par cet objet Statement.|  
|[getMaxRows](../../../connect/jdbc/reference/getmaxrows-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le nombre maximal de lignes que peut contenir un objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) produit par cet objet Statement.|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverpreparedstatement.md)|Récupère un objet [SQLServerResultSetMetaData Class](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md) contenant des informations sur les colonnes de l’objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) retourné à l’exécution de cet objet Statement.|  
|[getMoreResults](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Passe au résultat suivant de cet objet Statement.|  
|[getParameterMetaData](../../../connect/jdbc/reference/getparametermetadata-method-sqlserverpreparedstatement.md)|Récupère le nombre, les types et les propriétés des paramètres de cet objet Statement.|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le mode de mise en mémoire tampon des réponses de cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getQueryTimeout](../../../connect/jdbc/reference/getquerytimeout-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le nombre de secondes pendant lesquelles [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] attend que cet objet Statement s’exécute.|  
|[getResultSet](../../../connect/jdbc/reference/getresultset-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le résultat actuel sous forme d’objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getResultSetConcurrency](../../../connect/jdbc/reference/getresultsetconcurrency-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère la concurrence du jeu de résultats pour les objets [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) générés par cet objet Statement.|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère la capacité de mise en attente du jeu de résultats pour les objets [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) générés par cet objet Statement.|  
|[getResultSetType](../../../connect/jdbc/reference/getresultsettype-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le type de jeu de résultats pour les objets [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) générés par cet objet Statement.|  
|[getUpdateCount](../../../connect/jdbc/reference/getupdatecount-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le résultat actuel comme nombre de mises à jour.|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Récupère le premier avertissement signalé par des appels sur cet objet Statement.|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Indique si cet objet Statement a été fermé.|  
|[isPoolable](../../../connect/jdbc/reference/ispoolable-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Retourne une valeur qui indique s'il est possible d'ajouter une instruction au regroupement d'instructions fourni par l'utilisateur.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverpreparedstatement.md)|Indique si cet objet d'instruction est un wrapper pour l'interface spécifiée.|  
|[setArray](../../../connect/jdbc/reference/setarray-method-sqlserverpreparedstatement.md)|Définit le numéro de paramètre désigné selon l’objet Array donné.|  
|[setAsciiStream](../../../connect/jdbc/reference/setasciistream-method-sqlserverpreparedstatement.md)|Définit le numéro de paramètre désigné selon l’objet InputStream donné.|  
|[setBigDecimal](../../../connect/jdbc/reference/setbigdecimal-method-sqlserverpreparedstatement.md)|Définit le numéro de paramètre désigné selon l’objet BigDecimal donné.|  
|[setBinaryStream](../../../connect/jdbc/reference/setbinarystream-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon le flux d'entrée spécifié.|  
|[setBlob](../../../connect/jdbc/reference/setblob-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon l’objet Blob spécifié.|  
|[setboolean](../../../connect/jdbc/reference/setboolean-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon la valeur **booléenne** spécifiée.|  
|[setByte](../../../connect/jdbc/reference/setbyte-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon la valeur **d’octet** spécifiée.|  
|[setBytes](../../../connect/jdbc/reference/setbytes-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon le tableau d'octets spécifié.|  
|[setCharacterStream](../../../connect/jdbc/reference/setcharacterstream-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon l’objet Reader spécifié.|  
|[setClob](../../../connect/jdbc/reference/setclob-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon l’objet Clob donné.|  
|[setCursorName](../../../connect/jdbc/reference/setcursorname-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Affecte la chaîne spécifiée en tant que nom de curseur SQL en vue de son utilisation par les méthodes d'exécution suivantes.|  
|[setDate](../../../connect/jdbc/reference/setdate-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon la valeur de date spécifiée.|  
|[setDateTimeOffset](../../../connect/jdbc/reference/setdatetimeoffset-method-sqlserverpreparedstatement.md)|Définit la valeur de la colonne spécifiée sur la valeur [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[setDouble](../../../connect/jdbc/reference/setdouble-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon la valeur **double** spécifiée.|  
|[setEscapeProcessing](../../../connect/jdbc/reference/setescapeprocessing-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit le mode de traitement d'échappement.|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Fournit un conseil au pilote JDBC concernant la direction de traitement des lignes du jeu de résultats.|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Fournit un conseil au pilote JDBC concernant le nombre de lignes qui doivent être extraites depuis la base de données, lorsque davantage de lignes sont nécessaires.|  
|[setFloat](../../../connect/jdbc/reference/setfloat-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon la valeur **float** spécifiée.|  
|[setInt](../../../connect/jdbc/reference/setint-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon la valeur **int** spécifiée.|  
|[setLong](../../../connect/jdbc/reference/setlong-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon la valeur **long** spécifiée.|  
|[setMaxFieldSize](../../../connect/jdbc/reference/setmaxfieldsize-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit le nombre maximal d’octets dans une colonne [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) qui stocke des valeurs de caractères ou binaires selon le nombre d’octets donné.|  
|[setMaxRows](../../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit la limite du nombre maximal de lignes que tout objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) peut contenir sur le nombre donné.|  
|[setNCharacterStream](../../../connect/jdbc/reference/setncharacterstream-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon l’objet Reader spécifié.|  
|[setNClob](../../../connect/jdbc/reference/setnclob-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon l'objet spécifié.|  
|[setNull](../../../connect/jdbc/reference/setnull-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné avec une valeur Null, en fonction du type de paramètre à définir.|  
|[setNString](../../../connect/jdbc/reference/setnstring-method-int-java-lang-string.md)|Définit le paramètre désigné selon l’objet **String** spécifié.|  
|[setObject](../../../connect/jdbc/reference/setobject-method-sqlserverpreparedstatement.md)|Définit la valeur du paramètre désigné à l’aide de l’objet spécifique.|  
|[setPoolable](../../../connect/jdbc/reference/setpoolable-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Demande qu'une instruction soit regroupée ou non. Par défaut, un objet SQLServerPreparedStatement est regroupable lors de sa création.|  
|[setQueryTimeout](../../../connect/jdbc/reference/setquerytimeout-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit le nombre de secondes pendant lesquelles le pilote attend qu’un objet Statement s’exécute selon le nombre de secondes spécifié.|  
|[setRef](../../../connect/jdbc/reference/setref-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon l’objet Ref spécifié.|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md)|(Hérité de [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).) Définit le mode de mise en mémoire tampon des réponses de cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) selon **String full** ou **adaptive**, sans respect de la casse.|  
|[setShort](../../../connect/jdbc/reference/setshort-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon la valeur **short** spécifiée.|  
|[setString](../../../connect/jdbc/reference/setstring-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon la valeur **String** spécifiée.|  
|[setSQLXML](../../../connect/jdbc/reference/setsqlxml-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon l’objet **SQLXML** spécifié.|  
|[setTime](../../../connect/jdbc/reference/settime-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon la valeur d'heure spécifiée.|  
|[setTimestamp](../../../connect/jdbc/reference/settimestamp-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon la valeur d'horodatage spécifiée.|  
|[setUnicodeStream](../../../connect/jdbc/reference/setunicodestream-method-sqlserverpreparedstatement.md)|Définit le numéro de paramètre désigné selon le flux d’entrée spécifié, qui aura le nombre d’octets spécifié.|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlserverpreparedstatement.md)|Définit le paramètre désigné selon la valeur de l'URL spécifiée.|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverpreparedstatement.md)|Retourne un objet qui implémente l’interface spécifiée, afin d’autoriser l’accès aux méthodes propres au [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].|  
  
## <a name="inherited-methods"></a>Méthodes héritées  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerStatement|cancel, clearWarnings, execute, executeUpdate, getConnection, getFetchDirection, getFetchSize, getGeneratedKeys, getMaxFieldSize, getMaxRows, getMoreResults, getQueryTimeout, getResultSet, getResultSetConcurrency, getResultSetHoldability, getResultSetType, getUpdateCount, getWarnings, isPoolable, setCursorName, setEscapeProcessing, setFetchDirection, setFetchSize, setMaxFieldSize, setMaxRows, setPoolable, setQueryTimeout|  
|java.lang.Object|clone, equals, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Statement|cancel, clearWarnings, execute, executeUpdate, getConnection, getFetchDirection, getFetchSize, getGeneratedKeys, getMaxFieldSize, getMaxRows, getMoreResults, getQueryTimeout, getResultSet, getResultSetConcurrency, getResultSetHoldability, getResultSetType, getUpdateCount, getWarnings, setCursorName, setEscapeProcessing, setFetchDirection, setFetchSize, setMaxFieldSize, setMaxRows, setQueryTimeout|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Voir aussi  
 [Classe SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
