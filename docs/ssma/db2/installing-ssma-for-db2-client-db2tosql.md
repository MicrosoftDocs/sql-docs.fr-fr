---
title: Installation de SSMA pour le client DB2 (DB2ToSQL) | Microsoft Docs
description: En savoir plus sur les conditions préalables à l’installation de l’Assistant Migration SQL Server (SSMA) pour le client DB2 et sur l’installation de.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 3ae2a470-6afd-4512-b6d1-fcbe6afe88ad
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: dd9c4116acd482fa12b74556b9d1ad1fe9b06cdd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014729"
---
# <a name="installing-ssma-for-db2-client-db2tosql"></a>Installation de SSMA pour le client DB2 (DB2ToSQL)

Le client SSMA se compose des fichiers programme qui effectuent les tâches suivantes :

- Connectez-vous à une base de données DB2.
- Connectez-vous à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Convertit les objets de base de données DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe.
- Chargez les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
- Migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Cette rubrique décrit les conditions préalables à l’installation et les instructions pour l’installation de SSMA.

## <a name="prerequisites"></a>Prérequis

SSMA est conçu pour fonctionner avec DB2 sur z/OS version 9,0 et 10,0, DB2 sur LUW version 9,8 et 10,1 ou versions ultérieures, DB2 pour i version 7,1 ou supérieure et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012 ou versions ultérieures.

Avant d’installer SSMA, assurez-vous que l’ordinateur remplit les conditions suivantes :

- Windows 7 ou versions ultérieures, ou Windows Server 2008 ou versions ultérieures.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 ou versions ultérieures.
- La [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] version 4.7.2 ou une version ultérieure. Vous pouvez l’obtenir à partir du [Centre de développement .NET Framework](https://go.microsoft.com/fwlink/?LinkId=48882).
- Fournisseur OLE DB Microsoft pour DB2 version 5 ou ultérieure, ainsi que la connectivité aux bases de données DB2 que vous souhaitez migrer.
- Accès et autorisations suffisantes sur l’ordinateur qui héberge l’instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database où vous allez migrer les données et les objets de base de données. Pour plus d’informations, consultez [connexion à SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-sql-server-db2tosql.md).
- 4 Go de RAM recommandés.

## <a name="microsoft-ole-db-provider-for-db2"></a>Fournisseur Microsoft OLE DB pour DB2

Pour télécharger le fournisseur OLE DB pour DB2 version 6,0, accédez à [Microsoft® SQL Server® 2017 Feature Pack](https://www.microsoft.com/download/details.aspx?id=55992).

SSMA est téléchargeable sur le Web. Pour télécharger la dernière version, consultez la [page de téléchargement de Assistant Migration SQL Server](https://aka.ms/ssmafordb2).

Pour installer le client SSMA :

1. Double-cliquez sur **SSMAforDB2_ *n*. msi**, où *n* est le numéro de Build.
2. Sur la page d’**accueil**, sélectionnez **Suivant**.

   Si la configuration requise n’est pas installée, un message s’affiche pour indiquer que vous devez d’abord installer les composants requis. Assurez-vous que vous avez installé tous les composants requis, puis réexécutez le programme d’installation.

3. Lisez le contrat de licence End-User. Si vous acceptez, sélectionnez **J’accepte le contrat**, puis sélectionnez **suivant**.
4. Dans la page **choisir le type d’installation** , sélectionnez **standard**.
5. Sur la page **prêt pour l’installation** , vous pouvez activer ou désactiver la télémétrie et les vérifications automatiques des mises à jour à chaque démarrage de l’outil. Cliquez sur **Installer** pour commencer l'installation.

> [!IMPORTANT]
> Désinstallez toutes les versions antérieures de SSMA pour DB2 avant d’installer la nouvelle version.

L’emplacement d’installation par défaut est `C:\Program Files\Microsoft SQL Server Migration Assistant for DB2`.

## <a name="see-also"></a>Voir également

- [Installation des composants SSMA sur SQL Server](../../ssma/db2/installing-ssma-components-on-sql-server-db2tosql.md)
- [Migration de bases de données DB2 vers SQL Server](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)
