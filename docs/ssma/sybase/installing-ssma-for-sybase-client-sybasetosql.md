---
title: Installation de SSMA pour le client SAP ASE (SybaseToSQL) | Microsoft Docs
description: En savoir plus sur les conditions préalables à l’installation de Assistant Migration SQL Server (SSMA) pour SAP Adaptive Server Enterprise (ASE) et sur l’installation de.
ms.custom: ''
ms.date: 07/14/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: e770c2f2-52b9-4471-a207-0d35df41399c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d9b20774b0a32b0d9d007053bedd2674e79d4e12
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015359"
---
# <a name="installing-ssma-for-sap-ase-client-sybasetosql"></a>Installation de SSMA pour le client SAP ASE (SybaseToSQL)

Le client SSMA se compose des fichiers programme utilisés pour se connecter à un serveur de base de données SAP Adaptive Server Enterprise (ASE) et une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , convertir des objets de base de données ASE en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe ou, [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] charger les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , puis migrer les données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

Cette rubrique décrit les conditions préalables à l’installation et les instructions pour l’installation de SSMA.

## <a name="prerequisites"></a>Prérequis

SSMA est conçu pour fonctionner avec SAP ASE 11.9.2 ou versions ultérieures, ainsi que toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Avant d’installer SSMA, assurez-vous que l’ordinateur remplit les conditions suivantes :

- Windows 7 ou versions ultérieures, ou Windows Server 2008 ou versions ultérieures.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 ou une version ultérieure.
- La [!INCLUDE[msCoName](../../includes/msconame_md.md)] version de .NET Framework 4.7.2 ou une version ultérieure. Vous pouvez l’obtenir à partir du [Centre de développement .NET Framework](https://go.microsoft.com/fwlink/?LinkId=48882).
- Le fournisseur Sybase OLE DB/ADO.Net/ODBC et la connectivité au serveur de base de données SAP ASE qui contient les bases de données que vous souhaitez migrer. Vous pouvez installer des fournisseurs à partir du support produit SAP ASE. Pour plus d’informations sur la connectivité, consultez [connexion à Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md).
- Accès et autorisations suffisantes sur l’ordinateur qui héberge l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou sur [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] lequel vous allez migrer des objets et des données de base de données. Pour plus d’informations, consultez [connexion à SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sql-server-sybasetosql.md) / [connexion à Azure SQL Database &#40;SybaseToSQL ](../../ssma/sybase/connecting-to-azure-sql-db-sybasetosql.md)&#41;.
- 4 Go de RAM recommandés.

## <a name="installing-the-ssma-for-sybase-client"></a>Installation du client SSMA pour Sybase

SSMA est téléchargeable sur le Web. Pour télécharger la dernière version, consultez la [page de téléchargement de Assistant Migration SQL Server](https://aka.ms/ssmaforsybase).

Pour installer le client SSMA :

1. Double-cliquez sur **SSMAforSybase_ *n*. msi**, où *n* est le numéro de Build.
2. Sur la page d'accueil, cliquez sur **Suivant**.

   Si la configuration requise n’est pas installée, un message s’affiche pour indiquer que vous devez d’abord installer les composants requis. Assurez-vous que vous avez installé tous les composants requis, puis réexécutez le programme d’installation.

3. Lisez le contrat de licence End-User. Si vous acceptez, sélectionnez **J’accepte le contrat**, puis cliquez sur **suivant**.
4. Dans la page choisir le type d’installation, cliquez sur par **défaut**.
5. Sur la page **prêt pour l’installation** , vous pouvez activer ou désactiver la télémétrie et les vérifications automatiques des mises à jour à chaque démarrage de l’outil. Cliquez sur **Installer** pour commencer l'installation.

> [!IMPORTANT]
> Désinstallez toutes les versions antérieures de SSMA pour Sybase avant d’installer la nouvelle version.

L’emplacement d’installation par défaut est `C:\Program Files\Microsoft SQL Server Migration Assistant for Sybase`.

En plus des fichiers programme SSMA, vous devez également installer SSMA pour le pack d’extension Sybase sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Installing SSMA Components on SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md).

## <a name="see-also"></a>Voir également

- [Installation des composants SSMA sur SQL Server](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)  
- [Migration de bases de données Sybase ASE vers SQL Server Azure SQL Database](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)
