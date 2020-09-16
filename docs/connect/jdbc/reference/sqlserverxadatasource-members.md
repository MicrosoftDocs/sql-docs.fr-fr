---
description: Membres de SQLServerXADataSource
title: Membres de SQLServerXADataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 04178645-915f-4569-8907-d45e299bbe7d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ed2d2515dc742eca0ece6036dbf0b06dd9a9530c
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88458199"
---
# <a name="sqlserverxadatasource-members"></a>Membres de SQLServerXADataSource
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Les tableaux suivants listent les membres qui sont exposés par la classe [SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-class.md).  
  
## <a name="constructors"></a>Constructeurs  
  
|Nom|Description|  
|----------|-----------------|  
|[SQLServerXADataSource ()](../../../connect/jdbc/reference/sqlserverxadatasource-constructor.md)|Initialise une nouvelle instance de la classe [SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-class.md).|  
  
## <a name="fields"></a>Champs  
 Aucun.  
  
## <a name="inherited-fields"></a>Champs hérités  
 Aucun.  
  
## <a name="methods"></a>Méthodes  
  
|Nom|Description|  
|----------|-----------------|  
|[getApplicationIntent](../../../connect/jdbc/reference/getapplicationintent-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne la valeur de la propriété de connexion **applicationIntent**.|  
|[getApplicationName](../../../connect/jdbc/reference/getapplicationname-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne le nom de l’application.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Tente d’établir une connexion avec la source de données représentée par cet objet DataSource.|  
|[getDatabaseName](../../../connect/jdbc/reference/getdatabasename-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne le nom de la base de données.|  
|[getFailoverPartner](../../../connect/jdbc/reference/getfailoverpartner-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne le nom du serveur de basculement utilisé dans une configuration de mise en miroir de bases de données.|  
|[getInstanceName](../../../connect/jdbc/reference/getinstancename-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne le nom de l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getLastUpdateCount](../../../connect/jdbc/reference/getlastupdatecount-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne une valeur **booléenne** qui indique si la propriété lastUpdateCount est activée.|  
|[getLockTimeout](../../../connect/jdbc/reference/getlocktimeout-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne une valeur **int** qui indique le nombre de millisecondes pendant lesquelles la base de données doit attendre avant de signaler l’expiration du délai d’un verrou.|  
|[getLoginTimeout](../../../connect/jdbc/reference/getlogintimeout-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne le nombre de secondes pendant lesquelles cet objet DataSource doit patienter lors de la tentative de connexion.|  
|[getLogWriter](../../../connect/jdbc/reference/getlogwriter-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne un flux de sortie de caractères à utiliser pour tous les messages de journalisation et de suivi.|  
|[getMultiSubnetFailover](../../../connect/jdbc/reference/getmultisubnetfailover-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Récupère la valeur de la propriété de connexion **multiSubnetFailover**.|  
|[getPooledConnection](../../../connect/jdbc/reference/getpooledconnection-method-sqlserverconnectionpooldatasource.md)|(héritée de [SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md)) Tente d’établir une connexion de base de données physique pouvant être utilisée en tant que connexion regroupée.|  
|[getPortNumber](../../../connect/jdbc/reference/getportnumber-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne le numéro de port actuel utilisé pour communiquer avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getReference](../../../connect/jdbc/reference/getreference-method-sqlserverxadatasource.md)|Retourne une référence à cet objet [SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-class.md).|  
|[getSelectMethod](../../../connect/jdbc/reference/getselectmethod-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne le type de curseur par défaut utilisé pour tous les jeux de résultats créés à l’aide de cet objet DataSource.|  
|[getSendStringParametersAsUnicode](../../../connect/jdbc/reference/getsendstringparametersasunicode-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne une valeur **booléenne** qui indique si l’envoi de paramètres de **chaîne** au serveur au format UNICODE est activé.|  
|[getServerName](../../../connect/jdbc/reference/getservername-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne le nom de l’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne l’URL utilisée pour la connexion à la source de données.|  
|[getUser](../../../connect/jdbc/reference/getuser-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne le nom d’utilisateur utilisé pour la connexion à la source de données.|  
|[getWorkstationID](../../../connect/jdbc/reference/getworkstationid-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne le nom de l’ordinateur client utilisé pour la connexion à la source de données.|  
|[getXAConnection](../../../connect/jdbc/reference/getxaconnection-method-sqlserverxadatasource.md)|Essaie d'établir une connexion de base de données physique qui peut être utilisée dans une transaction distribuée.|  
|[getXopenStates](../../../connect/jdbc/reference/getxopenstates-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Retourne une valeur **booléenne** qui indique si la conversion d’états SQL en états conformes à XOPEN est activée.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverxadatasource.md)|Indique si cet objet est un wrapper pour l'interface spécifiée.|  
|[setApplicationIntent](../../../connect/jdbc/reference/setapplicationintent-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit la valeur de la propriété de connexion **applicationIntent**.|  
|[setApplicationName](../../../connect/jdbc/reference/setapplicationname-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit le nom de l’application.|  
|[setAuthenticationSceme](../../../connect/jdbc/reference/setauthenticationscheme-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Indique le type de sécurité intégrée que votre application doit utiliser.|  
|[setDatabaseName](../../../connect/jdbc/reference/setdatabasename-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit le nom de la base de données à laquelle se connecter.|  
|[setDescription](../../../connect/jdbc/reference/setdescription-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit la description de la source de données.|  
|[setFailoverPartner](../../../connect/jdbc/reference/setfailoverpartner-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit le nom du serveur de basculement utilisé dans une configuration de mise en miroir de bases de données.|  
|[setInstanceName](../../../connect/jdbc/reference/setinstancename-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit le nom de l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setIntegratedSecurity](../../../connect/jdbc/reference/setintegratedsecurity-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit une valeur **booléenne** qui indique si la propriété integratedSecurity est activée.|  
|[setLastUpdateCount](../../../connect/jdbc/reference/setlastupdatecount-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit une valeur **booléenne** qui indique si la propriété lastUpdateCount est activée.|  
|[setLockTimeout](../../../connect/jdbc/reference/setlocktimeout-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit une valeur **int** qui indique le nombre de millisecondes d’attente avant que la base de données signale l’expiration du délai d’un verrou.|  
|[setLoginTimeout](../../../connect/jdbc/reference/setlogintimeout-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit le nombre de secondes pendant lesquelles cet objet DataSource doit attendre pendant la tentative de connexion.|  
|[setLogWriter](../../../connect/jdbc/reference/setlogwriter-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit un flux de sortie de caractères à utiliser pour tous les messages de journalisation et de suivi.|  
|[setMultiSubnetFailover](../../../connect/jdbc/reference/setmultisubnetfailover-method-sqlserverdatasource.md)|(Héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit la valeur de la propriété de connexion **multiSubnetFailover**.|  
|[setPassword](../../../connect/jdbc/reference/setpassword-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit le mot de passe à utiliser pour la connexion à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setPortNumber](../../../connect/jdbc/reference/setportnumber-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit le numéro de port à utiliser pour communiquer avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setSelectMethod](../../../connect/jdbc/reference/setselectmethod-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit le type de curseur par défaut utilisé pour tous les jeux de résultats créés à l’aide de cet objet DataSource.|  
|[setSendStringParametersAsUnicode](../../../connect/jdbc/reference/setsendstringparametersasunicode-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit une valeur **booléenne** qui indique si l’envoi de paramètres de **chaîne** au serveur au format UNICODE est activé.|  
|[setServerName](../../../connect/jdbc/reference/setservername-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit le nom de l’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit l’URL utilisée pour la connexion à la source de données.|  
|[setUser](../../../connect/jdbc/reference/setuser-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit le nom d’utilisateur utilisé pour la connexion à la source de données.|  
|[setWorkstationID](../../../connect/jdbc/reference/setworkstationid-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit le nom de l’ordinateur client utilisé pour la connexion à la source de données.|  
|[setXopenStates](../../../connect/jdbc/reference/setxopenstates-method-sqlserverdatasource.md)|(héritée de [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Définit une valeur **booléenne** qui indique si la conversion d’états SQL en états conformes à XOPEN est activée.|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverxadatasource.md)|Retourne un objet qui implémente l’interface spécifiée, afin d’autoriser l’accès aux méthodes propres au [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].|  
  
## <a name="inherited-methods"></a>Méthodes héritées  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerConnectionPoolDataSource|getPooledConnection|  
|com.microsoft.sqlserver.jdbc.SQLServerDataSource|getApplicationName, getConnection, getDatabaseName, getDescription, getFailoverPartner, getInstanceName, getLastUpdateCount, getLockTimeout, getLoginTimeout, getLogWriter, getPortNumber, getSelectMethod, getSendStringParametersAsUnicode, getServerName, getURL, getUser, getWorkstationID, getXopenStates, setApplicationName, setDatabaseName, setDescription, setFailoverPartner, setInstanceName, setIntegratedSecurity, setLastUpdateCount, setLockTimeout, setLoginTimeout, setLogWriter, setPassword, setPortNumber, setSelectMethod, setSendStringParametersAsUnicode, setServerName, setURL, setUser, setWorkstationID, setXopenStates|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
|javax.sql.XADataSource|getLoginTimeout, getLogWriter, setLoginTimeout, setLogWriter|  
|javax.sql.ConnectionPoolDataSource|getLoginTimeout, getLogWriter, setLoginTimeout, setLogWriter|  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerXADataSource, classe](../../../connect/jdbc/reference/sqlserverxadatasource-class.md)  
  
  
