---
title: Utilisation de l'authentification NTLM pour se connecter à SQL Server
description: Découvrez comment établir une connexion à la base de données SQL à l’aide de l’authentification NTLM avec le pilote JDBC.
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.suite: sql
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: lilgreenbird
ms.author: v-susanh
manager: kenvh
ms.openlocfilehash: a0d58875603781f4959ee8d10cb683c98bd884c1
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837468"
---
# <a name="using-ntlm-authentication-to-connect-to-sql-server"></a>Utilisation de l'authentification NTLM pour se connecter à SQL Server

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Permet à une application d’utiliser la propriété de connexion **AuthenticationScheme** pour indiquer qu’elle souhaite se connecter à une base de données à l’aide de l’authentification NTLM v2. [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 

Les propriétés suivantes sont également utilisées pour l’authentification NTLM :

- **domain = domainName** (facultatif)
- **user = userName**
- **password = password**
- **integratedSecurity = true**

En dehors de **domain**, les autres propriétés sont obligatoires ; le pilote génère une erreur s’il n’y en a pas lorsque la propriété authenticationScheme de **NTLM** est utilisée. 

Pour plus d’informations sur les propriétés de connexion, consultez [Définition des propriétés de connexion](../../connect/jdbc/setting-the-connection-properties.md). Pour plus d’informations sur le protocole d’authentification Microsoft NTLM, consultez [Microsoft NTLM](/windows/desktop/SecAuthN/microsoft-ntlm).

## <a name="remarks"></a>Notes

Consultez [Sécurité du réseau : Niveau d’authentification LAN Manager](/windows/security/threat-protection/security-policy-settings/network-security-lan-manager-authentication-level) pour la description des paramètres SQL Server, qui contrôlent le comportement de l’authentification NTLM. 

## <a name="logging"></a>Journalisation

Un nouvel enregistreur d'événements a été ajouté pour la prise en charge de l'authentification NTLM : com.microsoft.sqlserver.jdbc.internals.NTLMAuthentication. Pour plus d’informations, consultez [Suivi du fonctionnement du pilote](../../connect/jdbc/tracing-driver-operation.md).

## <a name="datasource"></a>DataSource

Lorsque vous utilisez une source de données pour créer des connexions, les propriétés NTLM peuvent être définies par programmation à l’aide de **setAuthenticationScheme**, **setDomain** et (éventuellement) **setServerSpn**.

```java
SQLServerDataSource ds = new SQLServerDataSource();
ds.setServerName("<server>");
ds.setPortNumber(1433); // change if necessary
ds.setIntegratedSecurity(true);
ds.setAuthenticationScheme("NTLM");
ds.setDomain("<domainName>");
ds.setUser("<userName>");
ds.setPassword("<password>");
ds.setDatabaseName("<database>");
ds.setServerSpn("<serverSpn");

try (Connection c = ds.getConnection(); Statement s = c.createStatement();
        ResultSet rs = s.executeQuery("select auth_scheme from sys.dm_exec_connections where session_id=@@spid")) {
    while (rs.next()) {
        System.out.println("Authentication Scheme: " + rs.getString(1));
    }
}
```

## <a name="service-principal-names"></a>Noms des principaux du service

Le nom de principal du service (SPN) est le nom par lequel un client identifie de manière unique l'instance d'un service.

Vous pouvez spécifier le SPN à l’aide de la propriété de connexion **serverSpn**, ou laisser le pilote le créer pour vous (par défaut). Cette propriété a le format suivant : « MSSQLSvc/fqdn:port\@REALM », où fqdn désigne le nom de domaine complet, port désigne le numéro de port et REALM désigne le domaine du serveur SQL Server en majuscules. La partie REALM de cette propriété est facultative, car le domaine par défaut est le même que le domaine du serveur.

Par exemple, votre SPN peut ressembler à ce qui suit : « MSSQLSvc/some-server.zzz.corp.contoso.com:1433 »

Pour plus d'informations sur les noms des principaux du service (SPN), consultez :

- [Prise en charge des noms de principaux du service (SPN) dans les connexions clientes](../../relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections.md)

> [!NOTE]  
> L’attribut de connexion serverSpn est uniquement pris en charge par Microsoft JDBC Driver 4.2 et versions ultérieures.

> Avant la version 6.2 du pilote JDBC, vous deviez définir explicitement le **serverSpn**. Depuis la version 6.2, le pilote est en mesure de générer le **serverSpn** par défaut, bien qu’il soit possible d’utiliser **serverSpn** explicitement.

## <a name="security-risks"></a>Risques liés à la sécurité

Le protocole NTLM est un ancien protocole d’authentification avec différentes vulnérabilités, qui présentent un risque pour la sécurité. Il est basé sur un modèle de chiffrement relativement faible et est vulnérable à diverses attaques. Il a été remplacé par Kerberos, qui est beaucoup plus sécurisé et recommandé. L’authentification NTLM ne doit être utilisée que dans un environnement approuvé sécurisé, ou lorsque Kerberos ne peut pas être utilisé.

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] prend en charge uniquement NTLM v2, qui offre des améliorations de sécurité par rapport au protocole v1 d’origine. Il est également recommandé d’activer la protection étendue ou d’utiliser le chiffrement SSL pour une sécurité accrue. 

Pour plus d’informations sur l’activation de la protection étendue, consultez :

- [Se connecter au moteur de base de données à l'aide de la protection étendue](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md)

Pour plus d’informations sur la connexion avec le chiffrement SSL, consultez :

- [Connexion avec chiffrement SSL](../../connect/jdbc/connecting-with-ssl-encryption.md)

> [!NOTE]
> Pour la version 7.4, l’activation **à la fois** de la protection étendue et du chiffrement n’est pas prise en charge.

## <a name="see-also"></a>Voir aussi

[Connexion à SQL Server avec le pilote JDBC](../../connect/jdbc/connecting-to-sql-server-with-the-jdbc-driver.md)
