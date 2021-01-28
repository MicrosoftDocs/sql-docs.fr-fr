---
title: Chiffrement des connexions à SQL Server sur Linux
description: SQL Server sur Linux utilise le protocole TLS pour chiffrer des données transmises sur un réseau entre une application cliente et une instance de SQL Server.
ms.date: 06/29/2020
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
helpviewer_keywords:
- Linux, encrypted connections
ms.openlocfilehash: 2d4848a8fa3661abd94ef63f3289261142998409
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766324"
---
# <a name="encrypting-connections-to-sql-server-on-linux"></a>Chiffrement des connexions à SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Linux peut utiliser le protocole TLS pour chiffrer des données transmises sur un réseau entre une application cliente et une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] prend en charge les mêmes protocoles TLS sur Windows et sur Linux : TLS 1.2, 1.1 et 1.0. Toutefois, les étapes de configuration de TLS sont spécifiques au système d’exploitation sur lequel [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] s’exécute.  

## <a name="requirements-for-certificates"></a>Conditions requises pour les certificats 
Avant de commencer, vous devez vous assurer que vos certificats respectent les exigences suivantes :
- L'heure actuelle du système doit être postérieure à la propriété Valid from du certificat et antérieure à la propriété Valide to du certificat.
- Le certificat doit être destiné à une authentification serveur. Pour cela, la propriété Utilisation améliorée de la clé du certificat doit indiquer l’authentification du serveur (1.3.6.1.5.5.7.3.1).
- Le certificat doit être créé avec l'option KeySpec de AT_KEYEXCHANGE. Habituellement, la propriété d'utilisation de la clé du certificat (KEY_USAGE) inclut également un chiffrement de clé (CERT_KEY_ENCIPHERMENT_KEY_USAGE).
- La propriété Subject du certificat doit indiquer que le nom courant (CN) est le même que le nom de domaine ou le nom de domaine complet (FQDN, Fully Qualified Domain Name) de l'ordinateur serveur. Remarque : Les certificats génériques sont pris en charge.

## <a name="configuring-the-openssl-libraries-for-use-optional"></a>Configuration des bibliothèques OpenSSL en vue de leur utilisation (facultatif)
Vous pouvez créer dans le répertoire `/opt/mssql/lib/` des liens symboliques qui référencent les bibliothèques `libcrypto.so` et `libssl.so` à utiliser pour le chiffrement. C’est utile si vous souhaitez forcer SQL Server à utiliser une version spécifique d’OpenSSL autre que la valeur par défaut fournie par le système. En l’absence de ces liens symboliques, SQL Server chargera les bibliothèques OpenSSL configurées par défaut sur le système.

Ces liens symboliques doivent être nommés `libcrypto.so` et `libssl.so` et placés dans le répertoire `/opt/mssql/lib/`.

## <a name="overview"></a>Vue d’ensemble
Le protocole TLS est utilisé pour chiffrer des connexions d'une application cliente vers [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Lorsqu’il est configuré correctement, le protocole TLS garantit la confidentialité et l’intégrité des données pour les communications entre le client et le serveur.  Les connexions TLS peuvent être lancées par le client ou par le serveur. 

## <a name="client-initiated-encryption"></a>Chiffrement lancé par le client 
- **Gérer un certificat** (/CN doit correspondre au nom de domaine complet de votre hôte SQL Server)

> [!NOTE]
> Pour cet exemple, nous utilisons un certificat autosigné, ce qui ne doit pas être le cas pour les scénarios de production. Vous devez utiliser des certificats d’autorité de certification.<br>
> Vérifiez que le ou les dossiers dans lesquels vous enregistrez vos certificats et vos clés privées sont accessibles au groupe ou à l’utilisateur MSSQL et que l’autorisation est définie avec la valeur 700 (drwx-----). Vous pouvez créer des dossiers manuellement avec une autorisation égale à 700 (drwx------) et détenue par le groupe ou l’utilisateur MSSQL. Vous pouvez également définir une autorisation égale à 755 (drwxr-xr-x) et détenue par un autre utilisateur, mais elle doit être accessible au groupe/utilisateur MSSQL. Par exemple, vous pouvez créer un dossier nommé « sslcert » sous le chemin « /var/opt/mssql/ », puis enregistrer le certificat et la clé privée avec une autorisation égale à 600 sur les fichiers, comme indiqué ci-dessous. 

```bash
openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=mssql.contoso.com' -keyout mssql.key -out mssql.pem -days 365 
sudo chown mssql:mssql mssql.pem mssql.key 
sudo chmod 600 mssql.pem mssql.key 
# in this case we are saving the certificate to the certs folder under /etc/ssl/ which has the following permission 755(drwxr-xr-x)
sudo mv mssql.pem /etc/ssl/certs/ drwxr-xr-x 
# in this case we are saving the private key to the private folder under /etc/ssl/ with permissions set to 755(drwxr-xr-x)
sudo mv mssql.key /etc/ssl/private/ 
```

- **Configurer SQL Server**

```bash
systemctl stop mssql-server 
cat /var/opt/mssql/mssql.conf 
sudo /opt/mssql/bin/mssql-conf set network.tlscert /etc/ssl/certs/mssql.pem 
sudo /opt/mssql/bin/mssql-conf set network.tlskey /etc/ssl/private/mssql.key 
sudo /opt/mssql/bin/mssql-conf set network.tlsprotocols 1.2 
sudo /opt/mssql/bin/mssql-conf set network.forceencryption 0 
```

- **Inscrire le certificat sur votre ordinateur client (Windows, Linux ou macOS)**

    -   Si vous utilisez un certificat signé par une autorité de certification, vous devez copier le certificat d’autorité de certification au lieu du certificat utilisateur sur l’ordinateur client. 
    -   Si vous utilisez le certificat auto-signé, copiez simplement le fichier .pem dans les dossiers de distribution suivants et exécutez les commandes pour les activer 
        - **Ubuntu** : copier le certificat dans `/usr/share/ca-certificates/`, renommer son extension .crt et utiliser `dpkg-reconfigure ca-certificates` pour l’activer en tant que certificat d’autorité de certification système. 
        - **RHEL** : copier le certificat dans `/etc/pki/ca-trust/source/anchors/` et l’utiliser `update-ca-trust` pour l’activer en tant que certificat d’autorité de certification système.
        - **SUSE** : copier le certificat dans `/usr/share/pki/trust/anchors/` et l’utiliser `update-ca-certificates` pour l’activer en tant que certificat d’autorité de certification système.
        - **Windows** :  importer le fichier. pem en tant que certificat sous utilisateur actuel-> autorités de certification racines de confiance-> certificats
        - **macOS** : 
           - copier le certificat dans `/usr/local/etc/openssl/certs`
           - Exécutez la commande suivante pour récupérer la valeur de hachage : `/usr/local/Cellar/openssl/1.0.2l/openssl x509 -hash -in mssql.pem -noout`
           - Renommez le certificat à la valeur. Par exemple : `mv mssql.pem dc2dd900.0`. Vérifiez que dc2dd900.0 se trouve dans `/usr/local/etc/openssl/certs`
    
-   **Exemples de chaîne de connexion** 

    - **[!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)]**   
  ![Boîte de dialogue de connexion de SSMS](media/sql-server-linux-encrypted-connections/ssms-encrypt-connection.png "Boîte de dialogue de connexion de SSMS")  
  
    - **SQLCMD** 

        `sqlcmd  -S <sqlhostname> -N -U sa -P '<YourPassword>'`

    - **ADO.NET** 

        `"Encrypt=True; TrustServerCertificate=False;"`

    - **ODBC** 

        `"Encrypt=Yes; TrustServerCertificate=no;"`

    - **JDBC** 

        `"encrypt=true; trustServerCertificate=false;"`

## <a name="server-initiated-encryption"></a>Chiffrement lancé par le serveur 

- **Gérer un certificat** (/CN doit correspondre au nom de domaine complet de votre hôte SQL Server)

```bash
openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=mssql.contoso.com' -keyout mssql.key -out mssql.pem -days 365 
sudo chown mssql:mssql mssql.pem mssql.key 
sudo chmod 600 mssql.pem mssql.key   
sudo mv mssql.pem /etc/ssl/certs/ 
sudo mv mssql.key /etc/ssl/private/ 
```

- **Configurer SQL Server**

```bash
systemctl stop mssql-server 
cat /var/opt/mssql/mssql.conf 
sudo /opt/mssql/bin/mssql-conf set network.tlscert /etc/ssl/certs/mssql.pem 
sudo /opt/mssql/bin/mssql-conf set network.tlskey /etc/ssl/private/mssql.key 
sudo /opt/mssql/bin/mssql-conf set network.tlsprotocols 1.2 
sudo /opt/mssql/bin/mssql-conf set network.forceencryption 1 
```

-   **Exemples de chaîne de connexion** 

    - **SQLCMD**

        `sqlcmd  -S <sqlhostname> -U sa -P '<YourPassword>'`

    - **ADO.NET** 

        `"Encrypt=False; TrustServerCertificate=False;"`

    - **ODBC** 

        `"Encrypt=no; TrustServerCertificate=no;"`

    - **JDBC** 

        `"encrypt=false; trustServerCertificate=false;"`

> [!NOTE]
> Définissez **TrustServerCertificate** sur True si le client ne peut pas se connecter à l’autorité de certification pour valider l’authenticité du certificat

## <a name="common-connection-errors"></a>Erreurs de connexion courantes  

|Message d’erreur |Fix |
|--- |--- |
|La chaîne de certificats a été émise par une autorité qui n’est pas approuvée.  |Cette erreur se produit lorsque des clients ne peuvent pas vérifier la signature sur le certificat présenté par SQL Server pendant la négociation TLS. Assurez-vous que le client approuve directement le certificat [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ou l’autorité de certification qui a signé le certificat SQL Server. |
|Le principal nom cible est incorrect.  |Assurez-vous que le champ Nom commun sur le certificat de SQL Server correspond au nom de serveur spécifié dans la chaîne de connexion du client. |  
|une connexion existante a dû être fermée par l’hôte distant. |Cette erreur peut se produire lorsque le client ne prend pas en charge la version du protocole TLS requise par SQL Server. Par exemple, si [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est configuré pour exiger TLS 1.2, assurez-vous que vos clients prennent également en charge le protocole TLS 1.2. |
| | |   
