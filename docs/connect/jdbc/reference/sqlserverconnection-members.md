---
description: Membres de SQLServerConnection
title: Membres SQLServerConnection | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 3115a533-756b-4c78-aee9-4ba7253c85e0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ed9783e0b91dd79fe7b52c80b05fa7693213759c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172839"
---
# <a name="sqlserverconnection-members"></a>Membres de SQLServerConnection
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Les tableaux suivants présentent les membres exposés par la classe [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).  
  
## <a name="constructors"></a>Constructeurs  
 Aucun.  
  
## <a name="fields"></a>Champs  
  
|Nom|Description|  
|----------|-----------------|  
|[TRANSACTION_SNAPSHOT](../../../connect/jdbc/reference/transaction-snapshot-field-sqlserverconnection.md)|Utilisé pour spécifier le niveau d'isolation de la transaction d'instantané.|  
  
## <a name="inherited-fields"></a>Champs hérités  
  
|Classe héritée de :|Description|  
|---------------------------|-----------------|  
|java.sql.Connection|TRANSACTION_NONE, TRANSACTION_READ_COMMITTED, TRANSACTION_READ_UNCOMMITTED, TRANSACTION_REPEATABLE_READ, TRANSACTION_SERIALIZABLE|  
  
## <a name="methods"></a>Méthodes  
  
|Nom|Description|  
|----------|-----------------|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverconnection.md)|Efface tous les avertissements signalés pour cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverconnection.md)|Libère immédiatement les ressources JDBC et la base de données de cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md), au lieu de patienter jusqu’à leur libération automatique.|  
|[closeUnreferencedPreparedStatementHandles](../../../connect/jdbc/reference/closeunreferencedpreparedstatementhandles-method-sqlserverconnection.md)|Force l’exécution des demandes d’annulation pour les instructions préparées ignorées en suspens.| 
|[commit](../../../connect/jdbc/reference/commit-method-sqlserverconnection.md)|Rend permanentes toutes les modifications effectuées depuis la précédente validation ou restauration, et libère tous les verrous de base de données actuellement maintenus par cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[createBlob](../../../connect/jdbc/reference/createblob-method-sqlserverconnection.md)|Crée un objet **java.sql.Blob** sans aucune donnée.|  
|[createClob](../../../connect/jdbc/reference/createclob-method-sqlserverconnection.md)|Crée un objet **java.sql.Clob** sans aucune donnée.|  
|[createNClob](../../../connect/jdbc/reference/createnclob-method-sqlserverconnection.md)|Crée un objet **java.sql.NClob** sans aucune donnée.|  
|[createStatement](../../../connect/jdbc/reference/createstatement-method-sqlserverconnection.md)|Crée un objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) servant à envoyer des instructions SQL à la base de données.|  
|[createSQLXML](../../../connect/jdbc/reference/createsqlxml-method-sqlserverconnection.md)|Crée un objet **java.sql.SQLXML** sans aucune donnée.|  
|[getAutoCommit](../../../connect/jdbc/reference/getautocommit-method-sqlserverconnection.md)|Récupère le mode de validation automatique actuel de cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[getCatalog](../../../connect/jdbc/reference/getcatalog-method-sqlserverconnection.md)|Récupère le nom de catalogue actuel de cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[getClientConnectionID, méthode &#40;SQLServerConnection&#41;](../../../connect/jdbc/reference/getclientconnectionid-method-sqlserverconnection.md)|Obtient l'ID de la tentative de connexion la plus récente, que cette tentative ait réussi ou non.|  
|[getClientInfo](../../../connect/jdbc/reference/getclientinfo-method-sqlserverconnection.md)|Récupère les informations concernant les propriétés d'informations client prises en charge par le pilote JDBC.|  
|[getDisableStatementPooling](../../../connect/jdbc/reference/getdisablestatementpooling-method-sqlserverconnection.md)|Retourne la valeur de la propriété de connexion **disableStatementPooling**. Ce paramètre contrôle si le regroupement d’instructions est activé ou non pour cette connexion.|
|[getDiscardedServerPreparedStatementCount](../../../connect/jdbc/reference/getdiscardedserverpreparedstatementcount-method-sqlserverconnection.md)|Retourne le nombre d’actions d’annulation d’instructions préparées actuellement en suspens.|
|[getEnablePrepareOnFirstPreparedStatementCall](../../../connect/jdbc/reference/getenableprepareonfirstpreparedstatementcall-method-sqlserverconnection.md)|Retourne la valeur de la propriété de connexion **enablePrepareOnFirstPreparedStatementCall**.|
|[getHoldability](../../../connect/jdbc/reference/getholdability-method-sqlserverconnection.md)|Récupère la capacité actuelle de mise en attente des objets [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) créés à l’aide de cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverconnection.md)|Récupère un objet [SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md) contenant des métadonnées sur la base de données pour laquelle cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) représente une connexion.|  
|[getServerPreparedStatementDiscardThreshold](../../../connect/jdbc/reference/getserverpreparedstatementdiscardthreshold-method-sqlserverconnection.md)|Retourne la valeur de la propriété de connexion **serverPreparedStatementDiscardThreshold**.|  
|[getStatementHandleCacheEntryCount](../../../connect/jdbc/reference/getstatementhandlecacheentrycount-method-sqlserverconnection.md)|Retourne le nombre actuel de handles d’instruction préparée regroupés.|  
|[getStatementPoolingCacheSize](../../../connect/jdbc/reference/getstatementpoolingcachesize-method-sqlserverconnection.md)|Retourne la taille du cache d’instructions préparées pour cette connexion.|  
|[getTransactionIsolation](../../../connect/jdbc/reference/gettransactionisolation-method-sqlserverconnection.md)|Récupère le niveau d’isolation actuel de la transaction de cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[getTypeMap](../../../connect/jdbc/reference/gettypemap-method-sqlserverconnection.md)|Récupère l’objet Map associé à cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverconnection.md)|Récupère le premier avertissement signalé par des appels sur cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverconnection.md)|Indique si cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) a été fermé.|  
|[isReadOnly](../../../connect/jdbc/reference/isreadonly-method-sqlserverconnection.md)|Indique si cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) est en mode lecture seule.|  
|[isStatementPoolingEnabled](../../../connect/jdbc/reference/isstatementpoolingenabled-method-sqlserverconnection.md)|Indique si le regroupement d’instructions est activé ou non pour cette connexion.|  
|[isValid](../../../connect/jdbc/reference/isvalid-method-sqlserverconnection.md)|Indique si cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) n’a pas été fermé et est toujours valide.|  
|[nativeSQL](../../../connect/jdbc/reference/nativesql-method-sqlserverconnection.md)|Convertit l'instruction SQL donnée en grammaire SQL native du serveur de base de données.|  
|[prepareCall](../../../connect/jdbc/reference/preparecall-method-sqlserverconnection.md)|Crée un objet [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md) servant à appeler les procédures stockées de base de données.|  
|[prepareStatement](../../../connect/jdbc/reference/preparestatement-method-sqlserverconnection.md)|Crée un objet [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) servant à envoyer des instructions SQL paramétrables à la base de données.|  
|[releaseSavepoint](../../../connect/jdbc/reference/releasesavepoint-method-sqlserverconnection.md)|Supprime l’objet [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) spécifié de la transaction actuelle.|  
|[rollback](../../../connect/jdbc/reference/rollback-method-sqlserverconnection.md)|Annule toutes les modifications effectuées dans la transaction actuelle et libère tous les verrous de base de données actuellement maintenus par cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[setAutoCommit](../../../connect/jdbc/reference/setautocommit-method-sqlserverconnection.md)|Définit le mode de validation automatique actuel de cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) selon l’état donné.|  
|[setCatalog](../../../connect/jdbc/reference/setcatalog-method-sqlserverconnection.md)|Définit le nom de catalogue spécifié pour sélectionner un sous-espace de la base de données de cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) servant d’emplacement de travail.|  
|[setClientInfo](../../../connect/jdbc/reference/setclientinfo-method-sqlserverconnection.md)|Définit la valeur des propriétés d'informations client.|  
|[setDisableStatementPooling](../../../connect/jdbc/reference/setdisablestatementpooling-method-sqlserverconnection.md)|Définit le regroupement d’instructions sur true ou false.|  
|[setEnablePrepareOnFirstPreparedStatementCall](../../../connect/jdbc/reference/setenableprepareonfirstpreparedstatementcall-method-sqlserverconnection.md)|Spécifie la nouvelle valeur de la propriété de connexion **enablePrepareOnFirstPreparedStatementCall**.|  
|[setHoldability](../../../connect/jdbc/reference/setholdability-method-sqlserverconnection.md)|Attribue la capacité de mise en attente donnée aux objets [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) créés à l’aide de cet objet [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md).|  
|[setReadOnly](../../../connect/jdbc/reference/setreadonly-method-sqlserverconnection.md)|Place cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) en mode lecture seule pour indiquer au pilote JDBC qu’il doit activer les optimisations de base de données.|  
|[setSavepoint](../../../connect/jdbc/reference/setsavepoint-method-sqlserverconnection.md)|Crée un point de sauvegarde sans nom dans la transaction actuelle, puis retourne le nouvel objet [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) qui le représente.|  
|[setServerPreparedStatementDiscardThreshold](../../../connect/jdbc/reference/setserverpreparedstatementdiscardthreshold-method-sqlserverconnection.md)|Définit la nouvelle valeur de la propriété de connexion **serverPreparedStatementDiscardThreshold**.|  
|[setStatementPoolingCacheSize](../../../connect/jdbc/reference/setstatementpoolingcachesize-method-sqlserverconnection.md)|Définit la taille du cache d’instructions préparées pour cette connexion.|  
|[setTransactionIsolation](../../../connect/jdbc/reference/settransactionisolation-method-sqlserverconnection.md)|Tente de remplacer le niveau d’isolation de la transaction de cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) par le niveau donné.|  
|[setTypeMap](../../../connect/jdbc/reference/settypemap-method-sqlserverconnection.md)|Installe l’objet TypeMap donné comme mappage de type de cet objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
  
## <a name="inherited-methods"></a>Méthodes héritées  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.lang.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Voir aussi  
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
