---
title: Résoudre les problèmes de connectivité de PolyBase Kerberos | Microsoft Docs
description: Pour résoudre les problèmes d’authentification de PolyBase avec un cluster Hadoop sécurisé par Kerberos, vous pouvez utiliser les diagnostics interactifs intégrés à PolyBase.
author: alazad-msft
ms.author: alazad
ms.reviewer: mikeray
ms.technology: polybase
ms.devlang: ''
ms.topic: conceptual
ms.date: 10/02/2019
ms.prod: sql
ms.prod_service: polybase, sql-data-warehouse, pdw
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 0734f0a8163486ab46ef8143966a24adee35194d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079420"
---
# <a name="troubleshoot-polybase-kerberos-connectivity"></a>Résoudre les problèmes de connectivité de PolyBase Kerberos

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Vous pouvez utiliser des diagnostics interactifs qui ont été intégrés à PolyBase pour résoudre les problèmes d’authentification lors de l’utilisation de PolyBase dans un cluster Hadoop sécurisé par Kerberos. 

Cet article sert de guide tout le long du processus de débogage de tels problèmes en tirant parti de ces diagnostics intégrés.

> [!TIP]
> Au lieu de suivre les étapes de ce guide, vous pouvez choisir d’exécuter [HDFS Kerberos Tester](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/hdfs-kerberos-tester) pour dépanner les connexions Kerberos HDFS pour PolyBase, quand vous rencontrez un échec de Kerberos HDFS lors de la création d’une table externe dans un cluster HDFS sécurisé par Kerberos.
> Cet outil vous aidera à déterminer les problèmes non SQL Server, afin de vous aider à vous concentrer sur la résolution des problèmes d’installation de Kerberos HDFS, à savoir l’identification des problèmes liés à des erreurs de configuration de nom d’utilisateur/mot de passe et les erreurs de configuration de l’installation de cluster Kerberos.      
> Cet outil est complètement indépendant de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il est disponible en tant que Jupyter Notebook et nécessite Azure Data Studio.

## <a name="prerequisites"></a>Prérequis

1. [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] RTM CU6 / [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU3 / [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] ou version ultérieure avec PolyBase installé
1. Un cluster Hadoop (Cloudera ou Hortonworks) sécurisé avec Kerberos (Active Directory ou MIT)

## <a name="introduction"></a>Introduction
Vous devez tout d’abord avoir quelques notions élémentaires du protocole Kerberos. Trois acteurs sont impliqués :

1. Client Kerberos (SQL Server)
1. Ressource sécurisée (HDFS, MR2, YARN, historique des travaux, etc.)
1. Centre de distribution de clés (également appelé contrôleur de domaine dans Active Directory)

Chaque ressource sécurisée de Hadoop est inscrite dans le **centre de distribution de clés (KDC)** avec un **nom de principal du service (SPN)** unique lors de la configuration de Kerberos sur le cluster Hadoop. L’objectif est que le client obtienne un ticket d’utilisateur temporaire, appelé **TGT (Ticket Granting Ticket)** , afin de demander un autre ticket temporaire, appelé **ticket de service**, auprès du centre KDC pour le nom de principal du service spécifique auquel il veut accéder.  

Dans PolyBase, quand une authentification est demandée pour une ressource sécurisée par Kerberos, la négociation suivante, qui implique quatre allers-retours, se produit :

1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se connecte au centre KDC et obtient un ticket TGT pour l’utilisateur. Le ticket TGT est chiffré à l’aide de la clé privée KDC.
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appelle la ressource Hadoop sécurisée, HDFS, et détermine le nom de principal du service pour lequel il a besoin d’un ticket de service.
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] revient au centre KDC, renvoie le ticket TGT et demande un ticket de service pour accéder à cette ressource sécurisée particulière. Le ticket de service est chiffré avec la clé privée du service sécurisé.
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] transfère le ticket de service à Hadoop et est authentifié pour qu’une session soit créée sur ce service.

![SQL Server PolyBase](./media/polybase-sqlserver.png)

Les problèmes d’authentification relèvent de l’une ou plusieurs des quatre étapes ci-dessus. Pour permettre un débogage plus rapide, PolyBase a introduit un outil de diagnostic intégré pour mieux identifier le point de défaillance.

## <a name="troubleshooting"></a>Dépannage

PolyBase comprend les fichiers XML de configuration suivants contenant les propriétés du cluster Hadoop :

- core-site.xml
- hdfs-site.xml
- hive-site.xml
- jaas.conf
- mapred-site.xml
- yarn-site.xml

Ces fichiers se trouvent sous :

`\[System Drive\]:{install path}\{MSSQL##.INSTANCENAME}\MSSQL\Binn\PolyBase\Hadoop\conf`

Par exemple, la valeur par défaut pour [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] est `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\PolyBase\Hadoop\conf`.

Mettez à jour **core-site.xml** en ajoutant les trois propriétés ci-dessous. Définissez les valeurs en fonction de l’environnement :

```xml
<property>
    <name>polybase.kerberos.realm</name>
    <value>CONTOSO.COM</value>
</property>
<property>
    <name>polybase.kerberos.kdchost</name>
    <value>kerberos.contoso.com</value>
</property>
<property>
    <name>hadoop.security.authentication</name>
    <value>KERBEROS</value>
</property>
```
> [!NOTE]
> La valeur de la propriété `polybase.kerberos.realm` doit être toutes en majuscules.

Les autres fichiers XML devront également être mis à jour par la suite si des opérations de poussée sont souhaitées, mais le système de fichiers HDFS doit être au moins accessible avec simplement ce fichier configuré.

Comme l’outil s’exécute indépendamment de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il n’a pas à être en cours d’exécution ni redémarré si des mises à jour sont apportées aux fichiers XML de configuration. Pour exécuter l’outil, exécutez les commandes suivantes sur l’hôte avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installé :

```cmd
> cd C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\PolyBase  
> java -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge {Name Node Address} {Name Node Port} {Service Principal} {Filepath containing Service Principal's Password} {Remote HDFS file path (optional)}
```

## <a name="arguments"></a>Arguments

| Argument | Description|
| --- | --- |
| *Name Node Address* | Adresse IP ou nom de domaine complet du nœud de nom. Fait référence à l’argument « LOCATION » dans votre instruction T-SQL CREATE EXTERNAL DATA SOURCE. Remarque : La version SQL Server 2019 de l’outil nécessite *hdfs :\/\/* pour précéder l’adresse IP ou le nom de domaine complet.|
| *Name Node Port* | Port du nœud de nom. Fait référence à l’argument « LOCATION » dans votre instruction T-SQL CREATE EXTERNAL DATA SOURCE. Par exemple, 8020. |
| *Principal du service* | Principal du service d’administration pour votre centre KDC. Correspond à l’argument « IDENTITY » dans votre instruction T-SQL `CREATE DATABASE SCOPED CREDENTIAL`.|
| *Service Password* | Au lieu de taper votre mot de passe sur la console, stockez-le dans un fichier et indiquez le chemin du fichier ici. Le contenu du fichier doit correspondre à ce que vous utilisez comme argument « SECRET » dans votre instruction T-SQL `CREATE DATABASE SCOPED CREDENTIAL`. |
| *Remote HDFS file path (facultatif)* | Chemin d’un fichier existant auquel accéder. S’il n’est pas spécifié, la racine « / » est utilisée. |

## <a name="example"></a>Exemple

```cmd
java -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge 10.193.27.232 8020 admin_user C:\temp\kerberos_pass.txt
```

La sortie est détaillée pour un débogage amélioré, mais vous n’avez que quatre points de contrôle principaux à examiner, que vous utilisiez MIT ou Active Directory. Ils correspondent aux quatre étapes présentées ci-dessus. 

Les extraits suivants proviennent d’un centre KDC MIT. Vous pouvez vous référer à des exemples de sorties complets de MIT et d’Active Directory à la fin de cet article dans les références.

## <a name="checkpoint-1"></a>Point de contrôle 1
Il doit y avoir un vidage hexadécimal d’un ticket avec `Server Principal = krbtgt/MYREALM.COM@MYREALM.COM`. Cette opération indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été correctement authentifié auprès du centre KDC et a reçu un ticket TGT. Si ce n’est pas le cas, le problème se situe strictement entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le centre KDC, et non Hadoop.

PolyBase ne prend **pas** en charge les relations d’approbation entre AD et MIT, et doit être configuré sur le même centre KDC comme dans le cluster Hadoop. Dans de tels environnements, la création manuelle d’un compte de service sur ce centre KDC et son utilisation pour effectuer l’authentification fonctionnent.

```cmd
|>>> KrbAsReq creating message 
 >>> KrbKdcReq send: kdc=kerberos.contoso.com UDP:88, timeout=30000, number of retries =3, #bytes=143 
 >>> KDCCommunication: kdc=kerberos.contoso.com UDP:88, timeout=30000,Attempt =1, #bytes=143 
 >>> KrbKdcReq send: #bytes read=646 
 >>> KdcAccessibility: remove kerberos.contoso.com 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 >>> KrbAsRep cons in KrbAsReq.getReply myuser 
 [2017-04-25 21:34:33,548] INFO 687[main] - com.microsoft.polybase.client.KerberosSecureLogin.secureLogin(KerberosSecureLogin.java:97) - Subject: 
 Principal: admin_user@CONTOSO.COM 
 Private Credential: Ticket (hex) = 
 0000: 61 82 01 48 30 82 01 44 A0 03 02 01 05 A1 0E 1B a..H0..D........ 
 0010: 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D A2 21 30 .CONTOSO.COM.!0 
 0020: 1F A0 03 02 01 02 A1 18 30 16 1B 06 6B 72 62 74 ........0...krbt 
 0030: 67 74 1B 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D gt..CONTOSO.COM 
 0040: A3 82 01 08 30 82 01 04 A0 03 02 01 10 A1 03 02 ....0........... 
 *[...Condensed...]* 
 0140: 67 6D F6 41 6C EB E0 C3 3A B2 BD B1 gm.Al...:... 
 Client Principal = admin_user@CONTOSO.COM 
 Server Principal = krbtgt/CONTOSO.COM@CONTOSO.COM 
 *[...Condensed...]* 
 [2017-04-25 21:34:34,500] INFO 1639[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1579) - Successfully authenticated against KDC server. 
```

## <a name="checkpoint-2"></a>Point de contrôle 2
PolyBase tente d’accéder à HDFS et échoue, car la demande ne contient pas le ticket de service nécessaire.

```cmd
 [2017-04-25 21:34:34,501] INFO 1640[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1584) - Attempting to access external filesystem at URI: hdfs://10.193.27.232:8020 
 Found ticket for admin_user@CONTOSO.COM to go to krbtgt/CONTOSO.COM@CONTOSO.COM expiring on Wed Apr 26 21:34:33 UTC 2017 
 Entered Krb5Context.initSecContext with state=STATE_NEW 
 Found ticket for admin_user@CONTOSO.COM to go to krbtgt/CONTOSO.COM@CONTOSO.COM expiring on Wed Apr 26 21:34:33 UTC 2017 
 Service ticket not found in the subject 
```

## <a name="checkpoint-3"></a>Point de contrôle 3
Un deuxième vidage hexadécimal indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a utilisé le ticket TGT et acquis le ticket de service applicable pour le nom de principal du service du nœud de nom auprès du centre KDC.

```cmd
 >>> KrbKdcReq send: kdc=kerberos.contoso.com UDP:88, timeout=30000, number of retries =3, #bytes=664 
 >>> KDCCommunication: kdc=kerberos.contoso.com UDP:88, timeout=30000,Attempt =1, #bytes=664 
 >>> KrbKdcReq send: #bytes read=669 
 >>> KdcAccessibility: remove kerberos.contoso.com 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 >>> KrbApReq: APOptions are 00100000 00000000 00000000 00000000 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 Krb5Context setting mySeqNumber to: 1033039363 
 Created InitSecContextToken: 
 0000: 01 00 6E 82 02 4B 30 82 02 47 A0 03 02 01 05 A1 ..n..K0..G...... 
 0010: 03 02 01 0E A2 07 03 05 00 20 00 00 00 A3 82 01 ......... ...... 
 0020: 63 61 82 01 5F 30 82 01 5B A0 03 02 01 05 A1 0E ca.._0..[....... 
 0030: 1B 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D A2 26 ..CONTOSO.COM.& 
 0040: 30 24 A0 03 02 01 00 A1 1D 30 1B 1B 02 6E 6E 1B 0$.......0...nn. 
 0050: 15 73 68 61 73 74 61 2D 68 64 70 32 35 2D 30 30 .hadoop-hdp25-00 
 0060: 2E 6C 6F 63 61 6C A3 82 01 1A 30 82 01 16 A0 03 .local....0..... 
 0070: 02 01 10 A1 03 02 01 01 A2 82 01 08 04 82 01 04 ................ 
 *[...Condensed...]* 
 0240: 03 E3 68 72 C4 D2 8D C2 8A 63 52 1F AE 26 B6 88 ..hr.....cR..&.. 
 0250: C4 . 
```

## <a name="checkpoint-4"></a>Point de contrôle 4
Enfin, les propriétés de fichier du chemin cible doivent être imprimées avec un message de confirmation. Les propriétés de fichier vérifient que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été authentifié par Hadoop à l’aide du ticket de service et que l’accès à la ressource sécurisée a été accordé à une session.

Si vous atteignez ce stade, cela confirme que : (i) les trois acteurs sont en mesure de communiquer correctement, (ii) les fichiers core-site.xml et jaas.conf sont corrects, et (iii) le centre KDC a reconnu vos informations d’identification.

```cmd
 [2017-04-25 21:34:35,096] INFO 2235[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1586) - File properties for "/": FileStatus{path=hdfs://10.193.27.232:8020/; isDirectory=true; modification_time=1492028259862; access_time=0; owner=hdfs; group=hdfs; permission=rwxr-xr-x; isSymlink=false} 
 [2017-04-25 21:34:35,098] INFO 2237[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1587) - Successfully accessed the external file system. 
```

## <a name="common-errors"></a>Erreurs courantes
Si l’outil a été exécuté et que les propriétés de fichier du chemin cible n’ont *pas* été imprimées (point de contrôle 4), une exception doit avoir été levée à mi-chemin. Examinez-la et étudiez le contexte de l’endroit dans le flux des quatre étapes où elle s’est produite. Envisagez les problèmes courants suivants qui se sont peut-être produits, dans l’ordre :

| Exception et messages | Cause | 
| --- | --- |
| org.apache.hadoop.security.AccessControlException<br>L’authentification SIMPLE n’est pas activée. Available:[TOKEN, KERBEROS] | La propriété hadoop.security.authentication du fichier core-site.xml n’a pas la valeur « KERBEROS ».|
|javax.security.auth.login.LoginException<br>Client introuvable dans la base de données Kerberos (6) - CLIENT_NOT_FOUND |    Le principal du service d’administration fourni n’existe pas dans le domaine spécifié dans core-site.xml.|
| javax.security.auth.login.LoginException<br> Échec de la somme de contrôle |Le principal du service d’administration existe, mais le mot de passe est incorrect. |
| Nom de la configuration native : C:\Windows\krb5.ini<br>Chargement à partir de la configuration native | Ce message indique que le module krb5LoginModule de Java a détecté des configurations clientes personnalisées sur votre ordinateur. Vérifiez vos paramètres clients personnalisés, car ils peuvent être à l’origine du problème. |
| javax.security.auth.login.LoginException<br>java.lang.IllegalArgumentException<br>Nom de principal non conforme admin_user@CONTOSO.COM: org.apache.hadoop.security.authentication.util.KerberosName$NoMatchingRule: aucune règle appliquée à admin_user@CONTOSO.COM | Ajoutez la propriété « hadoop.security.auth_to_local » à core-site.xml avec les règles appropriées selon le cluster Hadoop. |
| java.net.ConnectException<br>Tentative d’accès au système de fichiers externe à l’URI : hdfs://10.193.27.230:8020<br>Échec de l’appel de IAAS16981207/10.107.0.245 à 10.193.27.230:8020 lors d’une connexion | L’authentification auprès du centre KDC a réussi, mais l’accès au nœud de nom Hadoop a échoué. Vérifiez l’adresse IP et le port du nœud de nom. Vérifiez que le pare-feu est désactivé sur Hadoop. |
| java.io.FileNotFoundException<br>Le fichier n’existe pas : /test/data.csv |    L’authentification a réussi, mais l’emplacement spécifié n’existe pas. Vérifiez le chemin ou effectuez d’abord un test avec la racine « / ». |

## <a name="debugging-tips"></a>Conseils de débogage

### <a name="mit-kdc"></a>Centre KDC MIT  
Tous les noms de principal du service inscrits auprès du centre KDC, dont les administrateurs, peuvent être affichés en exécutant **kadmin.local** > (connexion admin) > **listprincs** sur l’hôte KDC ou tout client KDC configuré. Si Kerberos est correctement configuré sur le cluster Hadoop, il doit exister un nom de principal du service pour chacun des services disponibles dans le cluster (par exemple : `nn`, `dn`, `rm`, `yarn`, `spnego`, etc.) Leurs fichiers keytab correspondants (substituts de mots de passe) sont affichés par défaut sous **/etc/security/keytabs**. Ils sont chiffrés à l’aide de la clé privée KDC.  

Vous pouvez également utiliser [`kinit`](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/kinit.html) pour vérifier les informations d’identification de l’administrateur localement sur le centre KDC. Voici un exemple d’utilisation : `kinit identity@MYREALM.COM`. Une invite à entrer un mot de passe indique que l’identité existe.  
Les journaux du centre KDC sont disponibles dans **/var/log/krb5kdc.log**, par défaut, ce qui inclut toutes les demandes de tickets, dont l’adresse IP du client qui a effectué la demande. Deux requêtes doivent avoir été émises à partir de l’adresse IP de l’ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où l’outil a été exécuté : la première pour obtenir le ticket TGT auprès du serveur d’authentification comme **AS\_REQ**, suivie d’une requête **TGS\_REQ** pour obtenir le ticket de service auprès du serveur d’accord de tickets.

```bash
 [root@MY-KDC log]# tail -2 /var/log/krb5kdc.log 
 May 09 09:48:26 MY-KDC.local krb5kdc[2547](info): **AS_REQ** (3 etypes {17 16 23}) 10.107.0.245: ISSUE: authtime 1494348506, etypes {rep=16 tkt=16 ses=16}, admin_user@CONTOSO.COM for **krbtgt/CONTOSO.COM@CONTOSO.COM** 
 May 09 09:48:29 MY-KDC.local krb5kdc[2547](info): **TGS_REQ** (3 etypes {17 16 23}) 10.107.0.245: ISSUE: authtime 1494348506, etypes {rep=16 tkt=16 ses=16}, admin_user@CONTOSO.COM for **nn/hadoop-hdp25-00.local@CONTOSO.COM** 
```

### <a name="active-directory"></a>Active Directory 
Dans Active Directory, les noms de principal du service peuvent être affichés en accédant au Panneau de configuration > Utilisateurs et ordinateurs Active Directory > *MonDomaine* > *MonUnitéOrganisation*. Si Kerberos est correctement configuré sur le cluster Hadoop, il existe un nom de principal du service pour chacun des services disponibles (par exemple : `nn`, `dn`, `rm`, `yarn`, `spnego`, etc.)

### <a name="general-debugging-tips"></a>Conseils généraux relatifs au débogage
Il est utile d’avoir une certaine expérience de Java pour examiner les journaux et déboguer les problèmes Kerberos, lesquels sont indépendants de la fonctionnalité PolyBase SQL Server.

Si vous rencontrez toujours des problèmes d’accès à Kerberos, suivez les étapes ci-dessous pour effectuer un débogage :

1. Vérifiez que vous pouvez accéder aux données HDFS Kerberos en dehors de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez : 

    - Écrire votre propre programme Java, ou
    - Utiliser la classe `HdfsBridge` du dossier d’installation de PolyBase. Par exemple :

      ```java
      -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge 10.193.27.232 8020 admin_user C:\temp\kerberos_pass.txt
      ```

     Dans l’exemple ci-dessus, `admin_user` inclut uniquement le nom d’utilisateur, mais aucune partie relative au domaine.

2. Si vous ne pouvez pas accéder aux données HDFS Kerberos depuis l’extérieur de PolyBase :
    - Il existe deux types d’authentification Kerberos : l’authentification Kerberos Active Directory et l’authentification Kerberos MIT.
    - Vérifiez que l’utilisateur existe dans le compte de domaine et utilisez le même compte d’utilisateur lors de la tentative d’accès à HDFS.

3. Pour Kerberos Active Directory, vérifiez que vous pouvez voir le ticket mis en cache à l’aide de la commande `klist` sur Windows.
    - Connectez-vous à l’ordinateur PolyBase, puis exécutez `klist` et `klist tgt` dans l’invite de commandes pour voir si le centre KDC, le nom d’utilisateur et les types de chiffrement sont corrects.

4. Si le centre KDC peut uniquement prendre en charge AES256, vérifiez que les [fichiers de stratégie JCE](http://www.oracle.com/technetwork/java/javase/downloads/index.html) sont installés.

## <a name="see-also"></a>Voir aussi
[Integrating PolyBase with Cloudera using Active Directory Authentication](/archive/blogs/microsoftrservertigerteam/integrating-polybase-with-cloudera-using-active-directory-authentication)  
[Cloudera’s Guide to setting up Kerberos for CDH](https://www.cloudera.com/documentation/enterprise/5-6-x/topics/cm_sg_principal_keytab.html)  
[Hortonworks’ Guide to Setting up Kerberos for HDP](https://docs.hortonworks.com/HDPDocuments/Ambari-2.2.0.0/bk_Ambari_Security_Guide/content/ch_configuring_amb_hdp_for_kerberos.html)  
[Résolution des problèmes de Polybase](polybase-troubleshooting.md)