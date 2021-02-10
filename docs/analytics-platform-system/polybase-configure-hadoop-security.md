---
title: Configurer Polybase Hadoop Security
description: Fournit une référence pour différents paramètres de configuration qui affectent la connectivité Polybase APS à Hadoop.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: 3b99f9d274f7c2d30bf8fae326953e2f57e9810e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052730"
---
# <a name="configure-polybase-hadoop-security"></a>Configurer Polybase Hadoop Security

Cet article fournit une référence pour différents paramètres de configuration qui affectent la connectivité Polybase APS à Hadoop. Pour obtenir une procédure pas à pas sur ce qui est Polybase, consultez [qu’est-ce que Polybase](configure-polybase-connectivity-to-external-data.md).

> [!NOTE]
> Sur APS, les modifications apportées aux fichiers XML sont nécessaires sur tous les nœuds de calcul et de contrôle.
> 
> Soyez particulièrement vigilant lors de la modification de fichiers XML dans APS. Les balises manquantes ou les caractères indésirables peuvent invalider le fichier XML qui entrave le usablilty de la fonctionnalité.
> Les fichiers de configuration Hadoop se trouvent dans le chemin d’accès suivant :  
> ```  
> C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100\Hadoop\conf 
> ``` 
> Toute modification apportée aux fichiers XML requiert un redémarrage du service pour être effective.

## <a name="hadooprpcprotection-setting"></a><a id="rpcprotection"></a> Paramètre Hadoop.RPC.Protection

Une méthode courante pour sécuriser la communication dans un cluster hadoop consiste à changer le paramètre de configuration hadoop.rpc.protection de « Privacy » à « Integrity ». Par défaut, PolyBase suppose que la configuration est définie sur « Authenticate ». Pour remplacer cette valeur par défaut, ajoutez la propriété suivante dans votre fichier core-site.xml. Cette nouvelle configuration permet de transférer en toute sécurité les données entre les nœuds hadoop et la connexion SSL vers SQL Server.

```xml
<!-- RPC Encryption information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG -->
   <property>
     <name>hadoop.rpc.protection</name>
     <value></value>
   </property> 
```

## <a name="kerberos-configuration"></a><a id="kerberossettings"></a> Configuration de Kerberos  

Notez que quand PolyBase s’authentifie auprès d’un cluster sécurisé Kerberos, le paramètre hadoop.rpc.protection doit être défini sur « Authenticate ». De cette façon, la communication de données entre les nœuds Hadoop n’est pas chiffrée. Afin d’utiliser les paramètres « Privacy » ou « Integrity » pour hadoop.rpc.protection, mettez à jour le fichier core-site.xml sur le serveur PolyBase. Pour plus d’informations, consultez la section précédente [Connexion à un cluster Hadoop avec Hadoop.rpc.protection](#rpcprotection).

Pour vous connecter à un cluster Hadoop sécurisé par Kerberos à l’aide de MIT KDC, les modifications suivantes sont nécessaires sur tous les nœuds de calcul APS et le nœud de contrôle :

1. Recherchez les répertoires de configuration Hadoop dans le chemin d’installation des APS. En règle générale, le chemin d’accès est le suivant :  

   ```  
   C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100\Hadoop\conf  
   ```  

2. Recherchez la valeur de configuration côté Hadoop des clés de configuration répertoriées dans le tableau. Sur l’ordinateur Hadoop, recherchez les fichiers dans le répertoire de configuration Hadoop.  
   
3. Copiez les valeurs de configuration dans la propriété de valeur dans les fichiers correspondants sur l’ordinateur SQL Server.  
   
   |**#**|**Fichier de configuration**|**Clé de configuration**|**Action**|  
   |------------|----------------|---------------------|----------|   
   |1|core-site.xml|polybase.kerberos.kdchost|Spécifiez le nom d’hôte KDC. Par exemple : kerberos.votre-domaine.com.|  
   |2|core-site.xml|polybase.kerberos.realm|Spécifiez le domaine Kerberos. Par exemple : VOTRE-DOMAINE.COM|  
   |3|core-site.xml|hadoop.security.authentication|Recherchez la configuration côté Hadoop et copiez-la sur l’ordinateur SQL Server. Par exemple : KERBEROS<br></br>**Note de sécurité :** KERBEROS doit être écrit en majuscules. Dans le cas contraire, il pourrait ne pas être activé.|   
   |4|hdfs-site.xml|dfs.namenode.kerberos.principal|Recherchez la configuration côté Hadoop et copiez-la sur l’ordinateur SQL Server. Par exemple : hdfs/_HOST@YOUR-REALM.COM|  
   |5|mapred-site.xml|mapreduce.jobhistory.principal|Recherchez la configuration côté Hadoop et copiez-la sur l’ordinateur SQL Server. Par exemple : mapred/_HOST@YOUR-REALM.COM|  
   |6|mapred-site.xml|mapreduce.jobhistory.address|Recherchez la configuration côté Hadoop et copiez-la sur l’ordinateur SQL Server. Par exemple : 10.193.26.174:10020|  
   |7|yarn-site.xml yarn.|yarn.resourcemanager.principal|Recherchez la configuration côté Hadoop et copiez-la sur l’ordinateur SQL Server. Par exemple : yarn/_HOST@YOUR-REALM.COM|  

**core-site.xml**
```xml
<property>
  <name>polybase.kerberos.realm</name>
  <value></value>
</property>
<property>
  <name>polybase.kerberos.kdchost</name>
  <value></value>
</property>
<property>
    <name>hadoop.security.authentication</name>
    <value>KERBEROS</value>
</property>
```

**hdfs-site.xml**
```xml
<property>
  <name>dfs.namenode.kerberos.principal</name>
  <value></value> 
</property>
```

**mapred-site.xml**
```xml
<property>
  <name>mapreduce.jobhistory.principal</name>
  <value></value>
</property>
<property>
  <name>mapreduce.jobhistory.address</name>
  <value></value>
</property>
```

**yarn-site.XML**
```xml
<property>
  <name>yarn.resourcemanager.principal</name>
  <value></value>
</property>
```

4. Créez un objet d’informations d’identification limité à la base de données pour spécifier les informations d’authentification de chaque utilisateur Hadoop. Consultez [Objets T-SQL PolyBase](../relational-databases/polybase/polybase-t-sql-objects.md).

## <a name="hadoop-encryption-zone-setup"></a><a id="encryptionzone"></a> Configuration de la zone de chiffrement Hadoop
Si vous utilisez la zone de chiffrement Hadoop, modifiez core-site.xml et hdfs-site.xml comme suit. Indiquez l’adresse IP où le service KMS s’exécute avec le numéro de port correspondant. Le port par défaut pour KMS sur CDH est 16000.

**core-site.xml**
```xml
<property>
  <name>hadoop.security.key.provider.path</name>
  <value>kms://http@<ip address>:16000/kms</value> 
</property>
```

**hdfs-site.xml**
```xml
<property>
  <name>dfs.encryption.key.provider.uri</name>
  <value>kms://http@<ip address>:16000/kms</value>
</property>
<property>
  <name>hadoop.security.key.provider.path</name>
  <value>kms://http@<ip address>:16000/kms</value>
  </property>
```