---
title: Mise en route de la sécurité SQL Server sur Linux
description: Explorez les fonctionnalités de sécurité de SQL Server sur Linux pour obtenir un aperçu des zones à étudier plus en détail.
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ecc72850-8b01-492e-9a27-ec817648f0e0
ms.openlocfilehash: f4201aa6c07d4ae96d44d8aa443b23e275e1f9f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346412"
---
# <a name="walkthrough-for-the-security-features-of-sql-server-on-linux"></a>Procédure pas à pas pour les fonctionnalités de sécurité de SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Si vous êtes un utilisateur de Linux nouveau dans SQL Server, les tâches suivantes vous guident tout au long des tâches de sécurité. Celles-ci ne sont pas uniques ni spécifiques à Linux, mais elles vous permettent de vous faire une idée des domaines à examiner en détail. Dans chaque exemple, un lien est fourni vers la documentation détaillée de cette zone.

> [!NOTE]
>  Les exemples suivants utilisent l’exemple de base de données **AdventureWorks2014**. Pour obtenir des instructions sur l’obtention et l’installation de cet exemple de base de données, consultez [Restaurer une base de données SQL Server à partir de Windows vers Linux](sql-server-linux-migrate-restore-database.md).


## <a name="create-a-login-and-a-database-user"></a>Créer une connexion et un utilisateur de base de données 

Accordez à d’autres utilisateurs l’accès à SQL Server en créant une connexion dans la base de données MASTER à l’aide de l’instruction [CREATE LOGIN](../t-sql/statements/create-login-transact-sql.md). Par exemple :

```
CREATE LOGIN Larry WITH PASSWORD = '************';  
```

> [!NOTE]
>  Utilisez toujours un mot de passe fort à la place des astérisques dans la commande précédente.

Les connexions peuvent se connecter à SQL Server et avoir accès (avec des autorisations limitées) à la base de données MASTER. Pour se connecter à une base de données utilisateur, une connexion a besoin d’une identité correspondante au niveau de la base de données, appelé utilisateur de base de données. Les utilisateurs sont spécifiques à chaque base de données et doivent être créés séparément dans chaque base de données pour leur accorder l’accès. L’exemple suivant vous déplace dans la base de données AdventureWorks2014, puis utilise l’instruction [CREATE USER](../t-sql/statements/create-user-transact-sql.md) pour créer un utilisateur nommé Larry associé à la connexion nommée Larry. Bien que la connexion et l’utilisateur soient associés (mappés entre eux), il s’agit de différents objets. La connexion est un principal au niveau du serveur. L’utilisateur est un principal au niveau de la base de données.

```
USE AdventureWorks2014;
GO
CREATE USER Larry;
GO
```

- Un compte d’administrateur SQL Server peut se connecter à n’importe quelle base de données et peut créer des connexions et des utilisateurs supplémentaires dans n’importe quelle base de données.  
- Lorsqu’un utilisateur crée une base de données, il devient propriétaire de la base de données et peut se connecter à cette base de données. Les propriétaires de bases de données peuvent créer davantage d’utilisateurs.

Plus tard, vous pouvez autoriser d’autres connexions pour créer un plus grand nombre de connexions en leur accordant l’autorisation `ALTER ANY LOGIN`. Dans une base de données, vous pouvez autoriser d’autres utilisateurs à créer davantage d’utilisateurs en leur accordant l’autorisation `ALTER ANY USER`. Par exemple :   

```
GRANT ALTER ANY LOGIN TO Larry;   
GO   
   
USE AdventureWorks2014;   
GO   
GRANT ALTER ANY USER TO Jerry;    
GO   
```

À présent, la connexion Larry peut créer plus de connexions et l’utilisateur Jerry peut créer davantage d’utilisateurs.


## <a name="granting-access-with-least-privileges"></a>Octroi d’un accès avec des privilèges minimum

Les premières personnes qui se connectent à une base de données utilisateur seront les comptes administrateur et propriétaire de la base de données. Toutefois, ces utilisateurs disposent de toutes les autorisations disponibles sur la base de données. Il s’agit d’un nombre d’autorisations plus important que la plupart des utilisateurs. 

Lorsque vous commencez la prise en main, vous pouvez affecter des catégories d’autorisations générales à l’aide des *rôles de base de données fixes* intégrés. Par exemple, le rôle de base de données fixe `db_datareader` peut lire tous les tableaux de la base de données, mais ne peut pas apporter de modifications. Octroyez l’appartenance à un rôle de base de données fixe à l’aide de l’instruction [ALTER ROLE](../t-sql/statements/alter-role-transact-sql.md). L’exemple suivant ajoute l’utilisateur `Jerry` au rôle de base de données fixe `db_datareader`.   
   
```   
USE AdventureWorks2014;   
GO   
   
ALTER ROLE db_datareader ADD MEMBER Jerry;   
```   

Pour obtenir une liste des rôles de base de données fixes, consultez [Rôles au niveau de la base de données](../relational-databases/security/authentication-access/database-level-roles.md).

Plus tard, lorsque vous serez prêt à configurer un accès plus précis à vos données (fortement recommandé), créez vos propres rôles de base de données définis par l’utilisateur à l’aide de l’instruction [CREATE ROLE](../t-sql/statements/create-role-transact-sql.md). Attribuez ensuite des autorisations granulaires spécifiques à vos rôles personnalisés.

Par exemple, les instructions suivantes créent un rôle de base de données nommé `Sales`, octroie au groupe `Sales` la possibilité de consulter, de mettre à jour et de supprimer des lignes du tableau `Orders`, puis ajoute l’utilisateur `Jerry` au rôle `Sales`.   
   
```   
CREATE ROLE Sales;   
GRANT SELECT ON Object::Sales TO Orders;   
GRANT UPDATE ON Object::Sales TO Orders;   
GRANT DELETE ON Object::Sales TO Orders;   
ALTER ROLE Sales ADD MEMBER Jerry;   
```

Pour plus d’informations sur le système d’autorisations, voir [Débuter avec les autorisations du moteur de base de données](../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).


## <a name="configure-row-level-security"></a>Configurer la sécurité au niveau des lignes  

[La sécurité au niveau des lignes](../relational-databases/security/row-level-security.md) vous permet de limiter l’accès aux lignes d’une base de données en fonction de l’utilisateur qui exécute une requête. Cette fonctionnalité est utile pour des scénarios tels que la vérification que les clients ne peuvent accéder qu’à leurs propres données ou que les employés ne peuvent accéder qu’aux données pertinentes pour leur service.   

Les étapes suivantes vous guident dans la configuration de deux utilisateurs avec un accès au niveau des lignes différent à la table `Sales.SalesOrderHeader`. 

Créez deux comptes d’utilisateur pour tester la sécurité au niveau des lignes :    
   
```   
USE AdventureWorks2014;   
GO   
   
CREATE USER Manager WITHOUT LOGIN;     
   
CREATE USER SalesPerson280 WITHOUT LOGIN;    
```

Accordez l'accès en lecture sur la table `Sales.SalesOrderHeader` à chaque utilisateur :    
   
```   
GRANT SELECT ON Sales.SalesOrderHeader TO Manager;      
GRANT SELECT ON Sales.SalesOrderHeader TO SalesPerson280;
```
   
Créez un nouveau schéma et une fonction table inline. La fonction renvoie 1 lorsqu'une ligne de la colonne `SalesPersonID` est identique à l’ID d’une connexion `SalesPerson` ou si l'utilisateur exécutant la requête est l'utilisateur Manager.   
   
```     
CREATE SCHEMA Security;   
GO   
   
CREATE FUNCTION Security.fn_securitypredicate(@SalesPersonID AS int)     
    RETURNS TABLE   
WITH SCHEMABINDING   
AS     
   RETURN SELECT 1 AS fn_securitypredicate_result    
WHERE ('SalesPerson' + CAST(@SalesPersonId as VARCHAR(16)) = USER_NAME())     
    OR (USER_NAME() = 'Manager');    
```

Créez une stratégie de sécurité qui ajoute cette fonction en tant que prédicat de filtre et prédicat BLOCK sur la table :  

```
CREATE SECURITY POLICY SalesFilter   
ADD FILTER PREDICATE Security.fn_securitypredicate(SalesPersonID)    
  ON Sales.SalesOrderHeader,   
ADD BLOCK PREDICATE Security.fn_securitypredicate(SalesPersonID)    
  ON Sales.SalesOrderHeader   
WITH (STATE = ON);   
```

Exécutez la commande suivante pour interroger la table `SalesOrderHeader` pour chaque utilisateur. Vérifiez que `SalesPerson280` ne voit que les lignes 95 de leurs propres ventes et que `Manager` peut voir toutes les lignes de la table.  

```    
EXECUTE AS USER = 'SalesPerson280';   
SELECT * FROM Sales.SalesOrderHeader;    
REVERT; 
 
EXECUTE AS USER = 'Manager';   
SELECT * FROM Sales.SalesOrderHeader;   
REVERT;   
```
 
Modifiez la stratégie de sécurité pour désactiver la stratégie.  Désormais, les deux utilisateurs peuvent accéder à toutes les lignes. 

```
ALTER SECURITY POLICY SalesFilter   
WITH (STATE = OFF);    
``` 


## <a name="enable-dynamic-data-masking"></a>Activez le masquage des données dynamiques

Le [masquage des données dynamiques](../relational-databases/security/dynamic-data-masking.md) vous permet de limiter l’exposition des données sensibles aux utilisateurs d’une application en masquant entièrement ou partiellement certaines colonnes. 

Utilisez une instruction `ALTER TABLE` pour ajouter une fonction de masquage à la colonne `EmailAddress` de la table `Person.EmailAddress` : 
 
```
USE AdventureWorks2014;
GO
ALTER TABLE Person.EmailAddress    
ALTER COLUMN EmailAddress    
ADD MASKED WITH (FUNCTION = 'email()');
``` 
 
Créez un nouvel utilisateur `TestUser` avec `SELECT` autorisation sur la table, puis exécutez une requête en tant que `TestUser` pour afficher les données masquées :   

```  
CREATE USER TestUser WITHOUT LOGIN;   
GRANT SELECT ON Person.EmailAddress TO TestUser;    
 
EXECUTE AS USER = 'TestUser';   
SELECT EmailAddressID, EmailAddress FROM Person.EmailAddress;       
REVERT;    
```
 
Vérifiez que la fonction de masquage modifie l’adresse de messagerie dans le premier enregistrement de :
  
|EmailAddressID |EmailAddress |  
|----|---- |   
|1 |ken0@adventure-works.com |    
 
en 

|EmailAddressID |EmailAddress |  
|----|---- |   
|1 |kXXX@XXXX.com |   


## <a name="enable-transparent-data-encryption"></a>Activer le chiffrement transparent des données

Une menace pour votre base de données est le risque que quelqu’un vole les fichiers de la base de données sur votre disque dur. Cela peut se produire avec une intrusion qui obtient un accès élevé à votre système, par le biais des actions d’un employé problématique ou par le vol de l’ordinateur contenant les fichiers (par exemple, un ordinateur portable).

Transparent Data Encryption (TDE) chiffre les fichiers de données au fur et à mesure qu’ils sont stockés sur le disque dur. La base de données de référence du moteur de base de données SQL Server possède la clé de chiffrement, afin que le moteur de base de données puisse manipuler les données. Les fichiers de base de données ne peuvent pas être lus sans accès à la clé. Les administrateurs de haut niveau peuvent gérer, sauvegarder et recréer la clé, de sorte que la base de données puisse être déplacée, mais uniquement par les personnes sélectionnées. Lorsque TDE est configuré, la base de données `tempdb` est également chiffrée automatiquement. 

Étant donné que le Moteur de base de données peut lire les données, Transparent Data Encryption ne protège pas contre les accès non autorisés par les administrateurs de l’ordinateur qui peuvent lire directement la mémoire ou accéder à SQL Server via un compte d’administrateur.

### <a name="configure-tde"></a>Configurer TDE

- Créez une clé principale.
- Créez ou obtenez un certificat protégé par la clé principale.
- Créez une clé de chiffrement de base de données et protégez-la à l'aide du certificat.
- Configurez la base de données pour qu'elle utilise le chiffrement.

La configuration de TDE requiert `CONTROL` une autorisation sur la base de données de référence et `CONTROL` une autorisation sur la base de données utilisateur. En général, un administrateur configure TDE. 

L'exemple ci-dessous illustre le chiffrement et le déchiffrement de la base de données `AdventureWorks2014` à l'aide d'un certificat installé sur le serveur nommé `MyServerCert`.


```
USE master;  
GO  

CREATE MASTER KEY ENCRYPTION BY PASSWORD = '**********';  
GO  

CREATE CERTIFICATE MyServerCert WITH SUBJECT = 'My Database Encryption Key Certificate';  
GO  

USE AdventureWorks2014;  
GO
  
CREATE DATABASE ENCRYPTION KEY  
WITH ALGORITHM = AES_256  
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;  
GO
  
ALTER DATABASE AdventureWorks2014  
SET ENCRYPTION ON;   
```

Pour supprimer TDE, exécutez `ALTER DATABASE AdventureWorks2014 SET ENCRYPTION OFF;`   

Les opérations de chiffrement et de déchiffrement sont planifiées sur des threads d'arrière-plan par SQL Server. Vous pouvez consulter l'état de ces opérations à l'aide des affichages catalogue et des vues de gestion dynamique mentionnés dans la liste fournie plus loin dans cette rubrique.   

> [!WARNING]
>  Les fichiers de sauvegarde des bases de données pour lesquelles le chiffrement transparent des données est activé sont également chiffrés à l'aide de la clé de chiffrement de base de données. En conséquence, lorsque vous restaurez ces sauvegardes, le certificat qui protège la clé de chiffrement de base de données doit être disponible. Cela signifie qu'en plus de sauvegarder la base de données, vous devez vous assurer que vous conservez des sauvegardes des certificats du serveur pour empêcher toute perte de données. Une perte de données interviendra si le certificat n'est plus disponible. Pour plus d'informations, consultez [SQL Server Certificates and Asymmetric Keys](../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  

Pour plus d’informations sur le chiffrement des bases de données, consultez l’article [Chiffrement TDE (Transparent Data Encryption)](../relational-databases/security/encryption/transparent-data-encryption.md).   


## <a name="configure-backup-encryption"></a>Configurer le chiffrement de sauvegarde
SQL Server peut chiffrer les données lors de la création d’une sauvegarde. En spécifiant l'algorithme de chiffrement et le chiffreur (un certificat ou une clé asymétrique) lors de la création d'une sauvegarde, vous créez un fichier de sauvegarde chiffré.    
  
> [!WARNING]
> Il est très important de sauvegarder le certificat ou la clé asymétrique, et de préférence dans un emplacement autre que le fichier de sauvegarde pour lequel il a été utilisé pour le chiffrement. Sans certificat ou clé asymétrique, vous ne pouvez pas restaurer la sauvegarde, ce qui rend le fichier de sauvegarde inutilisable. 
 
 
L’exemple suivant crée un certificat, puis crée une sauvegarde protégée par le certificat.

```
USE master;  
GO  
CREATE CERTIFICATE BackupEncryptCert   
   WITH SUBJECT = 'Database backups';  
GO 
BACKUP DATABASE [AdventureWorks2014]  
TO DISK = N'/var/opt/mssql/backups/AdventureWorks2014.bak'  
WITH  
  COMPRESSION,  
  ENCRYPTION   
   (  
   ALGORITHM = AES_256,  
   SERVER CERTIFICATE = BackupEncryptCert  
   ),  
  STATS = 10  
GO  
```

Pour plus d’informations, voir [Chiffrement de sauvegarde](../relational-databases/backup-restore/backup-encryption.md).


## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les fonctionnalités de sécurité disponibles dans SQL Server, consultez le [Security Center pour le moteur de base de données SQL Server et Azure SQL Database](../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md).