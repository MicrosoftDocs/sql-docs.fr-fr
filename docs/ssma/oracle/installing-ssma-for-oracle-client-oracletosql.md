---
title: Installation de SSMA pour Oracle client (OracleToSQL) | Microsoft Docs
description: En savoir plus sur les conditions préalables à l’installation de l’Assistant Migration SQL Server (SSMA) pour le client Oracle et sur l’installation de.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: d5d4903d-e296-4bbf-8780-63674c4d62d5
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 5bed1589c0852acc2d782028f6b9cc253301babe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073489"
---
# <a name="installing-ssma-for-oracle-client-oracletosql"></a>Installation de SSMA pour Oracle client (OracleToSQL)

Le client SSMA se compose des fichiers programme qui effectuent les tâches suivantes :  
  
- Connectez-vous à une base de données Oracle.
- Connectez-vous à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Convertir des objets de base de données Oracle en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe.
- Chargez les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
- Migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Cette rubrique décrit les conditions préalables à l’installation et les instructions pour l’installation de SSMA.

## <a name="prerequisites"></a>Prérequis

SSMA est conçu pour fonctionner avec Oracle 9 ou versions ultérieures, ainsi que toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Avant d’installer SSMA, assurez-vous que l’ordinateur remplit les conditions suivantes :

- Windows 7 ou versions ultérieures, ou Windows Server 2008 ou versions ultérieures.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 ou une version ultérieure.
- La [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] version 4.7.2 ou une version ultérieure. Vous pouvez l’obtenir à partir du [Centre de développement .NET Framework](https://go.microsoft.com/fwlink/?LinkId=48882).
- Connectivité aux bases de données Oracle que vous souhaitez migrer.
- Accès et autorisations suffisantes sur l’ordinateur qui héberge l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou sur [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] lequel vous allez migrer des objets et des données de base de données. Pour plus d’informations, consultez [connexion à SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-sql-server-oracletosql.md).
- 4 Go de RAM recommandés.  
  
## <a name="installing-the-ssma-for-oracle-client"></a>Installation du client SSMA pour Oracle

SSMA est téléchargeable sur le Web. Pour télécharger la dernière version, consultez la [page de téléchargement de Assistant Migration SQL Server](https://aka.ms/ssmafororacle).

Pour installer le client SSMA :

1. Double-cliquez sur **SSMAforOracle_ *n*. msi**, où *n* est le numéro de Build.
2. Sur la page d'accueil, cliquez sur **Suivant**.

   Si la configuration requise n’est pas installée, un message s’affiche pour indiquer que vous devez d’abord installer les composants requis. Assurez-vous que vous avez installé tous les composants requis, puis réexécutez le programme d’installation.  

3. Lisez le contrat de licence End-User. Si vous acceptez, sélectionnez **J’accepte le contrat**, puis cliquez sur **suivant**.
4. Dans la page **choisir le type d’installation** , cliquez sur par **défaut**.
5. Sur la page **prêt pour l’installation** , vous pouvez activer ou désactiver la télémétrie et les vérifications automatiques des mises à jour à chaque démarrage de l’outil. Cliquez sur **Installer** pour commencer l'installation.

> [!IMPORTANT]
> Désinstallez toutes les versions antérieures de SSMA pour Oracle avant d’installer la nouvelle version.

L’emplacement d’installation par défaut est `C:\Program Files\Microsoft SQL Server Migration Assistant for Oracle`.

En plus des fichiers programme SSMA, vous devez également installer SSMA pour Oracle extension Pack sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [installation des composants SSMA sur SQL Server](../../ssma/oracle/installing-ssma-components-on-sql-server-oracletosql.md).

## <a name="see-also"></a>Voir également

- [Installation des composants SSMA sur SQL Server](../../ssma/oracle/installing-ssma-components-on-sql-server-oracletosql.md)
- [Migration de bases de données Oracle vers SQL Server](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)
