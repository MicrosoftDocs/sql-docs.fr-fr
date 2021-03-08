---
title: Connexion à partir de Linux ou macOS
description: Découvrez comment créer une connexion à une base de données à partir de Linux ou macOS à l’aide de Microsoft ODBC Driver for SQL Server.
ms.custom: ''
ms.date: 09/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- connect to linux
- configure odbc.ini
ms.assetid: f95cdbce-e7c2-4e56-a9f7-8fa3a920a125
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d1bdbcbb34be9cbfa075ead7e1cd03ec813a5d9d
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247308"
---
# <a name="connecting-from-linux-or-macos"></a>Connexion à partir de Linux ou macOS

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Cette rubrique explique comment créer une connexion à une base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="connection-properties"></a>Propriétés de connexion  

Consultez [Attributs et mots clés de chaîne de connexion et DSN](../dsn-connection-string-attribute.md) pour découvrir tous les mots clés de chaîne de connexion et attributs pris en charge sur Linux et macOS.

> [!IMPORTANT]  
> Quand vous vous connectez à une base de données qui utilise la mise en miroir de bases de données (qui a un partenaire de basculement), ne spécifiez pas le nom de la base de données dans la chaîne de connexion. Envoyez plutôt une commande **use** _database_name_ pour vous connecter à la base de données avant l’exécution de vos requêtes.  
  
La valeur passée au mot clé **Pilote** peut être :  
  
- le nom que vous avez utilisé quand vous avez installé le pilote,

- le chemin de la bibliothèque du pilote, spécifié dans le fichier .ini du modèle utilisé pour installer le pilote.  

Les DSN sont facultatifs. Vous pouvez utiliser un DSN pour définir des mots clés de la chaîne de connexion sous un nom `DSN` que vous pouvez ensuite référencer dans la chaîne de connexion. Pour créer un DSN, créez (si nécessaire) et modifiez le fichier **~/.odbc.ini** (`.odbc.ini` dans votre répertoire de départ) pour un DSN utilisateur uniquement accessible à l’utilisateur actuel, ou `/etc/odbc.ini` pour un DSN système (privilèges d’administrateur requis). Voici un exemple de fichier qui montre les entrées minimales exigées pour un nom de source de données :  

```ini
# [DSN name]
[MSSQLTest]  
Driver = ODBC Driver 17 for SQL Server  
# Server = [protocol:]server[,port]  
Server = tcp:localhost,1433
#
# Note:  
# Port is not a valid keyword in the odbc.ini file  
# for the Microsoft ODBC driver on Linux or macOS
#  
```  

Pour vous connecter à l’aide du DSN ci-dessus dans une chaîne de connexion, vous devez spécifier le mot clé `DSN` comme suit : `DSN=MSSQLTest;UID=my_username;PWD=my_password`  
La chaîne de connexion ci-dessus équivaut à spécifier une chaîne de connexion sans le mot clé `DSN` comme suit : `Driver=ODBC Driver 17 for SQL Server;Server=tcp:localhost,1433;UID=my_username;PWD=my_password`

Vous pouvez éventuellement spécifier le protocole et le port à connecter au serveur. Par exemple, **Server=tcp:** _servername_ **,12345**. Notez que le seul protocole pris en charge par les pilotes Linux et macOS est `tcp`.

Pour vous connecter à une instance nommée sur un port statique, utilisez <b>Server=</b>*servername*,**port_number**. La connexion à un port dynamique n’est pas prise en charge avant la version 17.4.

Éventuellement, vous pouvez ajouter les informations de nom de source de données à un fichier de modèle, puis exécuter la commande suivante pour l’ajouter à `~/.odbc.ini` :
 - **odbcinst -i -s -f** _template_file_  

Pour obtenir une documentation complète sur les fichiers ini et `odbcinst`, consultez la [documentation unixODBC](http://www.unixodbc.org/odbcinst.html). Pour les entrées dans le fichier `odbc.ini` propre à ODBC Driver pour SQL Server, consultez [Mots clés et attributs du nom de source de données et de la chaîne de connexion](../dsn-connection-string-attribute.md) pour ceux qui sont pris en charge sur Linux et macOS.

Vous pouvez vérifier que votre pilote fonctionne en utilisant `isql` pour tester la connexion, ou en exécutant cette commande :
 - **bcp master.INFORMATION_SCHEMA.TABLES out OutFile.dat -S <server> -U <name> -P <password>**  

## <a name="using-tlsssl"></a>Utilisation de TLS/SSL  

Vous pouvez utiliser le protocole TLS (Transport Layer Security), anciennement SSL (Secure Sockets Layer), pour chiffrer les connexions à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le protocole TLS protège les noms d’utilisateur et mots de passe [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur le réseau. Il vérifie également l’identité du serveur à des fins de protection contre les attaques de l’intercepteur (« man-in-the-middle »).  

L’activation du chiffrement renforce la sécurité au détriment des performances.

Pour plus d’informations, consultez [Chiffrement des connexions à SQL Server](/previous-versions/sql/sql-server-2008-r2/ms189067(v=sql.105)) et [Utilisation du chiffrement sans validation](../../../relational-databases/native-client/features/using-encryption-without-validation.md).

Quels que soient les paramètres pour **Encrypt** et **TrustServerCertificate**, les informations d’identification de connexion serveur (nom d’utilisateur et mot de passe) sont toujours chiffrées. Le tableau suivant montre l’effet des paramètres **Encrypt** et **TrustServerCertificate** .  

||**TrustServerCertificate=no**|**TrustServerCertificate=yes**|  
|-|-------------------------------------|------------------------------------|  
|**Encrypt=no**|Le certificat de serveur n’est pas vérifié.<br /><br />Les données envoyées entre le client et le serveur ne sont pas chiffrées.|Le certificat de serveur n’est pas vérifié.<br /><br />Les données envoyées entre le client et le serveur ne sont pas chiffrées.|  
|**Encrypt=yes**|Le certificat de serveur est vérifié.<br /><br />Les données envoyées entre le client et le serveur sont chiffrées.<br /><br />Le nom (ou l’adresse IP) indiqué dans un nom commun de l’objet ou autre nom de l’objet dans un certificat TLS/SSL [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit correspondre exactement au nom (ou à l’adresse IP) du serveur spécifié dans la chaîne de connexion.|Le certificat de serveur n’est pas vérifié.<br /><br />Les données envoyées entre le client et le serveur sont chiffrées.|  

Par défaut, les connexions chiffrées vérifient toujours le certificat du serveur. Toutefois, si vous vous connectez à un serveur qui possède un certificat auto-signé, ajoutez également l’option `TrustServerCertificate` pour contourner la vérification du certificat par rapport à la liste des autorités de certification approuvées :  

```
Driver={ODBC Driver 17 for SQL Server};Server=ServerNameHere;Encrypt=YES;TrustServerCertificate=YES  
```  
  
Le protocole TLS utilise la bibliothèque OpenSSL. Le tableau suivant présente les versions minimales prises en charge d’OpenSSL et les emplacements du magasin d’approbations de certificat par défaut pour chaque plateforme :

|Plateforme|Version OpenSSL minimale|Emplacement du magasin d’approbations de certificat par défaut|  
|------------|---------------------------|--------------------------------------------|
|Debian 10|1.1.1|/etc/ssl/certs|
|Debian 9|1.1.0|/etc/ssl/certs|
|Debian 8.71|1.0.1|/etc/ssl/certs|
|OS X 10.11, macOS 10.12-10.15|1.0.2|/usr/local/etc/openssl/certs|
|Red Hat Enterprise Linux 8|1.1.1|/etc/pki/tls/cert.pem|
|Red Hat Enterprise Linux 7|1.0.1|/etc/pki/tls/cert.pem|
|Red Hat Enterprise Linux 6|1.0.0-10|/etc/pki/tls/cert.pem|
|SUSE Linux Enterprise 15|1.1.0|/etc/ssl/certs|
|SUSE Linux Enterprise 11, 12|1.0.1|/etc/ssl/certs|
|Ubuntu 18.10, 19.04, 19.10, 20.04|1.1.1|/etc/ssl/certs|
|Ubuntu 18.04|1.1.0|/etc/ssl/certs|
|Ubuntu 16.04, 16.10, 17.10|1.0.2|/etc/ssl/certs|
|Ubuntu 14.04|1.0.1|/etc/ssl/certs|

Vous pouvez également spécifier le chiffrement dans la chaîne de connexion à l’aide de l’option `Encrypt` lorsque vous vous connectez avec **SQLDriverConnect**.

## <a name="adjusting-the-tcp-keep-alive-settings"></a>Ajustement des paramètres TCP Keep-Alive

À partir d’ODBC 17.4, vous pouvez définir la fréquence à laquelle le pilote envoie des paquets Keep-Alive et les retransmet lorsqu’une réponse n’est pas reçue.
Pour cela, ajoutez les paramètres suivants à la section du pilote dans `odbcinst.ini`, ou à la section du DSN dans `odbc.ini`. Lors de la connexion à un DSN, le pilote utilise les paramètres de la section du DSN, le cas échéant. Dans le cas contraire, ou si vous vous connectez avec une chaîne de connexion uniquement, il utilisera les paramètres de la section du pilote dans `odbcinst.ini`. Si le paramètre n’est présent dans aucun de ces emplacements, le pilote utilise la valeur par défaut.

- `KeepAlive=<integer>` contrôle la fréquence à laquelle TCP tente de vérifier qu’une connexion inactive est toujours intacte en envoyant un paquet keep-alive. La valeur par défaut est **30** secondes.

- `KeepAliveInterval=<integer>` détermine l’intervalle qui sépare les retransmissions keep-alive jusqu’à ce qu’une réponse soit reçue.  La valeur par défaut est **1** seconde.

## <a name="see-also"></a>Voir aussi

- [Installation de Microsoft ODBC Driver for SQL Server sur Linux](installing-the-microsoft-odbc-driver-for-sql-server.md)
- [Installation de Microsoft ODBC Driver for SQL Server sur macOS](install-microsoft-odbc-driver-sql-server-macos.md)
- [Instructions de programmation](programming-guidelines.md)
