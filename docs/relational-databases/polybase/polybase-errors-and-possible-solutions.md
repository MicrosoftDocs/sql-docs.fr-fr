---
title: Erreurs de configuration de PolyBase et solutions possibles
description: Référence PolyBase pour les erreurs et les solutions suggérées.
ms.date: 02/17/2021
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
dev_langs:
- TSQL
- XML
f1_keywords:
- PolyBase, monitoring
- PolyBase, performance monitoring
helpviewer_keywords:
- PolyBase, troubleshooting
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: ''
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-2016'
ms.openlocfilehash: 463b54aefd36e74318331c90cf2c944734f8a5cc
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873326"
---
# <a name="polybase-errors-and-possible-solutions"></a>Erreurs de configuration de PolyBase et solutions possibles

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

Cet article contient des scénarios d’erreur courants et des solutions pour PolyBase.

Pour plus d’informations sur l’analyse et la résolution des problèmes de PolyBase, consultez [Analyser et résoudre des problèmes de PolyBase](polybase-troubleshooting.md).

Pour connaître les emplacements de fichier journal PolyBase courants sous Windows et Linux, consultez [Analyser et résoudre des problèmes de PolyBase](polybase-troubleshooting.md#log-file-locations).


## <a name="error-messages-and-possible-solutions"></a>Messages d’erreur et solutions possibles


### <a name="service-account-change"></a>Modifier le compte de service

Exemple de message d’erreur :

> 107035;Échec de l’autorisation DMS, car [DOMAINE\utilisateur] n’est pas membre du groupe [PdwDataMovementAccess] <BR>
> 107017;En-tête de contrôle DMS non valide :

Cette erreur est probablement due à la modification du compte de service PolyBase. Pour changer les comptes de service pour le moteur PolyBase et le service Mouvement de données PolyBase, désinstallez et réinstallez la fonctionnalité PolyBase.


### <a name="data-movement-service-permissions-errors"></a>Erreurs d’autorisations d'accès au service de déplacement des données

Pour plus d’informations sur le dépannage et la résolution des problèmes d’autorisations avec le service de déplacement des données, consultez [Autorisations de compte de service PolyBase et erreurs courantes observées quand elles sont manquantes](https://techcommunity.microsoft.com/t5/sql-server-support/polybase-service-account-permissions-and-common-errors-observed/ba-p/2112711).


### <a name="windows-authentication-failure"></a>Échec de l’authentification Windows

Pour plus d’informations sur le dépannage et la résolution des problèmes d’autorisations associés à une défaillance de l’authentification Windows, consultez [Autorisations de compte de service PolyBase et erreurs courantes observées quand elles sont manquantes](https://techcommunity.microsoft.com/t5/sql-server-support/polybase-service-account-permissions-and-common-errors-observed/ba-p/2112711).


### <a name="cannot-execute-the-query-remote-query"></a>Impossible d’exécuter la requête « Requête distante » 

Exemple de message d’erreur :

> Message 7320, niveau 16, état 110, ligne 14<BR>
> Impossible d'exécuter la requête « Requête distante » sur le fournisseur OLE DB « SQLNCLI11 » du serveur lié « (null) ». Requête abandonnée : le seuil de rejet maximal (0 ligne) a été atteint durant la lecture d'une source externe : 1 ligne rejetée sur un total de 1 ligne traitée.
(/nation/sensors.ldjson.txt)Column ordinal: 0, Expected data type: INT, Offending value: {"id":"S2740036465E2B","time":"2016-02-26T16:59:02.9300000Z","temp":23.3,"hum":0.77,"wind":17,"press":1032,"loc":[-76.90914996169623,38.8929314364726]} (Column Conversion Error), Erreur : Erreur de conversion du type de données NVARCHAR en INT.

Gardez à l’esprit qu’il peut y avoir des dérivations de cette erreur. Le nom du premier fichier rejeté apparaît dans SQL Server Management Studio (SSMS) avec des valeurs ou des types de données incriminés.

**Raison possible :**  
Cette erreur se produit parce que chaque fichier a un schéma différent. La table externe PolyBase DDL en cas de pointage vers un répertoire lit de manière récursive tous les fichiers de ce répertoire. En cas d’incompatibilité de colonne ou de type de données, cette erreur peut apparaître dans SSMS.

**Solution possible :**  
Si les données de chaque table se composent d’un seul fichier, utilisez le nom de fichier dans la section EMPLACEMENT préfixé par le répertoire des fichiers externes. S’il existe plusieurs fichiers par table, placez chaque ensemble de fichiers dans différents répertoires dans le stockage Blob Azure. EMPLACEMENT du point vers le répertoire au lieu d’un fichier particulier. Cette solution est recommandée.

**Exemple :**  

```sqlsyntax
Create External Table foo
(col1 int)WITH (LOCATION='/bar/foobar.txt',DATA_SOURCE…); OR
Create External Table foo
(col1 int) WITH (LOCATION = '/bar/', DATA_SOURCE…);
```


### <a name="kerberos-support"></a>Prise en charge Kerberos

SQL Server est configurée pour accéder à un cluster Hadoop pris en charge. La sécurité Kerberos n’est pas appliquée dans le cluster Hadoop.

La sélection de la table externe renvoie l’erreur suivante :

> Message 105019, niveau 16, état 1, ligne 55<BR>
> Échec de l’accès à la TABLE EXTERNE en raison d’une erreur interne : « exception Java levée lors de l’appel à HdfsBridge_Connect : Erreur [impossible d’instancier LoginClass] s’est produite lors de l’accès au fichier externe. »<BR>
> Message 7320, niveau 16, état 110, ligne 55<BR>
> Impossible d'exécuter la requête « Requête distante » sur le fournisseur OLE DB « SQLNCLI11 » du serveur lié « (null) ». Échec de l’accès à la TABLE EXTERNE en raison d’une erreur interne : « exception Java levée lors de l’appel à HdfsBridge_Connect : Erreur [impossible d’instancier LoginClass] s’est produite lors de l’accès au fichier externe. »

L’interrogation du journal du serveur DWEngine affiche l’erreur suivante :

> [Thread:16432] [EngineInstrumentation:EngineQueryErrorEvent] (Erreur, Élevé) :<BR>
> Échec de l’accès à la TABLE EXTERNE en raison d’une erreur interne : « exception Java levée lors de l’appel à HdfsBridge_Connect : Erreur [com.microsoft.polybase.client.KerberosSecureLogin] s’est produite lors de l’accès au fichier externe. »
Microsoft.SqlServer.DataWarehouse.Common.ErrorHandling.MppSqlException : échec d’accès à la TABLE EXTERNE en raison d’une erreur externe : « exception Java levée lors de l’appel à HdfsBridge_Connect : Erreur [com.microsoft.polybase.client.KerberosSecureLogin] s’est produite lors de l’accès au fichier externe. » ---> Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsAccessException : exception Java levée lors de l’appel à HdfsBridge_Connect : Erreur [com.microsoft.polybase.client.KerberosSecureLogin] s’est produite lors de l’accès au fichier externe.

**Raison possible :**  
Kerberos n’est pas activé dans le cluster Hadoop, mais la sécurité Kerberos est activée dans core-site.xml, yarn-site.xml ou hdfs-site.xml situé par défaut sous Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\MSSQL\Binn\Polybase\Hadoop\conf. Dans Linux, les fichiers se trouvent par défaut dans /var/opt/mssql/binn/polybase/hadoop/conf/.

**Solution possible :**  
Commentez les informations de sécurité Kerberos des fichiers mentionnés ci-dessus.

Pour plus d’informations sur la résolution des problèmes liés à PolyBase et Kerberos, consultez [Résoudre des problèmes de connectivité de PolyBase Kerberos](polybase-troubleshoot-connectivity.md).

### <a name="internal-query-processor-error"></a>Erreur interne du processeur de requêtes

L’interrogation d’une table externe retourne l’erreur suivante :

> Message 8680, niveau 17, état 5, ligne 118<BR>
> Erreur interne du processeur de requêtes : le processeur de requêtes a rencontré une erreur inattendue pendant le traitement d’une phase de la requête distante.

Le journal du serveur DWEngine contient les messages suivants :

> [Thread:5216] [ControlNodeMessenger:ErrorEvent] (Error, High) : ***** Le système DMS a des nœuds déconnectés :<BR>
> [Thread:5216] [ControlNodeMessenger:ErrorEvent] (Error, High) : ***** Le système DMS a des nœuds déconnectés :<BR>
> [Thread:5216] [ControlNodeMessenger:ErrorEvent] (Error, High) : ***** Le système DMS a des nœuds déconnectés :<BR>

**Raison possible :**  
Cette erreur peut être due au fait que SQL Server n’a pas été redémarré après la configuration de PolyBase.

**Solution possible :**  
Redémarrez SQL Server. Consultez le journal du serveur DWEngine pour vérifier qu’il n’y a aucune déconnexion DMS après le redémarrage.


### <a name="user-needed-for-hdfs-access"></a>Utilisateur requis pour l’accès à HDFS

**Scénario :**  
SQL Server est connecté à un cluster Hadoop non sécurisé (Kerberos n’est pas activé). PolyBase est configuré pour pousser le calcul vers le cluster Hadoop.

**Exemple de requête :**  

```sql
select count(*) from foo WITH (FORCE EXTERNALPUSHDOWN);
```

Si un message d'erreur semblable au suivant s'affiche : 

> Message 105019, niveau 16, état 1, ligne 1<BR>
> Échec de l’accès à la TABLE EXTERNE en raison d’une erreur interne : «exception Java levée lors de l’appel à JobSubmitter_PollJobStatus : erreur [Java.net.ConnectException : échec de l’appel de big1506sql2016/172.16.1.4 à 0.0.0.0:10020 sur l’exception de connexion : java.net ConnectException : connexion refusée : aucune information supplémentaire ; Pour plus d’informations, consultez :  http://wiki.apache.org/hadoop/ConnectionRefused ] s’est produit lors de l’accès à un fichier externe.»<BR>
> Fournisseur OLE DB « SQLNCLI11 » du serveur lié « (null) » a retourné le message « Erreur non spécifiée ».<BR>
> Message 7421, niveau 16, état 2, ligne 1<BR>
> Impossible de récupérer (fetch) l'ensemble de lignes du fournisseur OLE DB « SQLNCLI11 » pour le serveur lié « (null) ». .<BR>

Erreur de journal Yarn Hadoop :
> Échec de l’installation du travail : org.apache.hadoop.security.AccessControlException: autorisation refusée : user=pdw_user, access=WRITE, inode="/user":hdfs:hdfs:drwxr-xr-x sur org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkFsPermission(FSPermissionChecker.java:265) sur org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:251) sur org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:232) org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:176) sur org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkPermission(FSNamesystem.java:5525)

**Raison possible :**  
Si Kerberos est désactivé, PolyBase utilise pdw_user en tant qu’utilisateur pour accéder à HDFS et envoyer des tâches MapReduce.

**Solution possible :**  
Créez pdw_user sur Hadoop et accordez-lui des autorisations suffisantes pour les répertoires utilisés pendant le traitement de mapreduce. Assurez-vous également que pdw_user est le propriétaire du répertoire HDFS /user/pdw_user.

Vous trouverez ci-dessous un exemple de création d’un répertoire de base et d’affectation d’autorisations pour pdw_user :

```console
sudo -u hdfs hadoop fs -mkdir /user/pdw_user
sudo -u hdfs hadoop fs -chown pdw_user /user/pdw_user
```

Après cela, assurez-vous que pdw_user dispose des autorisations de lecture, d’écriture et d’exécution sur le répertoire /user/pdw_user. Assurez-vous que le répertoire /tmp dispose des autorisations 777.

Pour plus d’informations sur la résolution des problèmes liés à PolyBase et Kerberos, consultez [Résoudre des problèmes de connectivité de PolyBase Kerberos](polybase-troubleshoot-connectivity.md).

### <a name="java-memory-error-due-to-utf-8"></a>Erreur de mémoire Java en raison d’UTF-8

**Scénario :**  
SQL Server Polybase est configuré avec un cluster Hadoop ou un stockage Blob Azure. Toute requête Select échoue avec l’erreur suivante :

> Message 106000, niveau 16, état 1, ligne 1<BR>
> Espace de tas Java<BR>

**Raison possible :**  
Une entrée illégale peut provoquer une erreur de mémoire insuffisante dans Java. Le fichier ne peut pas être au format UTF-8. DMS tente de lire l’ensemble du fichier sous la forme d’une ligne, car il ne peut pas décoder le délimiteur de ligne et s’exécute en erreur de l’espace du segment de mémoire Java.

**Solution possible :**  
Convertissez le fichier au format UTF-8, car PolyBase requiert actuellement le format UTF-8 pour les fichiers délimités par du texte.

### <a name="hadoop-connectivity-configuration"></a>Configuration de la connectivité Hadoop

La configuration de SQL Server PolyBase pour la connexion au stockage Blob Azure retourne le message d’erreur suivant dans SQL Server :

> Message 105019, niveau 16, état 1, ligne 74<BR>
> Échec de l’accès à la TABLE EXTERNE en raison d’une erreur interne : « exception Java levée lors de l’appel à HdfsBridge_Connect : Erreur [Aucun FileSystem pour le schéma : wasbs] s’est produite lors de l’accès au fichier externe. »<BR>

**Raison possible :**  
La connectivité Hadoop n’est pas définie sur la valeur de configuration pour l’accès au stockage Blob Azure.

**Solution possible :**  
Définissez la connectivité Hadoop sur une valeur (de préférence 7) qui prend en charge le stockage Blob Azure et redémarrez SQL Server. Pour obtenir la liste des valeurs de connectivité et des types pris en charge, consultez [Configuration de la connectivité PolyBase](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md#arguments).


### <a name="create-table-as-select-error"></a>Erreur Create Table As Select

**Scénario :**  
La tentative d’exportation de données vers le stockage Blob Azure ou le système de fichiers Hadoop à l’aide de PolyBase avec la syntaxe CREATE EXTERNAL TABLE AS SELECT (CETAS) à partir de SQL Server échoue avec le message d’erreur suivant :

> Message 156, niveau 15, état 1, ligne 177<BR>
> Syntaxe incorrecte près du mot clé « WITH ».<BR>
> Message 319, niveau 15, état 1, ligne 177<BR>
> Syntaxe incorrecte près du mot clé 'with'. Si l'instruction est une expression de table commune, une clause xmlnamespaces ou une clause de contexte de suivi des modifications, l'instruction précédente doit se terminer par un point-virgule.<BR>

**Raison possible :**  
En cas d’exportation de données vers Hadoop ou le Stockage Blob Azure via PolyBase, seules les données sont exportées, et non les noms de colonnes (métadonnées), comme le définit la commande CREATE EXTERNAL TABLE.

**Solution possible :**  
Créez d’abord la table externe, puis utilisez l’option INSERT INTO SELECT pour exporter vers l’emplacement externe. Pour obtenir un exemple de code, consultez [Scénarios de requête PolyBase](polybase-queries.md#export-data).


### <a name="create-external-table-from-azure-blob-storage-fails"></a>Échec de la création d’une table externe à partir du stockage Blob Azure

**Scénario :**  
DW SQL est configuré pour importer des données à partir du stockage Blob Azure. La création d’une table externe échoue avec le message suivant.

> Message 105019, niveau 16, état 1, ligne 34<BR>
> Échec de l’accès à la TABLE externe en raison d’une erreur interne : « exception Java levée lors de l’appel à HdfsBridge_IsDirExist. Exception Java message:com.microsoft.azure.storage.StorageException : échec du serveur pour authentifier la requête. Assurez-vous que la valeur de l’en-tête d’autorisation est correctement formée, y compris la signature. : erreur [com.microsoft.azure.storage.StorageException : le serveur n’a pas pu authentifier la requête. Assurez-vous que la valeur de l’en-tête d’autorisation est correctement formée, y compris la signature.] s’est produite lors de l’accès au fichier externe. »<BR>

**Raison possible :**  
Une clé de stockage Azure incorrecte a été utilisée pour créer les informations d’identification délimitées à la base de données.

**Solution possible :**  
Supprimez tous les objets associés (par ex., source de données, format de fichier), puis déposez et recréez les informations d’identification étendues à la base de données avec la clé de stockage appropriée.


### <a name="kerberos-configuration-capitalization"></a>Mise en majuscules de la configuration Kerberos

**Scénario :**  
SQL Server est configuré avec le cluster Cloudera activé pour Kerberos. SQL Server a été redémarré une fois toutes les modifications de configuration effectuées. Le moteur PolyBase et les services Mouvement de données PolyBase s’exécutent après le redémarrage. Les messages d’erreur suivant sont retournés :

La source de données configurée sans emplacement du suivi des tâches :  
> org.apache.hadoop.fs.FileSystem: le fournisseur org.apache.hadoop.fs.viewfs.ViewFileSystem ne peut pas être instancié

Source de données configurée sans emplacement du suivi des tâches :  
> Erreur [impossible d’accéder au domaine Kerberos] lors de l’accès au fichier externe

**Raison possible :**  
La valeur de la propriété « hadoop.security.authentication » indique Kerberos dans le Coresite.xml.

**Solution possible :**  
La propriété « hadoop.security.authentication » de Coresite.xml doit être KERBEROS (tout en majuscules) comme valeur. 

Pour plus d’informations sur la résolution des problèmes liés à PolyBase et Kerberos, consultez [Résoudre des problèmes de connectivité de PolyBase Kerberos](polybase-troubleshoot-connectivity.md).

### <a name="mapred-sitexml-missing-needed-values"></a>Valeurs manquantes nécessaires pour Mapred-site.xml

**Scénario :**  
SQL Server ou APS est configuré avec un cluster HDP pris en charge. Les requêtes qui ne requièrent pas un travail de pushdown, mais échouent avec le message suivant lorsque le conseil « FORCER LE PUSHDOWN » est utilisé avec les messages d’erreur suivants :

> Message 7320, niveau 16, état 110, ligne 35<BR>
> Impossible d'exécuter la requête « Requête distante » sur le fournisseur OLE DB « SQLNCLI11 » du serveur lié « (null) ». Échec de l’accès à la TABLE EXTERNE en raison d’une erreur interne : « exception Java levée lors de l’appel à JobSubmitter_PollJobStatus : erreur [org.apache.hadoop.ipc.RemoteException(java.lang.NullPointerException) : java.lang.NullPointerException<BR>
> sur org.apache.hadoop.mapreduce.v2.hs.HistoryClientService$HSClientProtocolHandler.getTaskAttemptCompletionEvents(HistoryClientService.java:277)<BR>
> sur org.apache.hadoop.mapreduce.v2.api.impl.pb.service.MRClientProtocolPBServiceImpl.getTaskAttemptCompletionEvents(MRClientProtocolPBServiceImpl.java:173)<BR>
> sur org.apache.hadoop.yarn.proto.MRClientProtocol$MRClientProtocolService$2.callBlockingMethod(MRClientProtocol.java:283)<BR>
> sur org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:619)<BR>
> sur org.apache.hadoop.ipc.RPC$Server.call(RPC.java:962)<BR>
> sur org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2127)<BR>
> sur org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2123)<BR>
> sur java.security.AccessController.doPrivileged(Native Method)<BR>
> sur javax.security.auth.Subject.doAs(Subject.java:415)<BR>
> sur org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1671)<BR>
> sur org.apache.hadoop.ipc.Server$Handler.run(Server.java:2121)<BR>
> ] s’est produite lors de l’accès au fichier externe. »<BR>

**Raison possible :**  
Des valeurs nécessaires Mapred-site.xml manquent pour vérifier les résultats intermédiaires et finaux.

**Solution possible :**  
Ajoutez les propriétés suivantes et associez les valeurs correctes telles qu’elles s’affichent sur Ambari dans le fichier mapred-site.xml sur SQL Server. 

```xml
<property>
<name>yarn.app.mapreduce.am.staging-dir</name>
<value>/user</value>
</property>
<property>
<name>mapreduce.jobhistory.done-dir</name>
<value>/mr-history/done</value>
</property>
<property>
<name>mapreduce.jobhistory.intermediate-done-dir</name>
<value>/mr-history/tmp</value>
</property>
```

### <a name="configuring-access-by-hostname"></a>Configuration de l’accès par nom d’hôte 

**Scénario :**  
SQL Server est configuré pour accéder à un cluster Hadoop pris en charge. La création d’une table externe retourne l’une des erreurs suivantes :

> Impossible d'exécuter la requête « Requête distante » sur le fournisseur OLE DB « SQLNCLI11 » du serveur lié « (null) ». 110802;Une erreur DMS interne s'est produite et a entraîné l'échec de l'opération. Détails : Exception : Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.DmsSqlNativeException, Message: SqlNativeBufferReader.Run, error in OdbcExecuteQuery: SqlState: 42000, NativeError: 8680, 'Error calling: SQLExecDirect(this->GetHstmt(), (SQLWCHAR *)statementText, SQL_NTS), SQL return code: -1 | Info erreur SQL : SrvrMsgState: 26, SrvrSeverity: 17,  Error <1>: ErrorMsg: [Microsoft][ODBC Driver 13 pour SQL Server][SQL Server]Erreur de processeur de requêtes interne : le processeur de requêtes a rencontré une erreur inattendue pendant le traitement d’une phase de la requête distante. | Erreur lors de l’appel : pReadConn->ExecuteQuery(statementText, bufferFormat) | state: FFFF, number: 24, active connections: 8', Connection String: Driver={pdwodbc};APP=RCSmall-DmsNativeReader:WAD1D16HD2001\mpdwsvc (3600)-ODBC-PoolId1433;Trusted_Connection=yes;AutoTranslate=no;Server=\\.\pipe\sql\query<BR>

> [Thread:30544] [AbstractReaderWorker:ErrorEvent] (Error, High): QueryId QID2433 PlanId 6c3a4551-e54c-4c06-a5ed-a8733edac691 StepId 7 :<BR>
> Impossible d’obtenir le bloc : BP-1726738607-192.168.225.121-1443123675290:blk_1159687047_86196509 file=/user/hive/warehouse/u_data/000000_0<BR>
> Microsoft.SqlServer.DataWarehouse.Common.ErrorHandling.MppSqlException: Impossible d’obtenir le bloc : BP-1726738607-192.168.225.121-1443123675290:blk_1159687047_86196509 file=/user/hive/warehouse/u_data/000000_0<BR>
> sur Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsBridgeReadAccess.Read(MemoryBuffer buffer, Boolean& isDone)<BR>
> sur Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.DataReader.ExternalMoveBufferReader.Read()<BR>
> sur Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.ExternalMoveReaderWorker.ReadAndSendData()<BR>
> sur Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.ExternalMoveReaderWorker.Execute(Object status)<BR>

**Raison possible :**  
Ce message d’erreur peut s’afficher lorsque le cluster Hadoop est configuré dans une configuration où les nœuds de données sont uniquement accessibles à l’extérieur du cluster à l’aide du nom d’hôte et non de l’adresse IP.

**Solution possible :**  
Ajoutez ce qui suit au fichier hdfs-site.xml du côté client (SQL Server). Cette configuration force le nœud du nom à retourner un URI pour les nœuds de données avec le nom d’hôte au lieu de l’adresse IP interne.

```xml
<property>
<name>dfs.client.use.datanode.hostname</name>
<value>true</value>
</property>
```

### <a name="folder-organization-forces-excess-memory-overhead"></a>L’organisation des dossiers force une surcharge de mémoire excessive

**Scénario :**  
SQL Server exécute une requête PolyBase sur un répertoire contenant un grand nombre de fichiers (>30 000 fichiers sous le chemin d’accès de répertoire de manière récursive) et l’un des messages d’erreur suivants est retourné :

> Message 105019, niveau 16, état 1, ligne 1<BR>
> Échec de l’accès à la TABLE EXTERNE en raison d’une erreur interne : « exception Java levée lors de l’appel à HdfsBridge_GetFileNameByIndex. Message d’exception Java : dépassement de la limite de surcharge du nettoyage de la mémoire : Erreur [dépassement de la limite de surcharge du nettoyage de la mémoire] s’est produit lors de l’accès au fichier externe. »<BR>

> Message 105019, niveau 16, état 1, ligne 1<BR>
> Échec de l’accès à la TABLE EXTERNE en raison d’une erreur interne : « exception Java levée lors de l’appel à HdfsBridge_GetDirectoryFiles. Message d’exception Java : espace du tas Java : erreur [espace du tas Java] lors de l’accès au fichier externe. »

**Raison possible :**  
Lors du traitement d’un chemin d’accès, PolyBase énumère tous les fichiers sous ce chemin d’accès et une surcharge de mémoire fixe est associée à la structure de données utilisée pour représenter les fichiers. Avec un grand nombre de fichiers, cette surcharge devient visible et peut finalement consommer toute la mémoire disponible pour la Machine virtuelle Java.

**Solution possible :**  
Réorganisez les données dans plusieurs répertoires afin que chaque répertoire contienne un sous-ensemble de fichiers, puis décomposez la requête en plusieurs, qui opèrent sur une partie du chemin d’accès d’origine à la fois et matérialisent les tables en tant que tables SQL Server (avant de les joindre).

Exemple : supposons que les données de la table externe se trouvent à l’emplacement suivant : Orders/file1.txt,..., file30K.txt.

Modifiez la disposition afin que les données soient disposées dans une structure de partition de fichier conventionnelle dans Orders/*yyyy*/*mm*/*dd*/file1.txt.
Pointez votre table externe vers un chemin d’accès au répertoire inférieur, par exemple mois (mm) ou jour (jj), puis importez les fichiers dans des tables SQL Server en plusieurs parties, puis ajoutez-les dans le cadre d’une table.
Même si vous avez commencé par la structure de répertoires appropriée, suivez l’étape #2 pour pouvoir utiliser ce nombre de fichiers sans manquer de mémoire JVM.


### <a name="unexpected-characters-in-configuration-files"></a>Caractères inattendus dans les fichiers config

**Scénario :**  
Configuration de SQL Server ou d’APS avec un cluster Hadoop qui implique la modification de yarn-site.xml, hdfs-site.xml et d’autres fichiers config. Le message d’erreur SQL Server suivant est observé :

> Message 105019, niveau 16, état 1, ligne 1<BR>
> Microsoft.SqlServer.DataWarehouse.Common.ErrorHandling.MppSqlException : échec de l’accès à la TABLE EXTERNE en raison d’une erreur interne : « extension Java levée lors de l’appel à HdfsBridge_Connect. Message d’exception Java : com.sun.org.apache.xerces.internal.impl.io.MalformedByteSequenceException: Invalid byte 1 of 1-byte UTF-8 sequence.: Error [com.sun.org.apache.xerces.internal.impl.io.MalformedByteSequenceException: Invalid byte 1 of 1-byte UTF-8 sequence.] s’est produit lors de l’accès au fichier ---><BR>

**Raison possible :**  
Cela peut se produire si vous avez copié et collé du texte dans des fichiers config à partir d’un site web ou d’une fenêtre de conversation. Il est possible que les caractères indésirables/non imprimables se trouvent dans les fichiers config.

**Solution possible :**  
Ouvrez les fichiers dans un autre éditeur de texte (autre que le bloc-notes) et recherchez ces caractères pour les éliminer. Redémarrez les services nécessaires. 


## <a name="see-also"></a>Voir aussi

[Superviser et dépanner PolyBase](polybase-troubleshooting.md)  
[Résoudre les problèmes de connectivité de PolyBase Kerberos](polybase-troubleshoot-connectivity.md)  

