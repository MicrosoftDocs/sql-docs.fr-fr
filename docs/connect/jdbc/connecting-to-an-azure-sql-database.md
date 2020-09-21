---
title: Connexion à une base de données SQL Azure
description: Cet article traite des problèmes liés à l’utilisation du pilote Microsoft JDBC pour SQL Server pour se connecter à une instance Azure SQL Database.
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 49645b1f-39b1-4757-bda1-c51ebc375c34
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 52821d92728e5d54f98e6d977e7829dd13ed5bf0
ms.sourcegitcommit: 822d4b3cfa53269535500a3db5877a82b5076728
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87988587"
---
# <a name="connecting-to-an-azure-sql-database"></a>Connexion à une base de données SQL Azure

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Cet article aborde les problèmes rencontrés lors de l’utilisation de [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] pour la connexion à une base de données [!INCLUDE[ssAzure](../../includes/ssazure_md.md)]. Pour plus d’informations sur la connexion à une base de données [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], consultez :  
  
- [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)  
  
- [Procédure : Se connecter à Azure SQL à l'aide de JDBC](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-java)  

- [Connexion avec l’authentification Azure Active Directory](connecting-using-azure-active-directory-authentication.md)  
  
## <a name="details"></a>Détails

Lorsque vous vous connectez à une base de données [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], connectez-vous à la base de données MASTER pour appeler **SQLServerDatabaseMetaData.getCatalogs**.  
[!INCLUDE[ssAzure](../../includes/ssazure_md.md)] ne prend pas en charge le retour de l’ensemble complet de catalogues d’une base de données utilisateur. **SQLServerDatabaseMetaData.getCatalogs** utilise la vue sys.databases pour récupérer les catalogues. Pour comprendre le comportement de **SQLServerDatabaseMetaData.getCatalogs** sur une base de données [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], consultez la discussion relative aux autorisations dans [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
## <a name="connections-dropped"></a>Connexions supprimées

Lors de la connexion à une base de données [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], les connexions inactives peuvent être arrêtées par un composant réseau (comme un pare-feu) après une période d’inactivité. Il existe deux types de connexions inactives dans ce contexte :  

- Les connexions inactives sur la couche TCP, lorsque les connexions peuvent être supprimées par n'importe quel dispositif réseau.  

- Les connexions inactives via la passerelle Azure SQL, en cas de messages TCP **keepalive** (qui rendent la connexion non inactive du point de vue de TCP), mais sans requête active pendant 30 minutes. Dans ce scenario, la passerelle détermine que la connexion TDS est inactive à 30 minutes et l'arrête.  
  
Pour éviter la suppression des connexions inactives par un composant réseau, les paramètres de registre suivants (ou leurs équivalents non Windows) doivent être définis sur le système d'exploitation dans lequel se trouve le pilote.  
  
|Paramètre de registre|Valeur recommandée|  
|----------------------|-----------------------|  
|HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ Tcpip \ Parameters \ KeepAliveTime|30000|  
|HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ Tcpip \ Parameters \ KeepAliveInterval|1 000|  
|HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ Tcpip \ Parameters \ TcpMaxDataRetransmissions|10|  
  
Redémarrez l’ordinateur pour appliquer les paramètres du Registre.  

Pour effectuer cela lorsque vous exécutez le système dans Azure, créez une tâche de démarrage pour ajouter les clés de Registre.  Par exemple, ajoutez la tâche de démarrage suivante au fichier de définition du service :  

```xml
<Startup>  
    <Task commandLine="AddKeepAlive.cmd" executionContext="elevated" taskType="simple">  
    </Task>  
</Startup>  
```

Puis ajoutez un fichier AddKeepAlive.cmd à votre projet. Définissez le paramètre « Copier dans le répertoire de sortie » sur « Toujours copier ». Voici un exemple de fichier AddKeepAlive.cmd :  

```bat
if exist keepalive.txt goto done  
time /t > keepalive.txt  
REM Workaround for JDBC keep alive on Azure SQL  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v KeepAliveTime /t REG_DWORD /d 30000 >> keepalive.txt  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v KeepAliveInterval /t REG_DWORD /d 1000 >> keepalive.txt  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v TcpMaxDataRetransmissions /t REG_DWORD /d 10 >> keepalive.txt  
shutdown /r /t 1  
:done  
```

## <a name="appending-the-server-name-to-the-userid-in-the-connection-string"></a>Ajout du nom de serveur à l'ID d'utilisateur dans la chaîne de connexion  

Avant la version 4.0 du [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], lors de la connexion à [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], vous deviez ajouter le nom du serveur à l’ID d’utilisateur dans la chaîne de connexion. Par exemple : user@servername. À compter de la version 4.0 de [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], il n’est plus nécessaire d’ajouter @servername à l’ID d’utilisateur dans la chaîne de connexion.  

## <a name="using-encryption-requires-setting-hostnameincertificate"></a>L'utilisation du chiffrement requiert la définition de hostNameInCertificate

Avant [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 7.2, vous devez spécifier **hostNameInCertificate** lorsque vous vous connectez à une base de données [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] si vous choisissez **encrypt=true** (si le nom du serveur dans la chaîne de connexion est *nom_court*.*nom_domaine*, définissez la propriété **hostNameInCertificate** sur \*.*nom_domaine*). Cette propriété est facultative depuis la version 7.2 du pilote.

Par exemple :

```java
jdbc:sqlserver://abcd.int.mscds.com;databaseName=myDatabase;user=myName;password=myPassword;encrypt=true;hostNameInCertificate=*.int.mscds.com;
```

## <a name="see-also"></a>Voir aussi

[Connexion à SQL Server avec le pilote JDBC](connecting-to-sql-server-with-the-jdbc-driver.md)  
