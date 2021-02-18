---
title: Guide d’utilisation du TDE (Transparent Data Encryption) au repos des Clusters Big Data SQL Server
titleSuffix: SQL Server Big Data Clusters
description: Cet article explique comment utiliser la fonction de chiffrement TDE SQL Server au repos du BDC
author: DaniBunny
ms.author: dacoelho
ms.reviewer: mihaelab
ms.date: 10/19/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7d249f9b70479603ad66e6b68cec784b1476324a
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489313"
---
# <a name="sql-server-big-data-clusters-transparent-data-encryption-tde-at-rest-usage-guide"></a>Guide d’utilisation du TDE (Transparent Data Encryption) au repos des Clusters Big Data SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Ce guide montre comment utiliser les fonctionnalités de chiffrement au repos des Clusters Big Data SQL Server pour chiffrer les bases de données.

L’expérience est en général identique à SQL Server sur Linux et la [documentation TDE standard](../relational-databases/security/encryption/transparent-data-encryption.md) s’applique, sauf mention contraire. Pour analyser l’état du chiffrement sur principal, suivez les modèles de requête DMV standard au dessus de [`sys.dm_database_encryption_keys`](../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) et [`sys.certificates`](../relational-databases/system-catalog-views/sys-certificates-transact-sql.md).

__Fonctionnalités non prises en charge :__
* Chiffrement du pool de données

## <a name="prerequisites"></a><a id="prereqs"></a> Conditions préalables

- [Cluster Big Data SQL Server CU8+](release-notes-big-data-cluster.md)
- [Outils Big Data](deploy-big-data-tools.md)
   - **Azure Data Studio**
- Utilisateur SQL Server avec des privilèges administratifs.

## <a name="query-the-installed-certificates"></a>Interroger les certificats installés

1. Dans Azure Data Studio, connectez-vous à l’instance maître SQL Server de votre cluster Big Data. Pour plus d’informations, consultez [Se connecter à l’instance maître SQL Server](connect-to-big-data-cluster.md#master).

1. Double-cliquez sur la connexion dans la fenêtre **Serveurs** pour afficher le tableau de bord de serveur de l’instance maître SQL Server. Sélectionnez **Nouvelle requête**.

   ![Requête d’instance maître SQL Server](./media/tutorial-data-pool-ingest-sql/sql-server-master-instance-query.png)

1. Exécutez la commande Transact-SQL suivante pour remplacer le contexte par celui de la base de données **MASTER** dans l’instance maître.

   ```sql
   USE master
   GO
   ```

1. Interrogez les certificats managés par le système installés. 

   ```sql
    SELECT TOP 1 name FROM sys.certificates WHERE name LIKE 'TDECertificate%' ORDER BY name DESC
   ```

    Utilisez des critères de requête différents selon vos besoins.

    Le nom du certificat est indiqué sous la forme « TDECertificate {timestamp} ». Quand vous voyez un préfixe TDECertificate suivi de timestamp, il s’agit du certificat fourni par la fonctionnalité managée par le système.

## <a name="encrypt-a-database-using-the-system-provided-certificate"></a>Chiffrer une base de données à l’aide du certificat fourni par le système

Dans les exemples suivants, considérons une base de données nommée __userdb__ en tant que cible pour le chiffrement et un certificat fourni par le système nommé __TDECertificate2020_09_15_22_46_27__ par sortie de la section précédente.

1. Utilisez le modèle suivant pour générer une clé de chiffrement de base de données à l’aide du certificat du système fourni.

   ```sql
    USE userdb; 
    GO
    CREATE DATABASE ENCRYPTION KEY WITH ALGORITHM = AES_256 ENCRYPTION BY SERVER CERTIFICATE TDECertificate2020_09_15_22_46_27;
    GO
   ```

1. Chiffrez la base de données __userdb__ à l’aide de la commande suivante.

   ```sql
    ALTER DATABASE userdb SET ENCRYPTION ON;
    GO
   ```

## <a name="next-steps"></a>Étapes suivantes

Apprenez-en davantage sur le chiffrement au repos pour HDFS :
> [!div class="nextstepaction"]
> [Zones de chiffrement HDFS](encryption-at-rest-hdfs-encryption-zones.md)
