---
title: Installation de SSMA pour MySQL client (MySQLToSQL) | Microsoft Docs
description: En savoir plus sur les conditions préalables à l’installation pour le client Assistant Migration SQL Server (SSMA) pour MySQL et sur l’installation de.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Installing client,Licensing
ms.assetid: ede3128c-370d-45a5-a815-3d94eecaea30
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e2eaa9daf9128b43ac061cad66e7e993882e43d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070134"
---
# <a name="installing-ssma-for-mysql-client-mysqltosql"></a>Installation de SSMA pour MySQL client (MySQLToSQL)

Le client SSMA pour MySQL se compose des fichiers programme qui effectuent les tâches suivantes :

- Connectez-vous à une base de données MySQL.  
- Connectez-vous à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .
- Convertissez les objets de base de données MySQL en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] objets ou.
- Chargez les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .
- Migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

Cette rubrique décrit les conditions préalables à l’installation et fournit des instructions pour l’installation du client SSMA pour MySQL.

## <a name="prerequisites"></a>Prérequis

SSMA pour MySQL est conçu pour fonctionner avec MySQL 4,1 ou versions ultérieures, ainsi que pour toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012 ou version ultérieure, et [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

Avant d’installer SSMA, assurez-vous que l’ordinateur remplit les conditions suivantes :

- Windows 7 ou versions ultérieures, ou Windows Server 2008 ou versions ultérieures.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 ou une version ultérieure.
- La [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] version 4.7.2 ou une version ultérieure. Vous pouvez l’obtenir à partir du [Centre de développement .NET Framework](https://go.microsoft.com/fwlink/?LinkId=48882).
- Pilote ODBC 5,1 MySQL et connectivité avec les bases de données MySQL que vous souhaitez migrer. Vous pouvez installer MySQL à partir du site Web MySQL. Pour plus d’informations sur la connectivité, consultez [connexion à MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md).
- Accès à et autorisations suffisantes sur l’ordinateur qui héberge l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où vous allez migrer des objets et des données de base de données. Pour plus d’informations, consultez [connexion à SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md).
- Dans le cas de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] projets, accès à et autorisations suffisantes sur l’instance de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] où vous allez migrer des données et des objets de base de données. Pour plus d’informations, consultez [connexion à Azure SQL Database &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md).
- 4 Go de RAM recommandés.

## <a name="installing-ssma-for-mysql-client"></a>Installation du client SSMA pour MySQL

SSMA est téléchargeable sur le Web. Pour télécharger la dernière version, consultez la [page de téléchargement de Assistant Migration SQL Server](https://aka.ms/ssmaformysql).

Pour installer le client SSMA :

1. Double-cliquez sur **SSMAforMySQL_ *n*. msi**, où *n* est le numéro de Build.
2. Sur la page d’**accueil**, cliquez sur **Suivant**.

   Si la configuration requise n’est pas installée, un message s’affiche pour indiquer que vous devez d’abord installer les composants requis. Assurez-vous que vous avez installé toutes les conditions préalables avant d’exécuter à nouveau le programme d’installation.

3. Lisez le contrat de licence End-User. Si vous acceptez, sélectionnez **J’accepte le contrat**, puis cliquez sur **suivant**.
4. Dans la page **choisir le type d’installation** , cliquez sur par **défaut**.
5. Sur la page **prêt pour l’installation** , vous pouvez activer ou désactiver la télémétrie et les vérifications automatiques des mises à jour à chaque démarrage de l’outil. Cliquez sur **Installer** pour commencer l'installation.

> [!IMPORTANT]
> Désinstallez toutes les versions antérieures de SSMA pour MySQL avant d’installer la nouvelle version.

L’emplacement d’installation par défaut est `C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL`.

## <a name="see-also"></a>Voir également

- [Migration de bases de données MySQL vers SQL Server Azure SQL Database](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
