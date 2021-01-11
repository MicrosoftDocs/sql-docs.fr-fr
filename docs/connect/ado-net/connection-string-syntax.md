---
title: Syntaxe de la chaîne de connexion
description: En savoir plus sur la syntaxe des chaînes de connexion dans le Fournisseur de données Microsoft SqlClient pour SQL Server. La syntaxe de chaque fournisseur est documentée dans sa propriété ConnectionString.
ms.date: 11/13/2020
ms.assetid: 0977aeee-04d1-4cce-bbed-750c77fce06e
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 42b1f8704be721c0b52b3c42946c9abbf7efd7db
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771625"
---
# <a name="connection-string-syntax"></a>Syntaxe de la chaîne de connexion

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La <xref:Microsoft.Data.SqlClient> a un objet `Connection` qui hérite de <xref:System.Data.Common.DbConnection>, ainsi qu’une propriété <xref:System.Data.Common.DbConnection.ConnectionString%2A> spécifique au fournisseur. La syntaxe de chaîne de connexion spécifique pour le fournisseur SqlClient est documentée dans sa propriété `ConnectionString`. Pour plus d'informations sur la syntaxe de chaîne de connexion, consultez <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A>.

## <a name="connection-string-builders"></a>Générateurs de chaînes de connexion

 Le fournisseur de données Microsoft SqlClient pour SQL Server est présenté dans le générateur de chaîne de connexion suivant.

- <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>

Les générateurs de chaînes de connexion vous permettent de générer des chaînes de connexion valides lors de l'exécution, vous évitant ainsi d'avoir à concaténer manuellement les valeurs de chaîne dans votre code. Pour plus d’informations, consultez [Builders de chaînes de connexion](connection-string-builders.md).

## <a name="windows-authentication"></a>Authentification Windows

Il est recommandé d'utiliser l'authentification Windows, appelée parfois *sécurité intégrée*, pour se connecter aux sources de données qui la prennent en charge. La table suivante montre la syntaxe d’authentification Windows utilisée avec le Fournisseur de données Microsoft SqlClient pour SQL Server.

|Fournisseur|Syntaxe|  
|--------------|------------|  
|`SqlClient`|`Integrated Security=true;`<br /><br /> `-- or --`<br /><br /> `Integrated Security=SSPI;`|  

## <a name="sqlclient-connection-strings"></a>Chaînes de connexion SqlClient

La syntaxe pour une chaîne de connexion <xref:Microsoft.Data.SqlClient.SqlConnection> est documentée dans la propriété <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType>. La propriété <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> vous permet d'obtenir ou de définir une chaîne de connexion pour une base de données SQL Server. Les mots clés de la chaîne de connexion sont également mappés aux propriétés de <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>.

> [!IMPORTANT]
> La valeur par défaut du mot clé `Persist Security Info` est `false`. L'attribution au mot clé de la valeur `true` ou `yes` permet d'obtenir de la connexion des informations sensibles pour la sécurité, par exemple l'ID utilisateur et le mot de passe, une fois la connexion ouverte. En attribuant à `Persist Security Info` la valeur `false`, vous avez la garantie que la source qui n'est pas digne de confiance n'aura pas accès aux informations sensibles de la chaîne de connexion.

### <a name="windows-authentication-with-sqlclient"></a>Authentification Windows avec SqlClient

Chacun des formulaires de syntaxe suivants utilise l'authentification Windows pour se connecter à la base de données **AdventureWorks** sur un serveur local.

```csharp  
"Persist Security Info=False;Integrated Security=true;  
    Initial Catalog=AdventureWorks;Server=MSSQL1"  
"Persist Security Info=False;Integrated Security=SSPI;  
    database=AdventureWorks;server=(local)"  
"Persist Security Info=False;Trusted_Connection=True;  
    database=AdventureWorks;server=(local)"  
```  

### <a name="sql-server-authentication-with-sqlclient"></a>Authentification SQL Server avec SqlClient

L'authentification Windows est la solution préférée pour se connecter à SQL Server. Cependant, si l'authentification SQL Server est requise, utilisez la syntaxe suivante pour spécifier un nom d'utilisateur et un mot de passe.  Dans cet exemple, des astérisques servent à représenter un nom d'utilisateur et un mot de passe valides.

```csharp  
"Persist Security Info=False;User ID=*****;Password=*****;Initial Catalog=AdventureWorks;Server=MySqlServer"  
```  

Lorsque vous vous connectez à Azure SQL Database ou à Azure SQL Data Warehouse et fournissez une connexion au format `user@servername`, assurez-vous que la valeur `servername` de la connexion correspond à la valeur fournie pour `Server=`.

> [!NOTE]
> L'authentification Windows est prioritaire sur les comptes de connexion SQL Server. Si vous spécifiez Integrated Security=true ainsi qu'un nom d'utilisateur et un mot de passe, ces derniers seront ignorés pour utiliser l'authentification Windows.

### <a name="connect-to-a-named-instance-of-sql-server"></a>Se connecter à une instance nommée de SQL Server

Pour vous connecter à une instance nommée de SQL Server, utilisez la syntaxe du *nom du serveur\nom de l'instance*.

```csharp  
"Data Source=MySqlServer\MSSQL1;"  
```  

Vous pouvez également définir la propriété <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A> du `SqlConnectionStringBuilder` sur le nom d'instance lors de la création d'une chaîne de connexion. La propriété <xref:Microsoft.Data.SqlClient.SqlConnection.DataSource%2A> d'un objet <xref:Microsoft.Data.SqlClient.SqlConnection> est en lecture seule.

### <a name="type-system-version-changes"></a>Modifications de la version du système de type

Le mot clé `Type System Version` dans une propriété <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> spécifie la représentation côté client des types SQL Server. Pour plus d'informations sur le mot clé <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType>, consultez `Type System Version`.

## <a name="connect-and-attach-to-sql-server-express-user-instances"></a>Connexion et attachement à des instances utilisateur SQL Server Express

Les instances utilisateur sont une fonctionnalité de SQL Server Express. Elles permettent à un utilisateur qui s'exécute sur un compte Windows local disposant de privilèges minimum de se connecter à une base de données SQL Server et de l'exécuter sans nécessiter de privilèges d'administrateur. Une instance utilisateur s'exécute avec les informations d'identification Windows de l'utilisateur, pas en tant que service.

Pour plus d’informations sur l’utilisation des instances utilisateur, consultez [Instances utilisateur SQL Server Express](./sql/sql-server-express-user-instances.md).

## <a name="use-trustservercertificate"></a>Utiliser TrustServerCertificate

Le mot clé `TrustServerCertificate` est valide uniquement pour se connecter à une instance SQL avec un certificat valide. Lorsque `TrustServerCertificate` a la valeur `true`, la couche transport utilise TLS/SSL pour chiffrer le canal et contourner l'exploration de la chaîne de certificat pour valider l'approbation.

```csharp  
"TrustServerCertificate=true;"
```  

> [!NOTE]
> Si `TrustServerCertificate` a la valeur `true` et le chiffrement est activé, le niveau de chiffrement spécifié sur le serveur sera utilisé même si  `Encrypt` a la valeur `false` dans la chaîne de connexion. Sinon, la connexion échouera.

### <a name="enable-encryption"></a>Activer le chiffrement

Pour activer le chiffrement lorsqu'un certificat n'a pas été fourni sur le serveur, les options **Forcer le chiffrement du protocole** et **Faire confiance au certificat de serveur** doivent être définies dans le Gestionnaire de configuration SQL Server. Dans ce cas, le chiffrement utilise un certificat de serveur auto-signé sans validation si aucun certificat vérifiable n'a été fourni sur le serveur.

Les paramètres d'application ne peuvent pas réduire le niveau de la sécurité configurée dans SQL Server, mais peuvent éventuellement la renforcer. Une application peut demander le chiffrement en affectant aux mots clés `TrustServerCertificate` et `Encrypt` la valeur `true`, ce qui garantit le chiffrement même en l'absence d'un certificat de serveur et si l'option **Forcer le chiffrement du protocole** n'a pas été configurée pour le client. Toutefois, si `TrustServerCertificate` n'est pas activé dans la configuration cliente, un certificat de serveur fourni est toujours nécessaire.

Le tableau ci-dessous décrit tous les cas.

|Paramètre client Forcer le chiffrement du protocole|Paramètre client Faire confiance au certificat de serveur|Attribut/chaîne de connexion Chiffrer/Utiliser le chiffrement pour les données|Attribut/chaîne de connexion Faire confiance au certificat de serveur|Résultats|  
|----------------------------------------------|---------------------------------------------|-------------------------------------------------------------------|-----------------------------------------------------------|------------|  
|Non |N/A|Non (par défaut)|Ignoré|Aucun chiffrement ne se produit.|  
|Non |N/A|Oui|Non (par défaut)|Le chiffrement se produit uniquement s'il existe un certificat de serveur vérifiable ; sinon, la tentative de connexion échoue.|  
|Non |N/A|Oui|Oui|Le chiffrement se produit toujours, mais peut utiliser un certificat de serveur auto-signé.|  
|Oui|Non |Ignoré|Ignoré|Le chiffrement se produit uniquement s'il existe un certificat de serveur vérifiable ; sinon, la tentative de connexion échoue.|  
|Oui|Oui|Non (par défaut)|Ignoré|Le chiffrement se produit toujours, mais peut utiliser un certificat de serveur auto-signé.|  
|Oui|Oui|Oui|Non (par défaut)|Le chiffrement se produit uniquement s'il existe un certificat de serveur vérifiable ; sinon, la tentative de connexion échoue.|  
|Oui|Oui|Oui|Oui|Le chiffrement se produit toujours, mais peut utiliser un certificat de serveur auto-signé.|  

Pour plus d’informations, consultez [Utilisation du chiffrement sans validation](/sql/relational-databases/native-client/features/using-encryption-without-validation).

## <a name="see-also"></a>Voir aussi

- [Chaînes de connexion](connection-strings.md)
- [Connexion à une source de données](connecting-to-data-source.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
