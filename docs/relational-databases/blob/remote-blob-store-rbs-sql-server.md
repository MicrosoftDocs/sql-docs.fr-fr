---
title: Magasin d’objets blob distants (RBS) (SQL Server) | Microsoft Docs
description: Le Magasin d’objets blob distants (RBS) SQL Server stocke les objets blob dans un stockage universel et non sur le serveur de base de données principal. Découvrez ce composant additionnel.
ms.custom: ''
ms.date: 11/03/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- Remote Blob Store (RBS) [SQL Server]
- RBS (Remote Blob Store) [SQL Server]
ms.assetid: 31c947cf-53e9-4ff4-939b-4c1d034ea5b1
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 89a79b07977d8fec2e2dd6f2a8b9acd9cea0ba5e
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100343160"
---
# <a name="remote-blob-store-rbs-sql-server"></a>Magasin d'objets blob distants (RBS) (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Le magasin d'objets blob distants[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est un composant additionnel facultatif qui permet aux administrateurs de base de données de stocker des objets blob dans des solutions de stockage de marchandises au lieu de les stocker directement sur le serveur de base de données principal.  
  
 RBS est inclus dans le CD d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , mais n'est pas installé par le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Recherchez RBS.msi sur le support d’installation pour localiser le fichier d’installation.

 Si vous ne disposez pas d’un support d’installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez télécharger RBS à l’un des emplacements suivants :

| Version [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] | Emplacement de téléchargement de RBS |
|:---|:---|
| [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] | [[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=56833) |
| [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] | [[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] Feature Pack](https://www.microsoft.com/download/details.aspx?id=55992) |
| [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] | [Page de téléchargement [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] RBS](https://go.microsoft.com/fwlink/?linkid=2109005) |
| &nbsp; | &nbsp; |
  
 
  
## <a name="why-rbs"></a>Pourquoi RBS ?  
  
### <a name="optimized-database-storage-and-performance"></a>Performances et stockage de base de données optimisés  
 Le stockage d'objets blob dans la base de données peut consommer une grande quantité d'espace de fichiers et se révéler coûteuse du point de vue des ressources de serveur. RBS transmet les objets blob à la solution de stockage de votre choix, et en stocke les références dans la base de données. Cela permet de libérer de l'espace de stockage serveur pour les données structurées, ainsi que des ressources serveur pour les opérations de base de données.  
  
### <a name="efficient-blob-management"></a>Gestion efficace des objets blob  
 Plusieurs fonctionnalités de RBS prennent en charge la gestion des objets blob stockés :  
  
-   Les objets blob sont gérés à l’aide des transactions ACID (Atomicité, Cohérence, Isolation et Durabilité).  
  
-   Les objets blob sont organisés en collections.  
  
-   Le nettoyage de la mémoire, la vérification de la cohérence et les autres fonctions de maintenance y sont inclus.  
  
### <a name="standardized-api"></a>API standardisée  
 RBS définit un ensemble d'API qui fournit un modèle de programmation standardisé permettant aux applications d'accéder à tous les magasins d'objets blob et de les modifier. Chaque magasin d’objets blob peut spécifier sa propre bibliothèque de fournisseurs qui se connecte à la bibliothèque cliente RBS et indique comment les objets blob sont stockés et accessibles.  
  
 Certains fournisseurs de solutions de stockage tiers ont développé des fournisseurs RBS qui sont conformes à ces API standard et qui prennent en charge le stockage d'objets blob sur différentes plateformes de stockage.  
  
## <a name="rbs-requirements"></a>Conditions requises du magasin d'objets blob distants (RBS)  
 - Le magasin d'objets blob distants nécessite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise pour le serveur de base de données principal sur lequel les métadonnées d'objets blob sont stockées.  Cependant, si vous utilisez le fournisseur FILESTREAM fourni, vous pouvez stocker les objets blob sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard. Pour vous connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], RBS nécessite au moins la version 11 du pilote ODBC pour [!INCLUDE[ssSQL14_md](../../includes/sssql14-md.md)] et la version 13 du pilote ODBC pour [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)]. Les pilotes sont disponibles à la page [Download ODBC Driver for SQL Server (Télécharger le pilote ODBC pour SQL Server)](../../connect/odbc/download-odbc-driver-for-sql-server.md).    
  
 Le magasin d'objets blob distants comprend un fournisseur FILESTREAM qui vous permet de stocker les objets blob sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous souhaitez utiliser le magasin d'objets blob distants pour stocker des objets blob dans une solution de stockage différente, vous devez utiliser un fournisseur RBS tiers, qui aura été développé pour cette solution de stockage particulière, ou bien développer un fournisseur RBS personnalisé à l'aide de l'API RBS. Un exemple de fournisseur stockant des objets blob sur un système de fichiers NTFS est disponible parmi les ressources pédagogiques du site [CodePlex](https://go.microsoft.com/fwlink/?LinkId=210190).  
  
## <a name="rbs-security"></a>Sécurité relative au magasin d'objets blob distants (RBS)  
 Le blog de l'équipe de stockage d’objets blob distants SQL est une bonne source d'informations sur cette fonctionnalité. Le modèle de sécurité RBS est décrit dans la partie [Modèle de sécurité RBS](/archive/blogs/sqlrbs/rbs-security-model)de la publication.  
  
### <a name="custom-providers"></a>Fournisseurs personnalisés  
 Lorsque vous utilisez un fournisseur personnalisé pour stocker des objets blob en dehors de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], assurez-vous de protéger les objets blob stockés à l'aide d'autorisations et d'options de chiffrement convenant au support de stockage utilisé par le fournisseur personnalisé.  
  
### <a name="credential-store-symmetric-key"></a>Clé symétrique du magasin d'informations d'identification  
 Si un fournisseur demande l’installation et l’utilisation d’un secret stocké dans le magasin d’informations d’identification, RBS utilise une clé symétrique pour chiffrer les secrets du fournisseur qu’un client peut utiliser pour obtenir l’autorisation d’accès au magasin d’objets blob du fournisseur.  
  
-   RBS 2016 utilise une clé symétrique **AES_128** . [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] n’autorise pas la création de nouvelles clés **TRIPLE_DES** , sauf pour des raisons de compatibilité descendante. Pour plus d’informations, consultez [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md).  
  
-   RBS 2014 et les versions antérieures utilisent un magasin d’informations d’identification qui maintient le chiffrement des clés secrètes à l’aide de l’algorithme de clé symétrique **TRIPLE_DES** , obsolète. Si vous utilisez **TRIPLE_DES**[!INCLUDE[msCoName](../../includes/msconame-md.md)] , nous vous recommandons d’améliorer votre sécurité en suivant les étapes décrites dans cette rubrique pour permuter votre clé vers une méthode de chiffrement plus forte.  
  
 Pour déterminer les propriétés des clés symétriques du magasin d’informations d’identification RBS, vous pouvez exécuter l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante dans la base de données RBS :   
`SELECT * FROM sys.symmetric_keys WHERE name = 'mssqlrbs_encryption_skey';` Si la sortie de cette instruction indique que **TRIPLE_DES** est toujours utilisé, vous devez permuter cette clé.  
  
### <a name="rotating-the-symmetric-key"></a>Rotation de la clé symétrique  
 Lorsque vous utilisez RBS, la clé symétrique du magasin d'informations d'identification doit régulièrement faire l’objet d’une rotation. Il s'agit d’une meilleure pratique de sécurité courante permettant de suivre les stratégies de sécurité de l'organisation.  Pour effectuer la rotation de la clé symétrique du magasin d’informations d'identification RBS, il est possible d’utiliser le [script ci-dessous](#Key_rotation) dans la base de données RBS.  Vous pouvez également utiliser ce script pour migrer vers des propriétés de chiffrement plus fortes, notamment la longueur de l'algorithme ou de la clé. Sauvegardez votre base de données avant d’effectuer la rotation de la clé.  Le script comprend à la fin quelques étapes de vérification.  
Si vos stratégies de sécurité nécessitent d’autres propriétés de clé (par exemple, la longueur de l’algorithme ou de la clé) que celles qui sont fournies, le script peut être utilisé comme modèle. Modifiez les propriétés de la clé à deux endroits : 1) la création de la clé temporaire 2) la création de la clé permanente.  
  
##  <a name="rbs-resources"></a><a name="rbsresources"></a> Ressources RBS  
  
 **Exemples RBS**  
 Les exemples de magasins d'objets blob distants (RBS) disponibles sur le site [CodePlex](https://go.microsoft.com/fwlink/?LinkId=210190) montrent comment développer une application RBS et comment développer et installer un fournisseur RBS personnalisé.  
  
 **Blog RBS**  
 Le [blog du magasin d'objets blob distants (RBS)](/archive/blogs/sqlrbs/) fournit des informations supplémentaires qui vous aideront à mieux comprendre, déployer et gérer les magasins d'objets blob distants.  
  
##  <a name="key-rotation-script"></a><a name="Key_rotation"></a> Script de permutation des clés  
 Cet exemple crée une procédure stockée nommée `sp_rotate_rbs_symmetric_credential_key` pour remplacer la clé symétrique du magasin d’informations d’identification RBS actuellement utilisée  
par celle de votre choix.  Ce remplacement est préférable s’il existe une stratégie de sécurité qui exige   
une permutation des clés régulière ou si des algorithmes spécifiques le précisent.  
 Dans cette procédure stockée, une clé symétrique utilisant **AES_256** remplace l’actuelle.  À la suite du  
remplacement de la clé symétrique, les clés secrètes doivent être chiffrées de nouveau avec la nouvelle clé.  Cette procédure   
stockée rechiffre également les clés secrètes.  La base de données doit être sauvegardée avant toute rotation de clé.  
  
```  
CREATE PROC sp_rotate_rbs_symmetric_credential_key  
AS  
BEGIN  
BEGIN TRANSACTION;  
BEGIN TRY  
CLOSE ALL SYMMETRIC KEYS;  
  
/* Prove that all secrets can be re-encrypted, by creating a   
temporary key (#mssqlrbs_encryption_skey) and create a   
temp table (#myTable) to hold the re-encrypted secrets.    
Check to see if all re-encryption worked before moving on.*/  
  
CREATE TABLE #myTable(sql_user_sid VARBINARY(85) NOT NULL,  
    blob_store_id SMALLINT NOT NULL,  
    credential_name NVARCHAR(256) COLLATE Latin1_General_BIN2 NOT NULL,  
    old_secret VARBINARY(MAX), -- holds secrets while existing symmetric key is deleted  
    credential_secret VARBINARY(MAX)); -- holds secrets with the new permanent symmetric key  
  
/* Create a new temporary symmetric key with which the credential store secrets   
can be re-encrypted. These will be used once the existing symmetric key is deleted.*/  
CREATE SYMMETRIC KEY #mssqlrbs_encryption_skey    
    WITH ALGORITHM = AES_256 ENCRYPTION BY   
    CERTIFICATE [cert_mssqlrbs_encryption];  
  
OPEN SYMMETRIC KEY #mssqlrbs_encryption_skey    
    DECRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
INSERT INTO #myTable   
    SELECT cred_store.sql_user_sid, cred_store.blob_store_id, cred_store.credential_name,   
    encryptbykey(  
        key_guid('#mssqlrbs_encryption_skey'),   
        decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'),   
            NULL, cred_store.credential_secret)  
        ),   
    NULL  
    FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials] AS cred_store;  
  
IF( EXISTS(SELECT * FROM #myTable WHERE old_secret IS NULL))  
BEGIN  
    PRINT 'Abort. Failed to read some values';  
    SELECT * FROM #myTable;  
    ROLLBACK;  
END;  
ELSE  
BEGIN  
/* Re-encryption worked, so go ahead and drop the existing RBS credential store   
 symmetric key and replace it with a new symmetric key.*/  
DROP SYMMETRIC KEY [mssqlrbs_encryption_skey];  
  
CREATE SYMMETRIC KEY [mssqlrbs_encryption_skey]   
WITH ALGORITHM = AES_256   
ENCRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
OPEN SYMMETRIC KEY [mssqlrbs_encryption_skey]   
DECRYPTION BY CERTIFICATE [cert_mssqlrbs_encryption];  
  
/*Re-encrypt using the new permanent symmetric key.    
Verify if encryption provided a result*/  
UPDATE #myTable   
SET [credential_secret] =   
    encryptbykey(key_guid('mssqlrbs_encryption_skey'), decryptbykey(old_secret))  
  
IF( EXISTS(SELECT * FROM #myTable WHERE credential_secret IS NULL))  
BEGIN  
    PRINT 'Aborted. Failed to re-encrypt some values'  
    SELECT * FROM #myTable  
    ROLLBACK  
END  
ELSE  
BEGIN  
  
/* Replace the actual RBS credential store secrets with the newly   
encrypted secrets stored in the temp table #myTable.*/                
SET NOCOUNT ON;  
DECLARE @sql_user_sid varbinary(85);  
DECLARE @blob_store_id smallint;  
DECLARE @credential_name varchar(256);  
DECLARE @credential_secret varbinary(256);  
DECLARE curSecretValue CURSOR   
    FOR SELECT sql_user_sid, blob_store_id, credential_name, credential_secret   
FROM #myTable ORDER BY sql_user_sid, blob_store_id, credential_name;  
  
OPEN curSecretValue;  
FETCH NEXT FROM curSecretValue   
    INTO @sql_user_sid, @blob_store_id, @credential_name, @credential_secret  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    UPDATE [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
        SET [credential_secret] = @credential_secret   
        FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
        WHERE sql_user_sid = @sql_user_sid AND blob_store_id = @blob_store_id AND   
            credential_name = @credential_name  
FETCH NEXT FROM curSecretValue   
    INTO @sql_user_sid, @blob_store_id, @credential_name, @credential_secret  
END  
CLOSE curSecretValue  
DEALLOCATE curSecretValue  
  
DROP TABLE #myTable;  
CLOSE ALL SYMMETRIC KEYS;  
DROP SYMMETRIC KEY #mssqlrbs_encryption_skey;  
  
/* Verify that you can decrypt all encrypted credential store entries using the certificate.*/  
IF( EXISTS(SELECT * FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials]   
WHERE decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'),   
    NULL, credential_secret) IS NULL))  
BEGIN  
    print 'Aborted. Failed to verify key rotation'  
    ROLLBACK;  
END;  
ELSE  
    COMMIT;  
END;  
END;  
END TRY  
BEGIN CATCH  
     PRINT 'Exception caught: ' + cast(ERROR_NUMBER() as nvarchar) + ' ' + ERROR_MESSAGE();  
     ROLLBACK  
END CATCH  
END;  
GO  
```  
  
 Vous pouvez désormais utiliser la procédure stockée `sp_rotate_rbs_symmetric_credential_key` pour effectuer la rotation de la clé symétrique du magasin d’informations d’identification RBS ; les clés secrètes restent les mêmes avant et après la rotation de la clé.  
  
```  
SELECT *, decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'), NULL, credential_secret)   
FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials];  
  
EXEC sp_rotate_rbs_symmetric_credential_key;  
  
SELECT *, decryptbykeyautocert(cert_id('cert_mssqlrbs_encryption'), NULL, credential_secret)   
FROM [mssqlrbs_resources].[rbs_internal_blob_store_credentials];  
  
/* See that the RBS credential store symmetric key properties reflect the new changes*/  
SELECT * FROM sys.symmetric_keys WHERE name = 'mssqlrbs_encryption_skey';  
```  
  
## <a name="see-also"></a>Voir aussi  
[Magasin d’objets blob distants et groupes de disponibilité Always On (SQL Server)](../../database-engine/availability-groups/windows/remote-blob-store-rbs-and-always-on-availability-groups-sql-server.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
